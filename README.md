#Debitoor API

Our RESTful API lets you interact with Debitoor using JSON over HTTPS.

The following resources will help you get up to speed.

##Getting Started
Follow these three steps and you'll hopefully be up and running in no time:

1. [Register your app](#registration) to allow your app to access the Debitoor API.

2. [Authenticate your app](#authentication) by going through the OAuth 2.0 authentication and authorization flow to obtain an `access_token`.

3. [Start interacting](#debitoor-api-endpoints) and build cool stuff!

If you have any questions or need help with these steps, please [contact us](mailto:techteam@debitoor.com). We'll be happy to help.

## Registration
To interact with the API, you first need to register your app. You can do this from the [Settings page](https://app.debitoor.com/account/settings) inside the Debitoor application. This will give you a `client_id` and a `client_secret` which are used to authenticate your app during the OAuth 2.0 flow.

You can register as many apps as you like.

## Authentication
The Debitoor API supports the OAuth 2.0 protocol for authentication and authorization. You can read more about setting up OAuth in our [Authentication Guide](https://github.com/debitoor/debitoor-api-docs/blob/master/pages/authentication.md).

##Debitoor API Endpoints

The API is accessed via `https://api.debitoor.com` and speaks JSON over HTTPS.

All endpoints are detailed in the API Documentation:

> [https://api.debitoor.com/api](https://api.debitoor.com/api)

If you've already obtained an `access_token` you can use the following call to retrieve a list of customers:

```sh
curl -i https://api.debitoor.com/api/customers/v1?access_token=YOUR_ACCESS_TOKEN
```

##General Pointers

- **HTTPS only**: All requests are done over HTTPS.
- **UTF-8 encoding**: All strings are sent and received in UTF-8.
- **JSON in, JSON out**: Data is sent and received as JSON (although some endpoints return CSV or PDF documents).
- **Schemas**: We're using the [JSON schema](http://json-schema.org/) format to document the [endpoints](https://api.debitoor.com/api).
- **ISO Date format**: Dates and DateTimes are in ISO 8601 format:
  - Date: `2013-07-18`
  - DateTime: `2013-07-18T19:30:06.445Z`

##HTTP Methods

We're trying to keep our URLs as RESTful as possible. This means that every resource endpoint may support one or more of the following HTTP methods:

- `GET` Retrieves information about a resource.
- `POST` Creates a resource.
- `PUT` Fully updates of a resource.
- `PATCH` Partially updates a resource; only include the properties to be updated.
- `DELETE` Deletes a resource.

The [API Documentation](https://api.debitoor.com/api) lists which methods are supported by the different endpoints.


##Error Handling

The API uses standard HTTP error codes to indicate that a request has gone wrong:

| Code | Description |
|------|-------------|
| 400  | You're sending invalid data. The response will include a description of what's wrong. |
| 401  | Not authorized: You've either sent an invalid token or no token at all. |
| 404  | Not found: The resource you requested could not be found. |
| 412  | There was a specific problem with the data you sent. More information is included in the response. |
| 500  | Server error: Something went wrong on our side. |

Additionally, the body of the response always contains an error object describing the issue in more detail.

##Testing the API

If you want to test your API calls we recommend using curl.
Additionally the API is CORS-enabled, so you can also use your browser as a sandbox for experimenting.

To further help you get started we've written a small sample app that demonstrates how to set up the OAuth 2.0 flow. You can find the [source for the app](https://github.com/debitoor/debitoor-oauth-sample) on github. Feel free to [give it a spin](https://s3-eu-west-1.amazonaws.com/debitoor-oauth-sample/index.html)!

## Node.js Helper
We've built a thin wrapper using node.js that will make it even easier to work with the API.
If you're re into that sort of thing, go [read more about it here](https://github.com/debitoor/node-debitoor).
