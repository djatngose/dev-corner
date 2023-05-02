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