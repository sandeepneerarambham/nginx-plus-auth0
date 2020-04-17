# NGINX+ with Auth0 for authentication

Use Auth0 as an OIDC authentication server to secure applications secured by NGINX+.

This repo is based on the [official NGINX+ OIDC library](https://github.com/nginxinc/nginx-openid-connect), where you can find fuller documentation.

Upon successful authentication, NGINX+ will receive an id_token from Auth0. NGINX+ will parse the id token and add some relevant user attributes as headers to the request it passes to the downstream application.

By default, this setup proxies a public web application: [http://auth0-headers.herokuapp.com/](http://auth0-headers.herokuapp.com/). The app just displays headers, so you can visit it now to see what it looks like without authentication and proxy by NGINX+. After you authenticate with Auth0, the app will display the additional headers it has parsed from the Auth0 id_token.

## Setup and installation

A sample nginx.conf file is included in this repo. These are some of the essential parameters that need to be updated to enable authentication with Auth0. Update these example values with values from your own Auth0 tenant:

```
set $oidc_jwt_keyfile "https://playground-workshop.au.auth0.com/.well-known/jwks.json"; # URL when using 'auth_jwt_key_request'
set $oidc_logout_redirect "/_logout"; # Where to send browser after requesting /logout location
set $oidc_authz_endpoint "https://playground-workshop.au.auth0.com/authorize";
set $oidc_token_endpoint "https://playground-workshop.au.auth0.com/oauth/token";
set $oidc_client "HRILebBAe6iMprFKUvFOC1MBSeKl6Q0K";
set $oidc_client_secret "{{your_client_secret}}";
set $oidc_hmac_key "uniquevalue"; # This should be unique for every NGINX instance/cluster
```

There are of course many other parameters that you can adjust.

### Auth0 setup
## Create a free account in Auth0

1. Go to [Auth0](https://auth0.com) and click Sign Up.
2. Use Google, GitHub or Microsoft Account to login.

To set up your OIDC client in Auth0, follow the quickstart guide [here](https://auth0.com/docs/quickstart/webapp/nodejs/01-login) 

You need to add the redirect_uri (http://localhost:8126/redirect_uri) to your OIDC client, and your hostname (http://localhost:8126) to the Trusted Origins for your Auth0 tenant. I am just using 8126 as an example port; you can use any port you wish.

Associate user application with a database and add a user to your Auth0 tenant so you can test authentication.

After you have updated your `nginx.conf` file with the settings from your Auth0 tenant, you can restart NGINX+ and requests to protected endpoints will be redrected to Auth0 for authentication.

### Authentication
When you authenticate, NGINX will parse the id_token it receives from Auth0, create some new headers, and load the downstream site, which just displays headers.

## What is Auth0?

Auth0 helps you to:

* Add authentication with [multiple authentication sources](https://docs.auth0.com/identityproviders), either social like **Google, Facebook, Microsoft Account, LinkedIn, GitHub, Twitter, Box, Salesforce, amont others**, or enterprise identity systems like **Windows Azure AD, Google Apps, Active Directory, ADFS or any SAML Identity Provider**.
* Add authentication through more traditional **[username/password databases](https://docs.auth0.com/mysql-connection-tutorial)**.
* Add support for **[linking different user accounts](https://docs.auth0.com/link-accounts)** with the same user.
* Support for generating signed [Json Web Tokens](https://docs.auth0.com/jwt) to call your APIs and **flow the user identity** securely.
* Analytics of how, when and where users are logging in.
* Pull data from other sources and add it to the user profile, through [JavaScript rules](https://docs.auth0.com/rules).

