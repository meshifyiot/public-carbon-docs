# General

There are a few different ways to authenticate via the API. User Membership (username/password), user token (token id, token secret), and an API key. Membership is intended for regular UI users, user tokens are intended for temporary access with user friendly keys and API keys are intended for headless users.

|                                | UI access via GET /api/login | API Access | Generated |
|--------------------------------|------------------------------|------------|------------|
| Membership (email, password)   |:heavy_check_mark:|:x:|:x:|
| User Token (access id, secret) |:heavy_check_mark::x:| :x:|
| API Key (key)                  |:x:|:heavy_check_mark:|:heavy_check_mark:


The user token and membership are allowed to be used as a login for the UI via the regular login page. The API key is not to be used for the UI and is for accessing the API directly. When logging into the UI, a cookie with related session info is returned and used to authenticate each request after that.

NOTE: we will never show the user the secret in plain text. The only exception is for the API key during creation we will return it via the post response. That will be the only time that the api key can be retrieved. The user token and password secrets will never be readable via our system.

## Authorization Header Format

All of the authentication methods are authorized very similarly via the `Authorization` header. Each type has a different prefix key that is used to identify the type of authentication you are using. The standard header value format is something like `{tokenPrefix} base64encode({id} + ':' + {secretkey})`.

If there is no separate id, secret key as with API keys, the header will just be the token prefix and the base64encoded value.

For the examples below in javascript we will be using base64 encoding function that handles utf-16 characters such as this function:

```javascript
function b64EncodeUnicode(str) {
    return btoa(encodeURIComponent(str).replace(/%([0-9A-F]{2})/g,
        function toSolidBytes(match, p1) {
            return String.fromCharCode('0x' + p1);
    }));
}
```

### Membership

The auth header value template is `Basic base64encode(userEmail + ':' + password)`

```javascript
var userEmail = 'somebody@example.com'
var password = `sooper sekrat`
var authValue = `UserToken ` + b64EncodeUnicode(userEmail + ':' + password)
console.log(authValue) // TODO
```

### UserToken

The auth header value template is `UserToken base64encode(accessId + ':' + secret)`

```javascript
var accessId = '123456789'
var secret = `sooper sekrat`
var authValue = `UserToken ` + b64EncodeUnicode(accessId + ':' + secret)
console.log(authValue) // TODO
```

### Api Key

The auth header value template is `Bearer base64encode(apiKey)`

```javascript
var apiKey = '123456789'
var authValue = `Bearer ` + b64EncodeUnicode(apiKey)
console.log(authValue) // TODO
```

## Notes About Enabling Authentication For Users

Permissions for authentication related methods are slightly granular meaning there is a modify own permission and a modify others permission. The modify own permission will allow a user to create or delete that users authentication method, while the modify others will allow that user to modify other users authentications. These should be carefully used as you could enable a user to delete their own UI login and lock themselves out of the system or potentially have access to create/modify other user's authentication methods.

The different authentication methods and their documentation can be found [here](http://carbon-docs.meshify.com/#!/authentication/authentication_create_api_key). When using these methods you must specify the user you wish to modify via a request param `userId` otherwise it will be defaulted to your own user's id. Each user can have multiple user tokens and api keys but only one membership.