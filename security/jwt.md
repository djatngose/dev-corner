# Signed Jwt
Signed JWTs are constructed from three different parts: the `header, the payload, and the signature.`

`These three parts are encoded separately`.

The signature is used to ensure that the token has not been tampered with and that it has been issued by a trusted party.

As such, it is possible to `remove the signature and then change the header to claim the JWT is unsigned`. Careless use of certain JWT validation libraries can result in unsigned tokens being taken as valid tokens, which may allow an attacker to modify the payload at his or her discretion. This is easily solved by making sure that the application that performs the validation does not consider unsigned JWTs valid.

```js
// header
{
  "alg": "RS256",
  "typ": "JWT"
}

//payload
{
  "jti": "gabnz5y54tgwjdahjqkf5rxfir",
  "sid": "f2c1e70dbac04cdbbb56a33b99bcd45c",
  "sub": "f2c1e70dbac04cdbbb56a33b99bcd45c",
  "name": "Dat Ngo",
  "email": "therocknpd+dev1@gmail.com",
  "email-verified": "1",
  "nbf": 1681896516,
  "exp": 1681900116,
  "iat": 1681896516,
  "iss": "urn:pie:angstroem",
  "aud": "urn:pie"
}
// verify signature
RSASHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  {Your-256-bit-secret} aka `Public Key in SPKI, PKCS #1, X.509 Certificate, or JWK string format, Private Key in PKCS #8, PKCS #1, or JWK string format. The key never leaves your browser.`

)
```
[''](../security/jwt-structure.png)
# sub
The subject claim usually identifies one of the parties to the other `(think of user IDs or emails)`

It is not a requirement that this claim be unique.


# what JwtSecurityTokenHandler.ValidateToken do?
The `ValidateToken` method takes in a string representation of a JWT, a TokenValidationParameters object that contains the parameters used to validate the token, and an out parameter of type SecurityToken. The method then validates the JWT by checking its signature, expiration time, and other relevant properties against the provided parameters. If the token is valid, the method returns a ClaimsPrincipal object that represents the authenticated entity associated with the token.

```c#
string jwt = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c";

var tokenHandler = new JwtSecurityTokenHandler();
var validationParameters = new TokenValidationParameters
{
    ValidateIssuerSigningKey = true,
    IssuerSigningKey = new SymmetricSecurityKey(Encoding.ASCII.GetBytes("MySuperSecretKey")),
    ValidateIssuer = false,
    ValidateAudience = false,
    ClockSkew = TimeSpan.Zero
};

SecurityToken validatedToken;
var claimsPrincipal = tokenHandler.ValidateToken(jwt, validationParameters, out validatedToken);

```
- The `IssuerSigningKey` property specifies the secret key used to sign the token
- `ValidateIssuerSigningKey` property indicates that the signature should be validated.
-  The `ValidateIssuer` and `ValidateAudience` properties are set to false because we are not validating the issuer or audience of the token.
-  the `ClockSkew` property is set to zero to ensure that the token's expiration time is checked strictly.
-  If `RequireExpirationTime` is set to true (the default value), the token's expiration time (exp claim) must be present in the token and must not have already passed for the token to be considered valid. If the expiration time is missing or has already passed, the JwtSecurityTokenHandler.ValidateToken method will throw a SecurityTokenExpiredException.

# Token-based auth flow
`JSON Web Token (JWT)` is a specific implementation of token-based authentication. Token-based authentication refers to the practice of using tokens to authenticate and authorize users in a system.

`Token-based authentication` involves the following steps:

`User authentication`: The user provides their credentials (such as username and password) to the server.
`Server validation`: The server verifies the user's credentials and generates a token.
`Token issuance`: The server issues a token to the user, typically in the form of a JSON Web Token (JWT).
`Token storage`: The user stores the token, often in client-side storage (such as local storage or cookies).
`Token inclusion`: The user includes the token in subsequent requests to the server, typically in the Authorization header.
`Token verification`: The server verifies the token's authenticity, integrity, and expiration to determine if the user is authenticated and authorized.
JWT is a type of token commonly used in token-based authentication. It is a compact, self-contained token that contains information about the user and additional metadata. A JWT consists of three parts: a header, a payload, and a signature. The header specifies the type of token and the cryptographic algorithm used for signing and verifying the token. The payload contains claims, which are statements about the user and additional data. The signature ensures the integrity of the token and allows the server to verify its authenticity.

`Token-based authentication offers several advantages over traditional session-based authentication`. It allows for stateless authentication, meaning the server does not need to store session information, making it more scalable and suitable for distributed systems. Tokens can be used across multiple applications and services, facilitating single sign-on (SSO) scenarios. Additionally, token-based authentication is well-suited for APIs and mobile applications.

While JWT is a popular implementation of token-based authentication, there are other token formats and protocols used in different systems, such as OAuth 2.0 and OpenID Connect. These protocols define additional mechanisms for authorization and delegation, allowing users to grant permissions to third-party applications without sharing their credentials directly.

In summary, JWT is a specific type of token used in token-based authentication, which is a broader concept referring to the practice of using tokens for authentication and authorization. Token-based authentication offers flexibility, scalability, and security benefits compared to traditional session-based authentication.

# cookie session vs jwt?

Both cookies/sessions and JWTs (JSON Web Tokens) can be used for authentication and session management, but they have some differences in terms of security, scalability, and implementation.

`Cookies` and sessions are widely used in web applications for session management. When a user logs in, the server generates a unique session ID and stores it in a cookie on the client-side. The client sends this cookie with each request, and the server checks the session ID to identify the user and retrieve their session data. Sessions can store sensitive data such as user ID, permissions, and authentication status. Sessions rely on the server to manage the session data, so they can be more secure than client-side tokens. However, `sessions can be vulnerable to session hijacking`, where an attacker steals the session ID and impersonates the user.

`JWTs` are a popular alternative to cookies/sessions for authentication and authorization. JWTs are self-contained tokens that store user data and an expiration time in a compact and portable format. The server generates a JWT after a successful login, and the client sends the JWT with each request in the Authorization header. The server verifies the JWT signature and decodes the payload to authenticate and authorize the user. JWTs are stateless, so they can be more scalable and performant than sessions. However, JWTs can be vulnerable to attacks such as token theft, token replay, and token tampering if not implemented securely.

In summary, cookies/sessions and JWTs have their strengths and weaknesses, and the choice depends on the specific use case and security requirements. It's essential to use best practices and security measures such as HTTPS, secure cookie flags, CSRF protection, token encryption, and token rotation to prevent attacks and secure user data.

`Statefulness`: Cookie-based sessions are stateful because the server needs to maintain session data on the server-side. The session ID is stored in a cookie on the client-side, and the server uses it to retrieve session data. In contrast, JWT is stateless as the token itself contains the necessary information for authentication and authorization. The server doesn't need to store any session data.

`Server Load`: With session-based authentication, the server needs to store session data for each user, which can consume server resources, especially in scenarios with many concurrent users. `JWTs, being stateless, can reduce the server load as there is no need to store and manage session data on the server.`

`Scalability`:` JWTs are more scalable in distributed systems or microservice architectures since they don't rely on server-side session storage. Each service or component can independently verify the JWT without any dependency on shared session storage. This makes JWTs more suitable for large-scale and distributed applications.`

`Cross-Origin Requests`: Cookies are automatically sent by the browser with every request to the same domain. This can simplify handling cross-origin requests as cookies are automatically included. JWTs can be used for cross-origin requests by including them in the request headers, typically using the Authorization header with the Bearer scheme.

`Token-based Communication`: JWTs can be used beyond just authentication and authorization. They can serve as self-contained tokens for conveying additional information or as a means of communication between different systems or services. This flexibility makes JWTs useful in scenarios where the authentication mechanism needs to integrate with other systems or carry additional data.

`Security Considerations`: Both session-based authentication and JWT-based authentication can be secure if implemented correctly. However, they have different security considerations. Session-based authentication requires protecting the session ID from attacks like session hijacking or fixation. JWT-based authentication requires ensuring the confidentiality and integrity of the tokens, protecting against token tampering or replay attacks, and securely handling the token's signing keys.