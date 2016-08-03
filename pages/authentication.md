# OAuth 2.0 Authentication

The Debitoor API supports the [OAuth 2.0](http://oauth.net/2/) protocol for authorizing third-party websites to connect without the need for those sites to know the user's username and password.

As a developer you need to [register your app](https://github.com/debitoor/debitoor-api-docs#registration) with Debitoor before you can use the OAuth flow.

# Web App Flow

Follow the steps below to retrieve an API `access_token` for your web app using OAuth 2.0. An access token allows your app to make requests to Debitoor on behalf of a user.

## 1. Redirect the user to the Debitoor authorization page
From your web app, add a link to the following page:

```plain
GET https://app.debitoor.com/login/oauth2/authorize?client_id=YOUR_CLIENT_ID&response_type=code
```

**Request parameters:**

- **client_id** (required): The client ID you received from Debitoor when you registered your app.
- **response_type** (required): The type of response. For the web flow, use the value `code`.
- **redirect_uri** (optional): URL-encoded URL in your app that users will be directed to following authorization. If not specified, the user will be sent to the callback URL you specified when registering your app. Note that the host and port of the redirect_uri must be exactly the same as the app's callback URL.
- **lang** (optional): Language code that determines the language of the authorization page. By default set to en-US.

**Schema:**

[/schemas/login/oauth2/authorize](https://app.debitoor.com/api/schemas/login/oauth2/authorize/v1)

**Response:**

The user will see the authorization prompt, asking if your app should be allowed access to the user's data.

## 2. Debitoor redirects the user back to your site

If the user accepts your request, Debitoor redirects back to your site with a temporary authorization `code` in the query string:

```plain
https://YOUR_REGISTERED_CALLBACK/?code=CODE
```

## 3. Receive an access token in exchange for your code

After receiving the `code` your website passes back the `code` to Debitoor. In return you will obtain an `access_token`. To perform this exchange, make a POST to:

```plain
POST https://app.debitoor.com/login/oauth2/access_token
```

**Request parameters:**

- **client_secret** (required): The client secret you received from Debitoor when you registered your app.
- **code** (required): The code you received as a response in the previous step.
- **redirect_uri** (required): The URL in your app that users will be directed to following authorization. Note that the host and port of the redirect_uri must be exactly the same as the app's callback URL.

**Schema:**

[/schemas/login/oauth2/access_token](https://app.debitoor.com/api/schemas/login/oauth2/access_token/v1)

**Response:**

The response will include a JSON string containing the `access_token`:

```js
{ access_token: 'ACCESS_TOKEN' }
```

You should save this token and use it to make API requests on behalf of the user. The token will not expire, but can be revoked by the user from within the Debitoor application.

# Using the Access Token

Our API is JSON-based. You can view all available endpoints in our [API Documentation](https://api.debitoor.com/api).

The token should be included in each API request. This can be done either in the query string:

```plain
curl https://api.debitoor.com/api/countries/v1?token=ACCESS_TOKEN
```

Or as an `x-token` HTTP header:

```plain
curl -H "x-token: ACCESS_TOKEN" https://api.debitoor.com/api/countries/v1
```

# OAuth Sample App

We've put together a small sample app that demonstrates how to set up the OAuth 2.0 authentication. You can find the [source for the app](https://github.com/debitoor/debitoor-oauth-sample) on github. Feel free to [give it a spin](https://s3-eu-west-1.amazonaws.com/debitoor-oauth-sample/index.html)!
