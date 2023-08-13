# How pcke flow works?

Proof-Key for Code Exchange (PKCE)
PKCE allows your authorization server to match a token request to an authorization request when exchanging an authorization code. PKCE enables this using the following flow:

The client application generates a random value, hashes it, and then sends the hash in the authorization request as the code_challenge parameter
The authorization server handles user authentication and consent, issues an authorization code, and remembers the code_challange for later
The client application sends the original, unhashed random value in the token request as the code_verifier parameter
The authorization server hashes the code_verifier and compares it to the code_challenge from the original authorization request
If the values match, the token request succeeds, and the client is issued tokens
If the values do not match, the token request fails, and the client does not get any tokens
PKCE allows the authorization server to verify that it’s the same entity swapping the authorization code as the one who asked for the code, as only they would know that original, plaintext proof-key. It prevents stolen authorization codes from being injected into the client application by an attacker.

It allows the authorization server to ask, “Is the app that is trying to swap the code for a token the same application that I sent it to? Is it as a result of the correct authorization request?”. If an attacker steals an authorization code, then this verification is vital. Client authentication alone wouldn’t help you here.
```js
https://estorelogisticsb2c.b2clogin.com/estorelogisticsb2c.onmicrosoft.com/B2C_1_API_SignIn/oauth2/v2.0/authorize?client_id=19eff4eb-c805-4b5b-a535-1fce9a1aef2c&response_type=code&redirect_uri=https%3A%2F%2Fqa-web-express-esl.azurewebsites.net&response_mode=query&scope=19eff4eb-c805-4b5b-a535-1fce9a1aef2c%20offline_access&state=25cda83c-0350-4a02-a5b9-c89bb95dae9e&code_challenge=nU_JGgwyuVxqS7kEnBMtHfzcJRJLNWoF5K-s3l17f0A&code_challenge_method=S256

client_id:19eff4eb-c805-4b5b-a535-1fce9a1aef2c
response_type:code
redirect_uri:https%3A%2F%2Fqa-web-express-esl.azurewebsites.net
response_mode:query
scope:19eff4eb-c805-4b5b-a535-1fce9a1aef2c%20offline_access
state:25cda83c-0350-4a02-a5b9-c89bb95dae9e
code_challenge:nU_JGgwyuVxqS7kEnBMtHfzcJRJLNWoF5K-s3l17f0A
code_challenge_method:S256
```

# Steps

1. The user clicks Login within the application.

2. Auth0's SDK creates a cryptographically-random code_verifier and from this generates a code_challenge.

3. Auth0's SDK redirects the user to the Auth0 Authorization Server (
/authorize
endpoint) along with the code_challenge.

4. Your Auth0 Authorization Server redirects the user to the login and authorization prompt.

5. The user authenticates using one of the configured login options and may see a consent page listing the permissions Auth0 will give to the application.

6. Your Auth0 Authorization Server stores the code_challenge and redirects the user back to the application with an authorization code, which is good for one use.

7. Auth0's SDK sends this code and the code_verifier (created in step 2) to the Auth0 Authorization Server (
/oauth/token
endpoint).

8. Your Auth0 Authorization Server verifies the code_challenge and code_verifier.

9. Your Auth0 Authorization Server responds with an ID token and access token (and optionally, a refresh token).

10. Your application can use the access token to call an API to access information about the user.

The API responds with requested data.
## PKCE on the Server
PKCE was initially designed to prevent code theft on native applications, helping to mitigate the downsides of native apps not being able to keep a secret. However, it turns out that PKCE is also useful for other application types, even client applications that can keep a secret, as it gives the authorization server a method of detecting code theft that it would otherwise never have.

PKCE is now a recommendation for server-side applications in OAuth 2.1, clarified with:

 Historic note: Although PKCE [RFC7636] was originally designed as a mechanism to protect native apps, this advice applies to all kinds of OAuth clients, including web applications and other confidential clients. 

## PKCE vs. OpenID Connect nonce
PKCE is similar to OpenID Connect’s nonce validation, but in this case, it is the authorization server that is doing the validation, preventing the generation of tokens rather than the client application rejecting invalid tokens. You can also use PKCE in pure OAuth flows, rather than relying on the use of OpenID Connect and identity tokens.

For a complete analysis of the differences between PKCE and nonce, check out Daniel Fett’s article on the topic.

“Do I still need a client secret when using PKCE?”
Yes, assuming you can keep a secret.

PKCE helps protect you against various code injection attacks, but PKCE does not replace client authentication.

With PKCE, you prove that the same application is swapping the code as the one who requested it.

With client authentication, you prove that the application is even allowed to swap the code.

PKCE is not a replacement for client secrets. It is a mitigation against stolen authorization codes that is particularly useful when a client application cannot keep a secret. It’s a bit like a Cross-Site Request Forgery (CSRF) token on a login page. The CSRF token allows you to validate that the user submitted the form on a page you created; however, without the user providing their credentials, you would be trusting them on username alone.
## Client Authentication with clients that cannot keep a secret
Is there any point to client authentication if a client application cannot keep a secret? For example, a Single Page Application (SPA) running in the browser would need to have the plaintext credentials in the end-users browser, and a mobile app would need to store it on the end-users phone. These are known as public clients, and this is when you would use PKCE without a client secret.

You could embed some client credentials with the approach of “Why make it easy for them? It’s another hurdle for the attacker”, but when it’s the same credentials across all instances of that client application, then I would say the benefits are negligible. It’s unlikely that anything will go seriously wrong if you do this; just don’t trust it on its own and accept that you’ll have pentesters and bug bounty hunters pointing it out to you every 5 minutes.

A client secret specific to that instance of the client application would be better. You could generate a secret as part of a bootstrapping process such as dynamic client registration. In this case, the public client becomes a credentialed client, a client that has a secret but who cannot be trusted based on the secret alone.

It’s not uncommon to have the OAuth functionality delegated onto a secure server for these kinds of applications. For example, by using a Backend for Frontend with SPAs or token exchange for code swaps on mobile apps when PKCE is not supported.
https://www.scottbrady91.com/oauth/client-authentication-vs-pkce