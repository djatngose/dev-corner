
# Set up web api with authentication(Openid) and authorzation(Oauth)
- Packages
```c#
       <PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="7.0.0" />
        <PackageReference Include="Microsoft.AspNetCore.Authentication.OpenIdConnect" Version="7.0.0" />
```
```c#
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.Identity.Web;

var builder = WebApplication.CreateBuilder(args);
var builder = WebApplication.CreateBuilder(args);
    JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

    builder.Services.AddAuthentication(config =>
    {
        config.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        config.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
     .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme)
     .AddOpenIdConnect(OpenIdConnectDefaults.AuthenticationScheme, options =>
        {
            // this is my Authorization Server Port
            options.Authority = "https://localhost:7275";
            options.ClientId = "platformnet6";
            options.ClientSecret = "123456789";
            options.ResponseType = "code";
            options.CallbackPath = "/signin-oidc";
            options.SaveTokens = true;
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuerSigningKey = false,
                SignatureValidator = delegate(string token, TokenValidationParameters validationParameters)
                {
                    var jwt = new JwtSecurityToken(token);
                    return jwt;
                },
            };
        });


    app.UseAuthentication();
    app.UseAuthorization();

app.MapControllers();

app.Run();
```
# The Authorization Server Project

```c#
System.IdentityModel.Tokens.Jwt
```
- https://dev.to/mohammedahmed/build-your-own-oauth-20-server-and-openid-connect-provider-in-aspnet-core-60-1g1m