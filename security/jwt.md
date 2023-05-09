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
  
Public Key in SPKI, PKCS #1, X.509 Certificate, or JWK string format.
,
  
Private Key in PKCS #8, PKCS #1, or JWK string format. The key never leaves your browser.

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