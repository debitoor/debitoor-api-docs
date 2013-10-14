#Debitoor API

Our RESTful API lets you interact with Debitoor using JSON over HTTPS.

The following resources will help you get up to speed.

##Getting started
Follow these three steps and you'll hopefully be up and running in no time. (If not, please [contact us](mailto:techteam@debitoor.com) - we'd be happy to help):

1. [Register your application](#registration) to give your app access to the API.

2. [Authenticate](#authentication): Go through the OAuth 2.0 authentication and authorization flow to obtain an `access_token`.

3. [Start interacting](#debitoor-api-endpoints) and build cool stuff!

## Registration
In order to interact with the API you first need to register an application. You can do this from the [Settings page](https://app.debitoor.com/account/settings) inside the app. This will give you a `client_id` and a `client_secret` which are used to authenticate your application during the OAuth 2.0 flow.

You can register as many applications as you like.

## Authentication
The Debitoor API supports the OAuth 2.0 protocol for authentication and authorization. You can read more about setting up OAuth in our [Authentication Guide](https://github.com/e-conomic/debitoor-api/blob/master/pages/authentication.md).

##Debitoor API Endpoints

The API is accessed via `https://api.debitoor.com` and speaks JSON over HTTPS.

All endpoints are documented in detail here:

> [https://api.debitoor.com/api](https://api.debitoor.com/api)

If you've already obtained an `access_token` you can use the following call to fetch a list of your customers:

```sh
curl -i https://api.debitoor.com/api/v1.0/customers?access_token=YOUR_ACCESS_TOKEN
```

##General Pointers

- **HTTPS only**: All requests are done over https.
- **UTF-8 Encoding**: All strings are sent and received in UTF-8.
- **JSON in, JSON out**: Data is sent and received as JSON. (Although some endpoints return csv or pdf documents).
- **Schemas**: We're using the [JSON schema](http://json-schema.org/) format to document the [endpoints](https://api.debitoor.com/api).
- **ISO Date format**: Dates and DateTimes are in ISO 8601 format:
  - Date: `2013-07-18`
  - DateTime: `2013-07-18T19:30:06.445Z`

##HTTP Methods

We're trying to keep our URLs as RESTful as possible. This means that every resource endpoint may support one or more of the following HTTP methods:

- `GET` Fetch information about a resource.
- `POST` Create a resource.
- `PUT` Full update of a resource.
- `PATCH` Partial update of a resource; only include the properties to be updated.
- `DELETE` Delete a resource.

The [endpoint documentation](https://api.debitoor.com/api) explains which methods are supported by the different endpoints.


##Error Handling

The API uses standard HTTP error codes to indicate that a request has gone wrong:

| Code | Description |
|------|-------------|
| 400  | You're sending invalid data. The response will include a description of what's wrong. |
| 401  | Not authorized: You've either sent an invalid token or no token at all. |
| 404  | Not found: The resource you requested could not be found. |
| 412  | There was a specific problem with the data you sent. More information is included in the response. |
| 500  | Server error: Something went wrong on our side. |

Additionally the body of the response always contains an error object describing the problem in more detail.

##Testing the API

If you want to test your API calls we recommend using curl.
Additionally the API is CORS enabled, so you can also use your browser as a sandbox for experimenting.

To further help you get started we've written a small sample application that demonstrates how to wire up the OAuth 2.0 flow.
The source for the app can be found on [github](https://github.com/e-conomic/debitoor-oauth-sample) and you can give it a spin [here](https://s3-eu-west-1.amazonaws.com/debitoor-oauth-sample/index.html).
