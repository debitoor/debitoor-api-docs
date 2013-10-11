# OAuth2 Authentication

The Debitoor API supports the [OAuth 2.0](http://oauth.net/2/) protocol for authorizing third party web sites to connect without the need for those sites to know the user's username and password.

Developers need to [register their application](https://github.com/e-conomic/debitoor-api#registration) with Debitoor before they can use the OAuth flow.

# Web Application Flow

This describes how you can retrieve an API `access_token` from your web app using OAuth 2.0. An access token allows your app to make requests to Debitoor on behalf of a user.

## 1. Redirect user to the Debitoor authorize page:

```plain
GET https://api.debitoor.com/login/oauth2/authorize?client_id=YOUR_CLIENT_ID&response_type=code
```

**Request Parameters:**

- **client_id** (required) - The client ID you received from Debitoor when you registered your app.
- **response_type** (required) - The type of response. For web flow use the value `code`.
- **redirect_uri** (optional) - URL encoded URL in your app where users will be sent after successful authorization. If not supplied the user will be sent to the callback URL you specified when registering your app. Note that the host and port of the redirect_uri must be exactly the same as the app's callback URL.
- **lang** (optional) - Language Code. Determines the language of the authorize page. Defaults to en-US.

**Schema:**

[/schemas/login/oauth2/authorize](https://api.debitoor.com/api/v1.0/schemas/login/oauth2/authorize)

**Response:**

The user will see the authorization prompt, asking him to allow your app to access his data.

## 2. Debitoor redirects user back to your site

If the user accepts your request, Debitoor redirects back to your site with a temporary authorization `code` in the query string:

```plain
https://YOUR_REGISTERED_CALLBACK/?code=CODE
```

## 3. Exchange code to access token

After receiving the `code` your web site passes back the `code` to Debitoor. In return it will obtain an `access_token`. This exchange is done by making a POST to:

```plain
POST https://app.debitoor.com/login/oauth2/access_token
```

**Request Parameters:**

- **client_secret** (required) - The client secret you received from Debitoor when you registered your app.
- **code** (required) - The code you received as a response to the first authorize step.
- **redirect_uri** (required) - URL in your app where users will be sent after successful authorization.

**Schema:**

[/schemas/login/oauth2/access_token](https://app.debitoor.com/api/v1.0/schemas/login/oauth2/access_token)

**Response:**

The response will contain a JSON string containing the `access_token`:

```js
{ access_token: 'ACCESS_TOKEN' }
```

You should save this token and use it to make API requests on behalf of the user. The token will not expire, but can be revoked by the user from within Debitoor.

# Use the token to access the API
Our API is JSON based. You can view all of the available endpoints at our [API documentation page](https://api.debitoor.com/api).

The token should be included in each API request. This can be done either in the query string:

```plain
curl https://api.debitoor.com/api/v1.0/sales/customers?token=ACCESS_TOKEN
```

Or as an `x-token` HTTP header:

```plain
curl -H "x-token: ACCESS_TOKEN" https://api.debitoor.com/api/v1.0/sales/customers
```

# OAuth Sample App
We've put together a small sample that demonstrates how to set up the OAuth 2.0 authentication.

The source for the app can be found on [github](https://github.com/e-conomic/debitoor-oauth-sample) and you can give it a spin [here](https://s3-eu-west-1.amazonaws.com/debitoor-oauth-sample/index.html).