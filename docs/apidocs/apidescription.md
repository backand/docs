# Security & Authentication

Backand provides role-based security that allows you to determine specific permissions for each group of users. Backand uses [OAuth2](http://oauth.net/2/) authentication to identify users. Backand's implementation of OAuth2 authentication requires you to send the username, password, and appname (application name). In response, you receive an authentication token that must be supplied for all further communication with Backand.

You can either provide this token with each request, or use the Backand SDK interceptor to append it to each request automatically (recommended). Providing this token is required to use Backand's REST API. We have prepared a Backand Provider that will help you the authentication activities. Start by including the Backand SDK script files in your app:

```
      <!-- Backand SDK for Angular -->
      <script src="//cdn.backand.net/backand/dist/1.8.0/backand.min.js"></script>
``` 
You will also need to add the Backand dependency to your angular app definition:

```
      //app.js
      angular.module('YOUR-APP-MODULE', ['backand'])
```
Configure the application name and tokens, which can be found in Backand Security & Auth --> Configuration and Social & Keys pages:
```
      angular.module('YOUR-APP-MODULE')
            .config(function (BackandProvider) {
                  BackandProvider.setAppName(YOUR-APP-NAME)
                        .setAnonymousToken(ANONYMOUS-TOKEN)
                        .setSignUpToken(SIGN-UP-TOKEN);
            }
```
 
Once this is complete you can use Backand SDK functions for authenticating the users of your application, as described below.


####User Authentication

####Sign In
Use the Backand provider with the following parameters to get an OAuth2 access token:

* **username** - Backand's user username.
* **password** - Backand's user password

```
      // SignInCtrl.js
      function SignInCtrl(Backand) {
        this.signIn = function() {
          Backand.signin(username, password)
          .then(
            function () {
              //enter session
            },
            function (data, status, headers, config) {
              //handle error
            }
          );
        }
      }
```

####Sign Up

With the `/user/signup` api you can enable other users to sign up to your app. The sign up process consists of the following steps:

- From your app's registration page, call the `/user/signup` action
- If Sign-Up Email Verification is enabled, an email is sent that contains a verification link. When the user clicks the included link, Backand verifies the user's email address and completes the sign-up process by redirecting the user to a pre-defined success page (typically the application's sign-in page).

The sign up api uses the SignUpToken in the request header. You can find this token in the Security & Auth Configuration menu 
(*) Configuration of the sign up process is located in the App left side menu: Security & Auth --> Configuration

The parameters for the sign-up call are:
* **SignUpToken** - A security token to prevent malicious attacks. this should be added to the header
* **user** - A JSON object representing the user's first name, last name, email, and password.

```
{
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "password": "string",
  "confirmPassword": "string"
}
```
```
  self.signUp = function (user, signUpToken) {
      return $http({
          method: 'POST',
          url : Backand.getApiUrl() + '/1/user/signup,
          headers: {
            'SignUpToken': signUpToken
          },
          data: user
      })
  };
```

####Request Reset Password

With the `/user/requestResetPassword` api you can request a password reset for a particular user (for example, when they have forgotten their password). The process consists of the following steps:

- From your app's registration page, call the `/user/requestResetPassword` action
- An email is sent containing a one-time use token that  your user can use to reset their password. See the Reset Password section below for more information on this process.

The parameters for the requestResetPassword call are:
* **appName** - A string containing your application's name
* **userName** - A string containing the user name requesting the password reset

```
  self.requestResetPassword = function (userName) {
      return $http({
          method: 'POST',
          url : Backand.getApiUrl() + '/1/user/requestResetPassword,
          data: 
            {
              "appName": "string",
              "userName": userNames
            }
      })
  };
```

####Reset Password

With the `/user/resetPassword` api you can reset a password for a particular user (for example, when they have forgotten their password). The process consists of the following steps:

- Ensure that the user has already requested a password reset.
- Obtain the one-time token from the user that was sent via email.
- Use the ResetPassword API to record the user's new password, including the one-time use token

The parameters for the requestResetPassword call are:
* **resetToken** - the one-time use token the user received via email
* **newPassword** - the new password for the user.

```
  self.resetPassword = function (resetToken, newPassword) {
      return $http({
          method: 'POST',
          url : Backand.getApiUrl() + '/1/user/resetPassword,
          data: 
            {
              "resetToken": resetToken,
              "newPassword": newPassword
            }
      })
  };
```

####Change Password

With the `/user/changePassword` api you can change the password for a particular user. The parameters for the changePassword call are:
* **oldPassword** - the old password for the user
* **newPassword** - the new password to assign to the user

```
  self.changePassword = function (oldPassword, newPassword) {
      return $http({
          method: 'POST',
          url : Backand.getApiUrl() + '/1/user/changePassword,
          data: 
            {
              "oldPassword": oldPassword,
              "newPassword": newPassword
            }
      })
  };
```

####Anonymous Access

If you want to allow anonymous users to access the application (i.e. without username and password), you can use the AnonymousToken parameter:


```
      // Use an Angular HTTP Interceptor to add the anonymous token to each HTTP request
      function httpInterceptor($q, $log) {
        return {
          request: function(config) {
            config.headers['AnonymousToken'] = anonymousToken;
            return config;
          }
        };
      }
    
```

# REST API CRUD Operations

Once authentication is completed, you can perform all relevant [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations in your application:

####List of Objects


Call `/objects/{name}` with the following parameters to get a list of items:

* **pageSize** - The number of returned items in each getList call (default 20).
* **pageNumber** - The page number starting with 1 (1-based, default 1).
* **filter** - An array of JSON objects where each item has the properties fieldName, operator and value. The operator options depend on the field type.  
An example for filter: 
```
[{"fieldName":"firstName","operator":"contains","value":"el"},{"fieldName":"lastName","operator":"startsWith","value":"ri"}]
```
following are the possible operators depending on the field type:  
**numeric or date fields:**  
-- equals  
-- notEquals  
-- greaterThan  
-- greaterThanOrEqualsTo  
-- lessThan  
-- lessThanOrEqualsTo  
-- empty  
-- notEmpty  
**textual fields:**  
-- equals  
-- notEquals  
-- startsWith  
-- contains  
-- notContains  
-- empty  
-- notEmpty  
**object fields:**  
-- in  
* **sort** - An array of JSON objects where each item has the properties fieldName and order. The order options are "asc" or "desc".
An example for sort: 
```
[{"fieldName":"firstName","order":"desc"}]
```
* **search** - Free text filter search.
* **deep** - When set to true, brings the related parent items in the relatedTables property.

```
  self.getList = function (name, pageSize, pageNumber, filter, sort) {
      return $http({
          method: 'GET',
          url: Backand.getApiUrl() + '/1/objects/' + name,
          params: {
            pageSize: pageSize,
            pageNumber: pageNumber,
            sort: sort,
            filter: filter
          }
      });
  };
```
####One Object


Call `/objects/{name}/{id}` with a specific item id and with the following parameters to get a specific item:

* **id** - The item's id, which is the primary key value for the item's database table
* **deep** - When set to true, brings the related collections and objects
* **level** - When deep is set to true, this parameter determines the collection relations dept level. The default is 3 (grandchildren)

```
  self.getOne = function (name, id, deep, level) {
      return $http({
          method: 'GET',
          url: Backand.getApiUrl() + '/1/objects/' + name + '/' + id,
          params: {
            deep: deep,
            level: level
          }
      });
  };
```

####List Collection Objects from a Collection Field for a Specific Object ID


To obtain a list of collection objects stored in a collection field on a specific object, call  `/objects/{name}/{id}/{collection}` with the following parameters:

* **id** - The item's id
* **collection** - A name of a collection field in the object
* **pageSize** - The number of returned rows in each getList call (default 20).
* **pageNumber** - The page number, starting with 1 (1-based, default 1).
* **filter** - An array of JSON objects where each item has the properties fieldName, operator and value. The operator options depend on the field type. Click [here](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-of-objects) for more information.
* **sort** - An array of JSON objects where each item has the properties fieldName and order. The order options are "asc" or "desc". Click [here](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-of-objects) for more information.
* **search** - Free text filter search.

```
  self.getList = function (name, id, collection, pageSize, pageNumber, filter, sort) {
      return $http({
          method: 'GET',
          url: Backand.getApiUrl() + '/1/objects/' + name + '/' + id + '/' + collection,
          params: {
            pageSize: pageSize,
            pageNumber: pageNumber,
            sort: sort,
            filter: filter
          }
      });
  };
```

####List Specific Collection Objects From a Set of Filtered Parent Objects


To list all of the collection objects for a specific collection field within a filtered set of objects, call `/objects/{name}/filter1/{collection}` with the following parameters:

* **filter1** - This specifies which filter to apply to the object, and is applied prior to obtaining the collection fields. Click [here](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-of-objects) for more information on filters.
* **collection** - A name of a collection field in the object.
* **pageSize** - The number of rows returned by this call (default 20).
* **pageNumber** - The page number to obtain starting with 1 (1-based, default 1).
* **filter** - An array of JSON objects where each item has the properties fieldName, operator, and value. The operator options depend on the field type. Click [here](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-of-objects) for more information on the filter parameter.   
* **sort** - An array of JSON objects where each item has the properties fieldName and order. The order options are "asc" or "desc". Click [here](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-of-objects) for more information on the sort parameter.
* **search** - Free text filter search.

```
  self.getList = function (name, filter1, collection, pageSize, pageNumber, filter, sort) {
      return $http({
          method: 'GET',
          url: Backand.getApiUrl() + '/1/objects/' + name + '/filter1/' + collection,
          params: {
            pageSize: pageSize,
            pageNumber: pageNumber,
            sort: sort,
            filter: filter,
            filter1: filter1
          }
      });
  };
```

####Create


POST to `/objects/{name}` with a new object to create (stored in the data portion of the AJAX call) using the following parameters:

* **returnObject** - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running

```
  self.create = function (name, data, returnObject) {
      return $http({
          method: 'POST',
          url : Backand.getApiUrl() + '/1/objects/' + name,
          data: data,
          params: {
            returnObject: returnObject
          }
      })
  };
```

####Update


PUT to `/objects/{name}/{id}` to update an existing object (with changes provided in the data portion of the AJAX call). You must also supply the following parameters:

* **id** - The item's id 
* **returnObject** - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running.

```
  self.update = function (name, id, data, returnObject) {
      return $http({
          method: 'PUT',
          url : Backand.getApiUrl() + '/1/objects/' + name + '/' + id,
          data: data,
          params: {
            returnObject: returnObject
          }
      })
  };
```

####Delete


DELETE to `/objects/{name}/{id}` to remove an item:

* **id** - The item's id

```
  self.delete = function (name, id) {
      return $http({
          method: 'DELETE',
          url : Backand.getApiUrl() + '/1/objects/' + name + '/' + id
      })
  };
```

# Custom Actions

[Custom actions](customactions.md) can be called by sending a GET request to `/objects/action/{objectName}/{id}` The custom action is associated with an object list in order to simplify security configuration. It can also be associated with a specific object id, which is used as an input to the action, but this is optional. You can also define additional parameters in the request, which are passed through to the function. The action returns a custom JSON response.

* **actionName** - The custom action name.
* **parameters** - A JSON object with all predefined parameters for the action.

```
self.callAction = function (objectName, id, actionName, parameters) {
      return $http({
          method: 'GET',
          url: Backand.getApiUrl() + '/1/objects/action/' + objectName + '/' + id
          params: {
             parameters: parameters
          }
      });
  };
```
**Note**: You can find a configuration and testing environment for each of the [custom actions](customactions.md) in the right side menu for your application on an object's Actions tab. 

# Queries

You can run custom pre-configured queries by sending a GET request to `/query/data/{queryName}` The query syntax must follow the SQL rules of the underlying database that your application is using. You can include predefined parameters in your request that are then used by your query.

* **parameters** - A JSON object with all predefined query parameters provided.

```
self.query = function (queryName, parameters) {
      return $http({
          method: 'GET',
          url: Backand.getApiUrl() + '/1/query/data/' + queryName
          params: {
             parameters: parameters
          }
      });
  };
```

**Note**: You can find a configuration and testing environment for custom queries in the Application dashboard's right side menu, under "Queries". 


# Experience More

To dive deeper into Backand's functionality, clone our Angular demo application on [github](https://github.com/backand/angular-yeoman-todos)
