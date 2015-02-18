# Microsoft Account Windows Forms

This library provides an easy way to generate Microsoft Account access and 
refresh tokens from a Windows Forms application.

Before using this library, you need to register your application on the 
[Microsoft Account developer center](https://account.live.com/developers/applications/).
Your application needs to be configured as a Mobile or Desktop Client application on the
API settings page for this library to work.

To retrieve a one-time access token:
```csharp
using MicrosoftAccount.WindowsForms;

public async void Authenticate()
{
  string accessToken = await MicrosoftAccountOAuth.LoginOneTimeAuthorizationAsync("client_id", 
    new string[] { "wl.signin", "wl.basic" });
}
```

To retrieve a refresh token along with your access token to enable your app to stay
signed-in for longer than the 1 hour access token validitity period:

```csharp
using MicrosoftAccount.WindowsForms;

public async void Authenticate()
{
  var token = await MicrosoftAccountOAuth.LoginAuthorizationCodeFlowAsync("client_id",
    "client_secret", new string[] { "wl.offline_access", "wl.basic", "wl.signin" });
  
  string accessToken = token.AccessToken;
  string refreshToken = token.RefreshToken;
  
  // Store these values somewhere useful. Treat them like passwords!
}
```

After the access token has expired, you can redeem the refresh token for a new set of
tokens using `RedeemRefreshTokenAsync()`:

```csharp
public async void RenewAuthentication()
{
  var token = await MicrosoftAccountOAuth.LoginAuthorizationCodeFlowAsync("client_id",
    "client_secret", "refresh_token");
  
  string accessToken = token.AccessToken;
  string refreshToken = token.RefreshToken;
  
  // Store these values somewhere useful. Treat them like passwords!
}
```
