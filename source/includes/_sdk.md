# Vanilla SDK

The primary functionality in the Backand SDK is contained in the Vanilla SDK, located [on our GitHub page](https://github.com/backand/vanilla-sdk). This provides a global object - `backand` - that can be used to work with the Backand SDK. Review the ReadMe on the GitHub repo to get started, or navigate to our [SDK Documentation](#sdk-documentation).

## Overview

This is the documentation for Back&'s Vanilla SDK. This encapsulates all common API functionality. All of our other SDKs wrap this core set of function calls, which in turn provides programmatic access to your app's back end.

## Installation

<aside class="notice">In most cases, the Vanilla SDK will be installed as a part of another, platform-specific SDK. However, if you wish to include it directly in your project, follow these instructions</aside>

To install the Vanilla SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/vanilla-sdk |
| yarn | $ yarn add @backand/vanilla-sdk |
| bower | $ bower install backand-vanilla-sdk |
| clone/download via Git | $ git clone https://github.com/backand/vanilla-sdk.git |

## Import
```javascript

<!-- Includes for index.html, or appropriate location -->
<script src="node_modules/@backand/vanilla-sdk/dist/backand.min.js"></script>
<script src="backand.min.js"></script>

// Import the Backand SDK object
import backand from '@backand/vanilla-sdk'
const backand = require('@backand/vanilla-sdk')

```
To work with the SDK, you need to first import it into your project. This requires including the JavaScript source in your project in `index.html` (or another appropriate location), and then importing the `backand` object into your application. The `backand` object is the root of the SDK, and will be used as the core of all calls to your Backand application.

## Quick Start
```shell
# Use a bearer token to retrieve items. Obtain the token from calling the /token endpoint
curl https://api.backand.com/1/objects/items -H "Authorization: Bearer OfKDiw6fQwfhmUcoh3WuXken_Sgj18za8SDqn1Ed2S_qI88HWNwGBSpG2ktUkSbNIwcGn6dwos7ivvVARC1UIiCf8fzlNynFNZamCZPDpbOhRjheq3RnU6HJiCbqlks57PLvMnf9PxGK8D3FU8MeaPyUk7mmbAtvgc6GF-9s13YpFjVQkM5XRZ-CsuWjaRoukygLMOivj1iPYxBB4c-hu6ocZKQljiSnw6rroYbD4spmUIkwDnC0rz1jP9ln5KNI3KmO0KLcWJF6E_zWfepdFw"
```
```javascript
// Initialize the SDK
backand.init({
  appName: 'APP_NAME',
  anonymousToken: 'ANONYMOUS_TOKEN'
});

// Retrieve a list of records from the server
backand.object.getList('users')
  .then((response) => {
      console.log(response);
  })
  .catch(function(error){
      console.log(error);
  });
```

Getting started with the SDK is as simple as configuring your application access details, initializing the SDK, and calling a `getList()` function for one of the objects in your system. You configure the SDK with your application's `APP_NAME` and `ANONYMOUS_TOKEN`. Once you've called `init()` with these values, the SDK will use this data to automaticaly manage authentication headers for API requests to your app's REST API.

```JSON
// Sample response
{
  "totalRows": 2,
  "data": [
    {
      "__metadata": {
        "id": "1",
        "fields": {
          "id": {
            "type": "int",
            "unique": true
          },
          "items": {
            "collection": "items",
            "via": "user"
          },
          "email": {
            "type": "string"
          },
          "firstName": {
            "type": "string"
          },
          "lastName": {
            "type": "string"
          }
        },
        "descriptives": {

        },
        "dates": {

        }
      },
      "id": 1,
      "items": null,
      "email": "matt@backand.com",
      "firstName": "Matt",
      "lastName": "Billock"
    },
    {
      "__metadata": {
        "id": "2",
        "fields": {
          "id": {
            "type": "int",
            "unique": true
          },
          "items": {
            "collection": "items",
            "via": "user"
          },
          "email": {
            "type": "string"
          },
          "firstName": {
            "type": "string"
          },
          "lastName": {
            "type": "string"
          }
        },
        "descriptives": {

        },
        "dates": {

        }
      },
      "id": 2,
      "items": null,
      "email": "start@backand.io",
      "firstName": "Start",
      "lastName": "Backand"
    }
  ]
}
```
This connects your Back& application (with the app name of *APP_NAME* and an anonymous access token of *ANONYMOUS TOKEN*) to your current project. Once the connection is configured, the SDK uses this connection information to construct HTTP requests to your API. Result data is returned in the *response* object as the member variable *data*. In the case of *getList*, this will be a JSON array of objects pulled from the *users* table in your Back& application. You can easily change the table being manipulated by replacing *users* with the name of any object in your system.

## Working with the SDK
The SDK is implemented as a wrapper around HTTP calls made to the Backand service. As a result, you can access all of Backand's functionality via cURL calls, provided you configure the header correctly. Review the section on [User Authentication and Security](#the-user-object-and-security) for more information on configuring the headers for each call. For JavaScript applications, the SDK takes care of the complexity of managing the authentication headers required to communicate with Backand. Simply utilize the built-in authentication methods to authenticate, and all future requests will include the correct headers.

The SDK is built around a global service object exported from the JavaScript called `backand`. You can use properties of this object to interact with the various endpoints. In the next several sections, we'll walk through all of the functionality offered by our SDK. View the pane on the right for sample code that can be used to call each method, as well as sample responses.



### SDK Properties - overview

Below is a list of the properties offered by the SDK, a description of the functionality handled by that property, and the list of methods provided by that property:

| Name | Methods | Description |
| ---- | ----------- | ------- |
| constants | EVENTS, URLS, SOCIALS | Provides access to constants in the SDK |
| helpers | *filter*, *sort*, *exclude*, *StorageAbstract* | Provides helper methods for working with the SDK |
| direct properties | *useAnonymousAuth*, *signin*, *signup*, *socialSignin*, *socialSigninWithToken*, *socialSignup*, *requestResetPassword*, *resetPassword*, *changePassword*, *signout*, *getSocialProviders* | These are properties available directly on the Backand SDK object, mostly focused on Authentication |
| defaults | *none* | This stores the current app's configuration in the SDK |
| object | *getList*, *create*, *getOne*, *update*, *remove*, *get* (action), *post* (action) | This encapsulates all methods used to manipulate objects |
| file | *upload*, *remove* | Provides helper methods for working with files |
| query | *get*, *post* | Allows you to work with custom query objects |
| user | *getUserDetails*, *getUsername*, *getUserRole*, *getToken*, *getRefreshToken* | Provides information on the current authenticated user |
| on | *none* | This is the event handler for socket.io functions, replacing socket.on |
| offline | `cache`, `queue`, `setOfflineMode` |  provides management for offline execution capabilities |

### Default Events
By default, the Back& SDK emits the following events that your code can respond to:

| Name    | Description           | Syntax                                                                     |
|---------|-----------------------|----------------------------------------------------------------------------|
| SIGNIN  | dispatched on signin  | window.addEventListener(backand.constants.EVENTS.SIGNIN, (e)=>{}, false);  |
| SIGNOUT | dispatched on signout | window.addEventListener(backand.constants.EVENTS.SIGNOUT, (e)=>{}, false); |
| SIGNUP  | dispatched on signup  | window.addEventListener(backand.constants.EVENTS.SIGNUP, (e)=>{}, false);  |
| START_OFFLINE_MODE  | dispatched on start offline mode  | window.addEventListener(backand.constants.EVENTS.START_OFFLINE_MODE, (e)=>{}, false);  |
| END_OFFLINE_MODE  | dispatched on end offline mode  | window.addEventListener(backand.constants.EVENTS.END_OFFLINE_MODE, (e)=>{}, false);  |

## SDK Methods
**NOTE:**
- **All Methods return a Promise -> you can work with the response using .then() and .catch()**
- **You can see the response schema [here](https://github.com/mzabriskie/axios#response-schema)**

## Root properties
The root properties on the SDK are primarily focused on authentication and session management functionality. These all hinge upon the use of the `.init()` function. This function accepts your application name, an anonymous access token, and an optional signup token by default - these three pieces of information are all you need to connect the SDK to any Backand application.

Other methods on the root of the object are focused on configuring individual platform settings, such as setting an API URL, adjusting the local storage method used, or working with our socket-based communications. Review the details for each function call below to see sample calls and responses, as well as explanations of each element's functionality.

### backand.init()
```shell
# This call has no cURL equivalent
```
```javascript
var config = {
   appName: 'APP_NAME',
   anonymousToken: 'ANONYMOUS_TOKEN'
             };
backand.init(config);
```

The *init()* method creates a new Back& SDK instance with the supplied configuration.

#### Parameters
The available parameters for the *config* parameter are:

| Param Name | Data Type | Usage | Required? | Default Value |
| ---------- | --------- | ----- | --------- | ------------- |
| **appName** | string | Sets the name of your backand app | **required** | |
| **anonymousToken** | string | Sets the anonymous token of your backand app | **required** | |
| **useAnonymousTokenByDefault** | boolean | Determines whether the sdk should use the anonymousToken when there is no other token | *optional* | *true* |
| **signUpToken** | string | Sets the signup token of your backand app | *optional* | |
| **apiUrl** | string | Sets the API url of backand servers | *optional* | *https://api.backand.com* |
| **storage** | object | Sets the storage type to use (local/session/implementation of StorageAbstract) | *optional* | *localStorage* |
| **storagePrefix** | string | Sets prefix to use in the storage structure | *optional* | *BACKAND_* |
| **manageRefreshToken** | boolean | Determines whether the sdk should manage refresh tokens internally | *optional* | *true* |
| **runSigninAfterSignup** | boolean | Determines whether the sdk should run signin after signup automatically | *optional* | *true* |
| **runSocket** | boolean | Determines whether the sdk should run socket automatically | *optional* | *false* |
| **socketUrl** | string | Sets the socket url of backand servers | *optional* | *https://socket.backand.com* |
| **isMobile** | boolean | Determines whether the sdk is part of a mobile application | *optional* | *false* |
| **mobilePlatform** | string | sets the platform used to build the mobile application ('ionic'/'react-native') | *optional* | 'ionic' |
| **runOffline** | boolean | Determines whether the sdk should run pending offline actions. (cached data and queued requests)  | *optional* | `false` |
| **allowUpdatesinOfflineMode** | boolean | Determines whether the sdk will allow object updates or deletion while in offline mode. When set to "true", all objects must contain a field, "updatedAt", that needs to be before the SDK entered offline mode. Any update/delete operations on fields with an "updatedAt" that occurs after the SDK entered offline mode will fail. | *optional* | `false` |
| **beforeExecuteOfflineItem** | function | Sets the function to be called before each cached request. In order to determines whether to dispatch the next request or drop it, you must call return with a valid boolean. | *optional* | `(request) => { return true }` |
| **afterExecuteOfflineItem** | function | Sets the function to be called after each cached request. | *optional* | `(response, request) => { }` |

### .on()
```shell
# This call has no cURL equivalent
```
```javascript
  //Wait for server updates on 'items' object
  Backand.on('items_updated', function (data) {
    //Get the 'items' object that have changed
    console.log(data);
  });
```

You can easily integrate with our Socket functionality using the *on* method. Socket signin and signout are handled automatically by the SDK.

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| eventName | string | Name of the socket event to subscribe to |
| callback | function | Callback triggered when *eventName* is received |

### .signin()
```shell
curl -X POST -d "username=<username>" -d "password=<password>" -d "appName=<app name>" -d "grant_type=password" https://api.backand.com/token
```
```javascript
backand.signin(username, password)
  .then(res => {
    console.log('signin succeeded with user:' + res.data.username);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample Response

```json
{
  "access_token": "TGvnEBfhreaVwJspg0s_uDKppvjaA9jICQGsHSocmHnt8s98_rkMQIkf4fvd25j3i5QOJKPpZTpoCpdu7duxcjsUyLRYihLBhcyiSAv6PxeY4zG6SpL5CtfDl6PSpqJx571k_fSphmIha40bb0wvks3O-21E85hV7GdD3D0TJtSR0U4twJhaJl-HJmeGCEEmfr27FFtRu2Hb9pwkCNnlUNQast4s5fTlTOEiapspgUgfWT9s4V47t5TUuD6onMtDNQmXfekr-Yj2i_Q68L8a_Q",
  "token_type": "bearer",
  "expires_in": 86399,
  "appName": "bkndionicstarter",
  "username": "john@backand.com",
  "role": "User",
  "firstName": "John",
  "lastName": "Doe",
  "fullName": "John Doe",
  "regId": 1009820,
  "userId": "29"
}
```
Signin with username and password in order to get access_token to be used in all other calls. If you don't have users you should use anonymous token only.

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| username | string | the username to authenticate |
| password | string | the user's password |

### .signup()
```shell
# Replace placeholder text with values for your user. Obtain signup token from
# the Security & Auth section in your app dashboard.
# This uses environment variables to ease presentation of the
# information, and supplies the fields as data arguments to the cURL command.
# $SIGNUP_TOKEN - the name of the backand application you are connecting to
curl https://api.backand.com/token -d username=$USERNAME -d password=$PASSWORD -d grant_type=password -d appName=$APP_NAME

curl -X POST -d "{'email':'user email','password':'user password','confirmPassword':'password confirmation','firstName':'first name','lastName':'last name'}" -H "SignUpToken: $SIGNUP_TOKEN" https://api.backand.com/1/user/signup
```
```javascript
backand.signup(firstName, lastName, email, password, confirmPassword, parameters = {})
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample Response

```json
{
  "username": "matt+test5@backand.com",
  "currentStatus": 1,
  "message": "The user is ready to sign in",
  "listOfPossibleStatus": [
    {
      "status": 1,
      "message": "The user is ready to sign in"
    },
    {
      "status": 2,
      "message": "The system is now waiting for the user to responed a verification email."
    },
    {
      "status": 3,
      "message": "The user signed up and is now waiting for an administrator approval."
    }
  ],
  "token": "yHbb7a9S3yteUIo604KwwhchcrpE-wESafVtnnhQgAkj57ZxjWveNR63t64hqy8N2Gd_NPQhw6SAjNLv2mfYYP_rG-vELKe6gqEhqpWBFV43lD5IESH8k1SZldCmZiKLBvhCB_iHg4TfJQ8rUxh8ODYzaLALsSsn-I30ck7h21zn_LWDzxqxZa1Jq12tS0wrII7wOIyQiFCbsEmZiNMnlOR7Oz8CD7zhNHdOCFVh8Y801QIt3Jnn1hYz_4u1niWKJ5acTQqIEfXLZ7k65WnXuD46-UwZ0sORyKDj99o_xxEZpovchVm55cb5kRggqsmUjZBnP5N79fZiYA4tA8p_jPMBePIWE0q2WQ2m6CNJeJc"
}
```

Creates a new user in your app. in signup you must provide the basic details of username email, first name, last name and password:

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| firstName | string | the user's first name |
| lastName | string | the user's last name |
| email | string | the user's email |
| password | string | the user's password |
| confirmPassword | string | the value entered by the user when asked to confirm the password during registration |
| parameters | object | An object containing information for any paremeters to the signup call. This allows you to set additional info on the user object at registration time |


### .changePassword()
```shell
 curl -X POST  -d "{'oldPassword':'<old password>','newPassword':'<new password>'}" -H "Authorization: Bearer FrJQqDuGeo-U2b5QHhPeF2GS86U9EDTz79CEUbtbTPl032g2VCJPpiuAuGjAS0dR-u4XfxB2p0AgeOdkMulo3FEZWqOXIdzVdjJe4u2m9CyPzzK49YzY4SeD5Ea5p-6LbJLcpDJbE2nkzGv4TQw-S3pq-Tmo-RuuKnEKbEDOMfdSjlTed7Tks2ioeVjSKUwazwX4yglg0DPVavLPFrRp9KBNTnTNnoTOFd4zbC7gGxLjIPwU1-_T_N2UK7XIBlSGXbUBL_Zybxt1fWRlSrvxlw" https://api.backand.com/1/user/changePassword
```
```javascript
backand.changePassword(oldPassword, newPassword)
  .then(res => {
    console.log('Password changed');
  })
  .catch(err => {
    console.log(err);
  });
```
> This call returns no data, only an HTTP Status Code

Changes the password of the current user

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| oldPassword | string | the user's old password |
| newPassword | string | the user's desired new password |


### .socialSignin()
```shell
# There is no cURL equivalent for this call
```
```javascript
backand.socialSignin(provider)
  .then(res => {
    console.log('signin succeeded with user:' + res.data.username);
  })
  .catch(err => {
    console.log(err);
  });
```

Signs the user into a Back& application using a social media provider as the authentication method. This opens a dialog window supplied by the social media network provider. If the user does not have an account with the selected provider, they will be prompted to create one as a part of this process.

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| provider | string | Name of the provider to authenticate with. The full list can be obtained by calling *backand.getSocialProviders(scb)* |

## .object property
The `.object` property contains functions related to basic database operations that can be performed on an object. You can also use this property to call the on-demand actions governed by an object in your system.

### .getList()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -H "AnonymousToken: $ANONYMOUS_TOKEN" https://api.backand.com/1/objects/items
```
```javascript
// No filters
backand.object.getList(object, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });

// With filter parameters
let params = {
  sort: backand.helpers.sort.create('creationDate', backand.helpers.sort.orders.desc),
  exclude: backand.helpers.exclude.options.all,
  filter: backand.helpers.filter.create('user', backand.helpers.filter.operators.relation.in, userId),
  pageSize: 20,
  pageNumber: 1
};
backand.object.getList(object, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });

```
> Sample response

```json
{
  "totalRows": 2,
  "data": [
    {
      "__metadata": {
        "id": "73",
        "fields": {
          "id": {
            "type": "int",
            "unique": true
          },
          "name": {
            "type": "string"
          },
          "description": {
            "type": "text"
          },
          "user": {
            "object": "users"
          },
          "createdAt": {
            "type": "datetime"
          }
        },
        "descriptives": {
          "user": {
            "label": null,
            "value": ""
          }
        },
        "dates": {
          "createdAt": "2017-03-08 23:13:10Z"
        }
      },
      "id": 73,
      "name": "Test 1",
      "description": "A first test item",
      "user": "",
      "createdAt": "2017-03-08T23:13:10"
    },
    {
      "__metadata": {
        "id": "74",
        "fields": {
          "id": {
            "type": "int",
            "unique": true
          },
          "name": {
            "type": "string"
          },
          "description": {
            "type": "text"
          },
          "user": {
            "object": "users"
          },
          "createdAt": {
            "type": "datetime"
          }
        },
        "descriptives": {
          "user": {
            "label": null,
            "value": ""
          }
        },
        "dates": {
          "createdAt": "2017-03-08 23:13:21Z"
        }
      },
      "id": 74,
      "name": "Test 2",
      "description": "A second test item",
      "user": "",
      "createdAt": "2017-03-08T23:13:21"
    }
  ]
}
```

Fetches a list of records from the specified object. Uses *params* to store filter data

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| params | object | A hash of filter parameters. Allowed parameters are: *pageSize*, *pageNumber*, *filter*, *sort*, *search*, *exclude*, *deep*, *relatedObjects* |


### .getOne()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -H "AnonymousToken: $ANONYMOUS_TOKEN" https://api.backand.com/1/objects/items/<id>
```
```javascript
backand.object.getOne(object, id, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response

```json
{
  "__metadata": {
    "id": "73",
    "fields": {
      "id": {
        "type": "int",
        "unique": true
      },
      "name": {
        "type": "string"
      },
      "description": {
        "type": "text"
      },
      "user": {
        "object": "users"
      },
      "createdAt": {
        "type": "datetime"
      }
    },
    "descriptives": {
      "user": {
        "label": null,
        "value": ""
      }
    },
    "dates": {
      "createdAt": "2017-03-08 23:13:10Z"
    }
  },
  "id": 73,
  "name": "Test 1",
  "description": "A first test item",
  "user": "",
  "createdAt": "2017-03-08T23:13:10"
}
```
Retrieves a single record from the specified object.

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| id | integer | ID of the record to retrieve, subject to the filter specified in *params* |
| params | object | A hash of filter parameters. Allowed parameters are: *deep*, *exclude*, *level* |

### .create()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -X POST -H "AnonymousToken: $ANONYMOUS_TOKEN" -d "{'name':'Test 3','description':'A third test item'}" https://api.backand.com/1/objects/items
```
```javascript
backand.object.create(object, data, params)
  .then(res => {
    console.log('object created');
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response

```json
{"__metadata":{"id":"75"}}
```

Creates a record with the provided data in the specified object

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| data | object | Data to use in the creation of the new record |
| params | object | A hash of filter parameters. Allowed parameters are: *returnObject*, *deep* |

### .update()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -X PUT -H "AnonymousToken: $ANONYMOUS_TOKEN" -d "{'description':'A first items updated description'}" https://api.backand.com/1/objects/items/73
```
```javascript
backand.object.update(object, id, data, params)
  .then(res => {
    console.log('object updated');
  })
  .catch(err => {
    console.log(err);
  });
```
> This call has no response other than the status code by default.

Updates a record with the specified ID in the specified object with the provided data.

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| id | integer | ID of the object to update |
| data | object | Data to update the record with |
| params | object | A hash of filter parameters. Allowed parameters are: *returnObject*, *deep* |


### .remove()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -X DELETE -H "AnonymousToken: $ANONYMOUS_TOKEN" https://api.backand.com/1/objects/items/77
```
```javascript
backand.object.remove(object, id)
  .then(res => {
    console.log('object removed');
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response

```json
{"__metadata":{"id":"77"}}⏎
```

Deletes a record from the specified object with the specified ID

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| id | integer | ID of the object to update |


### .action.get()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -H "AnonymousToken: $ANONYMOUS_TOKEN" "https://api.backand.com/1/objects/action/items?name=simpleOnDemand"
```
```javascript
backand.object.action.get(object, action, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response (depends on action)

```json
{"response":"Hello world!"}
```

Triggers on-demand custom actions that operate via HTTP GET requests

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| action | string | Name of the action to trigger |
| params | object | Parameters for the action to operate upon |


### .action.post()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -X POST -H "AnonymousToken: $ANONYMOUS_TOKEN" "https://api.backand.com/1/objects/action/items?name=simpleOnDemand"
```

```javascript
backand.object.action.post(object, action, data, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response (depends on action)

```json
{"response":"Hello world!"}
```

Triggers on-demand custom actions that operate via HTTP POST requests

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the Back& object to work with |
| action | string | Name of the action to trigger |
| data | object | Object data to send as the body of the POST request |
| params | object | Parameters for the action to operate upon |



## .file
This property allows you to work with file actions, interacting with the related files directly. You can use this after you have finished creating a server-side action in the Back& dashboard, in the Actions tab of an object (object -> actions tab -> Backand Files icon -> name: 'files')

### .upload()
```shell
curl -X POST  -d "{'filename':'test.txt','filedata':'sample data'}" -H "AnonymousToken: $ANONYMOUS_TOKEN" "https://api.backand.com/1/objects/action/items?name=files"
```
```javascript
backand.file.upload(object, 'files', filename, filedata)
  .then(res => {
    console.log('file uploaded. url: ' + res.data.url);
  })
  .catch(err => {
    console.log(err);
  });
```
>Sample Response

```json
{"url":"https://files.backand.io/bkndionicstarter/test.txt"}
```

Uploads a file for a server-side action

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the object controlling the desired server-side action |
| fileAction | string | The name of the file action to work with |
| filename | string | The name of the file to upload |
| filedata | string | The file's data |


### .remove()
```shell
curl -X DELETE  -d "{'filename':'test.txt'}" -H "AnonymousToken: $ANONYMOUS_TOKEN" "https://api.backand.com/1/objects/action/items?name=files"
```
```javascript
backand.file.remove(object, 'files', filename)
  .then(res => {
    console.log('file deleted');
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample Response

```json
{}
```

Removes a file from a server-side action file set.

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| object | string | Name of the object controlling the desired server-side action |
| fileAction | string | The name of the file action to work with |
| filename | string | The name of the file to remove |


## .user
The *user* property returns data about the connected user (getUserDetails, getUsername, getUserRole, getToken, getRefreshToken).

### .getUserDetails()
```shell
curl -H "Authorization:Bearer <access token>" https://api.backand.com/api/account/profile
```
```javascript
backand.user.getUserDetails(force)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response

```json
// From SDK
{
  "status": 200,
  "statusText": "OK",
  "headers": {

  },
  "data": {
    "access_token": "7g9LM8ZHcvFLEIcSrdg8aYmpIK4gV9DYabab5spVsRmM6B_U3ovCWf8q6HeNFW4RMMQhsD6oiVpiTclHnO-J5qyFB2R8OyLl-ubrbIULvU0a7L8aS-cxPw7c4ij98zSPPa8kLkZBJULJRrcHn-fwmB4GygeTShNw7H1tqsHnIlcnk9X9ioj1CbjNjW4060gjTsGEOTGXACTrpQV4Qn3JD4XkSDNWySOG_15LorZdpiu7bcJ9T1EH9jnAIUyXA_0fIhFlDUsGV5Pr4EczaBufJQ",
    "token_type": "bearer",
    "expires_in": 86399,
    "appName": "bkndionicstarter",
    "username": "john.doe@backand.com",
    "role": "User",
    "firstName": "John",
    "lastName": "Doe",
    "fullName": "John Doe",
    "regId": 1009820,
    "userId": "29"
  },
  "config": {
  }
}

// From cURL
{
  "username":"matt+test@backand.com",
  "fullName":"Matt Billock",
  "role":"User",
  "appName":"bkndionicstarter"
}
```

Gets the connected user's details.

#### Parameters

| name | type | description |
| ---- | ---- | ----------- |
| force | boolean | Forces the SDK to refresh its data from the server. **Default: FALSE** |

## .query

The *query* property lets you initiate a custom Back& query using either HTTP GET or HTTP POST.

### .get()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -H "AnonymousToken: $ANONYMOUS_TOKEN" https://api.backand.com/1/query/data/test1
```
```javascript
backand.query.get(name, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response varies based on query return values

```json
[
  {
    "id": 1,
    "email": "support@backand.com",
    "firstName": "support",
    "lastName": "support"
  },
  ...
]
```
Calls a custom query using a HTTP GET

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| name | string | The name of the query to work with |
| params | object | Parameters to be passed to the query |


### .post()
```shell
# Env var $ANONYMOUS_TOKEN should contain your app's anonymous token
curl -X POST -H "AnonymousToken: $ANONYMOUS_TOKEN" -d "{'params':{...}}" https://api.backand.com/1/query/data/test1
```
```javascript
backand.query.post(name, data, params)
  .then(res => {
    console.log(res.data);
  })
  .catch(err => {
    console.log(err);
  });
```
> Sample response varies based on query return values

```json
[
  {
    "id": 1,
    "email": "support@backand.com",
    "firstName": "support",
    "lastName": "support"
  },
  ...
]
```

Calls a custom query using a HTTP POST

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| name | string | The name of the query to work with |
| data | object | Data to be included in the body of the HTTP POST |
| params | object | Parameters to be passed to the query |

## .constants
This contains constants used by the SDK, and is provided for reference.

| constant | description |
| -------- | ----------- |
| EVENTS | Contains the names of events fired by the SDK upon completion of one of several authentication actions |
| URLS | Contains the URL tokens for the Backand API |
| SOCIALS | Contains the list of social media authentication providers |

## .helpers
The `.helpers` property provides helper methods for constructing some of the more complex objects in the SDK. This includes filters and sort clauses for queries, as well as exclude clauses. It also provides a helper for obtaining a storageAbstract object.

### .helpers.filter()
```shell
# There is no cURL equivalent
```
```javascript
backand.helpers.filter.create("name", "notEquals", "John")
```
>Sample response

```json
{
  "fieldName": "name",
  "operator": "notEquals",
  "value": "John"
}
```

Constructs a filter parameter for a GET query.

#### Parameters

| name | usage |
| ---- | ----- |
| fieldName | the name of the field to apply a filter to |
| operator | the operator to apply |
| value | the value to use the operator to test against |

Here's a list of the available operators and the data types on which they operate:

| Operator | Applicable Data Types |
| -------- | --------------------- |
| equals | all |
| notEquals | numeric, date, text |
| greaterThan | numeric, date |
| greaterThanOrEqualsTo | numeric, date |
| lessThan | numeric, date  |
| lessThanOrEqualsTo | numeric, date  |
| empty | numeric, date, text |
| notEmpty | numeric, date, text |
| in | relation |
| startsWith | text |
| endsWith | text |
| contains | text |

### .helpers.sort()
```shell
# There is no cURL equivalent
```
```javascript
backand.helpers.sort.create("name", "asc")
```
>Sample response

```json
{
  "fieldName": "name",
  "order": "asc"
}
```

Constructs a "sort" clause for a GET request

#### Parameters

| name | usage |
| ---- | ----- |
| fieldName | the name of the field to apply a filter to |
| order | the order to sort ('asc' for ascending, 'desc' for descending) |

### .helpers.exclude()

```shell
# There is no cURL equivalent
```
```javascript
backand.helpers.exclude.options
```
>Sample response

```json
{
  "metadata": "metadata",
  "totalRows": "totalRows",
  "all": "metadata,totalRows"}
```

The exclude option gives you a number of constants you can use to exclude system objects from results.

### .helpers.storageAbstract()
```javascript
export class MyStorage extends StorageAbstract{
  constructor () {
    super();
    this.data = {};
  }
  setItem (id, val) {
    return this.data[id] = String(val);
  }
  getItem (id) {
    return this.data.hasOwnProperty(id) ? this.data[id] : null;
  }
  removeItem (id) {
    delete this.data[id];
    return null;
  }
  clear () {
    return this.data = {};
  }
}
```
The storageAbstract function allows you to define a new storage proxy for storing browsing data in the local session/information store. Simply provide an object that implements the following methods:

| function | arguments | description |
| -------- | --------- | ----------- |
| setItem | id, val | sets a value in the storage container |
| getItem | id | gets a value from the storage container |
| remove | id | removes a key from the storage container |
| clear | none | clears the storage container |

## .defaults
Contains a list of defaults for the SDK initialization parameters. The defaults are:

| parameter | default value |
| --------- | ------------- |
| appName | null |
| anonymousToken | null |
| useAnonymousTokenByDefault | true |
| signUpToken | null |
| apiUrl | 'https://api.backand.com' |
| storage | {} |
| storagePrefix | 'BACKAND_' |
| manageRefreshToken | true |
| runSigninAfterSignup | true |
| runSocket | false |
| socketUrl | 'https://socket.backand.com' |
| exportUtils | false |
| isMobile | false |
| mobilePlatform | 'ionic' |


## .offline
The `offline` property allows you to control the SDK's offline mode. When the SDK detects that it has lost internet connection, it will queue requests internally. Based on the configuration provided to the init() function, this can result in either a delay prior to call execution or an error response to any update or delete calls. Once internet connection is restored, the queued commands are sent to the server, and the responses should be handled asynchronously through standard promise resolution

### .setOfflineMode
This function is used to enable and disable offline mode for debugging purposes. When true, the SDK will operate in offline mode. When false, the SDK will communicate over the web. <br/>

#### Parameters
| name | type | description |
| ---- | ---- | ----------- |
| force | boolean | Sets the SDK to operate in - or exit from - offline mode.. **Default: TRUE** |

#### Sample Code
```javascript
// Enter offline mode
backand.offline.setOfflineMode(true);
// Exit offline mode
backand.offline.setOfflineMode(false);
```

### .cache
An object in which the cached data is stored<br/>

#### Sample Code
```javascript
backand.offline.cache;
```

### .queue
All requests queued during offline mode are stored here. These operations will be dispatched when offline mode ends.<br/>

#### Sample Code
```javascript
backand.offline.queue;
```


## Examples and further reading
***To view the demo web page, pull down a sample from our [example page](https://github.com/backand/vanilla-sdk/blob/master/example/) and run `npm start.`***
