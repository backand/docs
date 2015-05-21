# API Description
###Security & Authentication
   
Backand provides role-based security that allows you to determine specific permissions for each group of users. Backand uses [OAuth2](http://oauth.net/2/) authentication to identify users. Backand's implementation of OAuth2 authentication requires you to send the username, password, and appname (application name). In response, you receive an authentication token that must be supplied for all further communication with Backand.

You can either provide this token with each request, or use a cookie to persist the authentication token (recommended). Providing this token is required to use Backand's REST API. We have prepared a Backand Provider that will help you the authentication activities. Start by including the Backand SDK script files in your app:

```
      <!-- We use client cookies to save the REST API token -->
      <script src="//code.angularjs.org/1.3.0/angular-cookies.min.js"></script>
      <!-- Backand SDK for Angular -->
      <script src="//cdn.backand.net/backand/dist/1.5.1/backand.min.js"></script>
``` 
You will also need to add the Backand and angular-cookies dependencies to your angular app definition:

```
      //app.js
      angular.module('YOUR-APP-NAME', ['backand', 'ngCookies'])
```
 
Once this is complete, you are ready to sign in to Backand, for other users to sign up to your app read about it here. There is also an option for anonymous access, read about it here.


####Sign In
Use the Backand provider with the following parameters to get an OAuth2 access token:

* **username** - Backand's username.
* **password** - Backand's .password
* **appName** - The application name.

```
      // SignInCtrl.js
      function SignInCtrl(Backand, $cookieStore) {
        $scope.signIn = function() {
          Backand.signin($scope.username, $scope.password, $scope.appName)
          .then(
            function (token) {
              //save the token in the cookie
              $cookieStore.put(Backand.configuration.tokenName, token);
            },
            function (data, status, headers, config) {
              //handle error
            }
          );
        }
      }
      // Use an Angular HTTP Interceptor to add the authentication token to each HTTP request
      function httpInterceptor($q, $log, $cookieStore) {
        return {
          request: function(config) {
            config.headers['Authorization'] = $cookieStore.get('backand_token');
            return config;
          }
        };
      }
    
```

####Sign Up

With the `/user/signup` api you can enable other users to sign up to your app. The sign up process consists of the following steps:

- From your app's registration page, call the `/user/signup` action
- The email sent contains a verification link. When the user clicks the included link, Backand verifies the user's email address and completes the sign-up process by redirecting the user to a pre-defined success page (typically the application's sign-in page).

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
          url : Backand.configuration.apiUrl + '/1/user/signup,
          headers: {
            'SignUpToken': signUpToken
          },
          data: user
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
  

To learn more about Backand's security and how to work with users and roles, look at the <a href="https://github.com/backand/angular-yeoman-todos/tree/todo-with-users">todo-with-users</a> example.
  
  
###REST API CRUD Operations
Once authentication is completed, you can perform all relevant [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations in your application:


####List of Objects


Call `/objects/{name}` with the following parameters to get a list of items:

* **pageSize** - The number of returned items in each getList call (default 20).
* **pageNumber** - The page number starting with 1 (1-based, default 1).
* **filter** - An array of JSON objects where each item has the properties fieldName, operator and value. The operator options depend on the field type.
* **sort** - An array of JSON objects where each item has the properties fieldName and order. The order options are "asc" or "desc".
* **search** - Free text filter search.
* **deep** - When set to true, brings the related parent items in the relatedTables property.

```
  self.getList = function (name, pageSize, pageNumber, filter, sort) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/objects/' + name,
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
* **deep** - When set to true, brings the related parent items in the relatedTables property

```
  self.getOne = function (name, id, deep) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id
          params: {
            deep: deep
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
          url : Backand.configuration.apiUrl + '/1/objects/' + name,
          data: data,
          params: {
            returnObject: returnObject
          }
      })
  };
```

####Update


PUT to `/objects/{name}/{id}` to update an existing object (with changes provided in the data portion of the AJAX call). You must also supply the following parameters:

* **id** - The item's id, which is the primary key value for the item's database table 
* **returnObject** - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running.

```
  self.update = function (name, id, data, returnObject) {
      return $http({
          method: 'PUT',
          url : Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id,
          data: data,
          params: {
            returnObject: returnObject
          }
      })
  };
```

####Delete


DELETE to `/objects/{name}/{id}` to remove an item:

* **id** - The item's id, which is the primary key value for the item's database table.

```
  self.delete = function (name, id) {
      return $http({
          method: 'DELETE',
          url : Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id
      })
  };
```

###Custom Actions
Custom actions can be called by sending a GET request to `/objects/action/{objectName}/{id}` The custom action is associated with an object list in order to simplify security configuration. It can also be associated with a specific object id, which is used as an input to the action, but this is optional. You can also define additional parameters in the request, which are passed through to the function. The action returns a custom JSON response.

* **actionName** - The custom action name.
* **parameters** - A JSON object with all predefined parameters for the action.

```
self.callAction = function (objectName, id, actionName, parameters) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/objects/action/' + objectName + '/' + id
          params: {
             parameters: parameters
          }
      });
  };
```
**Note**: You can find a configuration and testing environment for each of the custom actions in the right side menu for your application on an object's Actions tab. 

###Queries
You can run custom pre-configured queries by sending a GET request to `/query/data/{queryName}` The query syntax must follow the SQL rules of the underlying database that your application is using. You can include predefined parameters in your request that are then used by your query.

* **parameters** - A JSON object with all predefined query parameters provided.

```
self.query = function (queryName, parameters) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/query/data/' + queryName
          params: {
             parameters: parameters
          }
      });
  };
```

**Note**: You can find a configuration and testing environment for custom queries in the Application dashboard's right side menu, under "Queries". 

###Experience more
To dive deeper into Backand's functionality, clone our Angular demo application on [github](https://github.com/backand/angular-yeoman-todos)

