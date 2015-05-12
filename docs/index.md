
# API Description

##What is Backand
Backand's goal is to free up the front end development of web applications by providing a rich, robust, and scalable back end with minimal impact on the development process. The key feature offered by Backand is the [ORM](http://en.wikipedia.org/wiki/Object-relational_mapping), but most application back ends require more than just object management. Backand provides you with most of what your application needs automatically, offering features like tracking data changes, logging, role-based security, back-office connectivity, and much more. You can even create your own server side actions with JavaScript, running custom server side queries and easily integrating with third party services.

##Backand ORM
Backand [ORM](http://en.wikipedia.org/wiki/Object-relational_mapping) automatically provides you with a REST API to perform [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations against your database. You can connect an existing database you already have, or create a new database with the Backand dashboard. When you create a database at Backand it is automatically populated in Amazon's AWS Relational Database Service (AWS RDS), providing an isolated database server that you can scale automatically, or even use in other applications entirely independent of Backand. Even though Backand focuses on software services as opposed to platform services, when you register a database with Backand it is truly your database.

##SQL and NoSQL - The Best of Both Worlds
Through the flexibility of Backand's API, you have the ability to work with your data at whatever level you desire. You can perform basic [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations in a couple ways. In one way you can mimic a SQL database by simply returning a shallow representation of the object, with foreign key references remaining as simple IDs in the response data. However, you can also perform deep queries that resolve all of the underlying data objects into a single set of response data, giving you the level of object detail that you often see with NoSQL databases. With a simple parameter change, you can switch between the two patterns at will!

#Security Concept
##Authentication
The default authentication setup for Backand applications relies on [OAuth2](http://oauth.net/2/) to provide tokenize authentication. By logging in with your username (your email address), your password, and your app name, you receive an authentication token that is valid for 24 hours. This token is required for all communication with Backand, and as such we highly recommend that you use Backand's angular infrastructure to help you save the access token. Using the [ngCookies](https://docs.angularjs.org/api/ngCookies) wrapper, you can adjust the default request header to include your current access token, greatly easing the process of securing your API communications. Backand also offers Anonymous Access, which allows you to access your application without the need to authenticate via username and password.

##Sign Up
Registering with Backand, and creating an application, automatically sets you as a user with an "Admin" role in your new project (see Roles for more info). By default your application is marked private, which means that only users that you have personally invited can sign up for your app. This can be changed in the dashboard by setting your application to Public in the Security & Auth --> Configuration menu. Setting your application to public allows any user to register for - and use - your application. These users are assigned a default role, which needs to be configured when you enable public usage of your app (see Roles for more details). For security reasons you cannot change the role of a user through API calls - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -> Users page, or by creating a custom server-side action that performs this task. For a private application the registration steps are as follow:

* First, set your application's registration page on the Security & Auth --> Configuration page.
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page. Once you have entered the emails you wish to invite, click the "Invite User(s)" button. This will send an email to each new user with a link to the registration page that you created.
The following steps are the same for both Public and Private apps:
* The user enters his/her email, name and password on your registration page
* After the user submits their registration, Backand sends a verification email to authenticate the user's identity
* After the user clicks on the link in the verification email, Backand completes the registration process and redirects to the "Custom Verified Email Page" URL for your application. Configure this on the Security & Auth --> Configuration page
Here is the api of the Sign Up
##Remove user from the app
There are two ways to remove a user from the application. You can permanently remove a user from the application by deleting a user from the Users grid in your application's admin section. This requires the user to register again if they wish to continue using your app. Alternatively, you can un-check the "approved" checkbox in the user's row on the Security & Auth --> Users page. This allows you to reinstate the user simply by re-checking the "approved" column.

##Anonymous Access
If you wish to enable anonymous access to your application, you need to perform a couple steps. First, you need to turn on the Anonymous Access option in you application's settings. Once that's done, you are given the option to create an AnonymousToken value. By passing this value in a request header (e.g. AnonymousToken=<your token here>) you can perform api actions for your application. finally, you need to set the default role of an anonymous user within your application's settings (see Roles for more info).

##Link your app's users with Backand's users
We offer full capability to link your application's users with Backand users. However, to ensure that you can restrict access to your application without impacting the Backand user's experiece, we recommend that you create another table for your app's users with a unique email address column. This allows you to build rules specific to your app, but still link to Backand's users without affecting their overall experience.

##Roles & Security Templates
Each user has a role. When you created your app, you were automatically assigned with an "Admin" role. The "Admin" role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.

#Custom Actions
In Backand's system, you can create server-side activity called Actions. These actions can be used for the purpose of security, integration, performance, notification and data integrity, among others, providing you with more flexibility in your app's design. There are two types of Actions that can be created. The first are initiated via a direct web request. These are known as "On Demand" actions. Additionally, you can create automated actions that take place based upon a data interaction event. These automated actions can occur whenever you create, update, or delete an item in your system. On Demand actions are associated with a specific object, and can be found on the Object --> {name} page in the Actions tab. The automated Create, Update and Delete actions are associated with a specific object that is compatible with a specific row in a table, while On Demand actions make association with a specific role optional.

There are 3 kinds of actions that can be created for either action type:
- Server side JavaScript code actions
- Transactional database script actions
- Send Email actions
All 3 types of actions use the following common parameters:
* A Where condition - a SQL where clause that determines if the action will be performed.
* Input Parameters, added to the query string of the request that triggers the action, that will serve as variable values that you can supply to your action's code. These parameters will serve as tokens in the action definition and will be replaced with the actual values when the code executes.
#Server Side Javascript code

You can run standard JavaScript on the server. It runs on the [V8 engine](http://en.wikipedia.org/wiki/V8_(JavaScript_engine)). To execute the JavaScript action, put your code into the following function:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
    // write your code here
    return {};
}
```

The function parameters are:
* userInput: This parameter is only provided for Create and Update actions, and is the object that was sent to the action. It is null for Delete actions, as well as for On Demand actions.
* dbRow: This parameter is populated in After Create, Update, and Delete automated actions, and if you supply an optional ID to an On Demand action. The dbRow parameter will contain the row's entry in the database prior to any changes made.
* parameters: This parameter represents the variables sent in the query string for the action.
* userProfile: This parameter stores the current username, the user's role, and the access token used by the user to perform the action. It is of the format {"username": "string", "role": "string", "token": "string"}.
In addition to the above parameters, you can also make use of the following global objects:
* $http - a service for AJAX calls
* $http({method:"GET",url:CONSTS.apiUrl + "/1/objects/objectexample" , headers: {"Authorization":userProfile.token}});
CONSTS - CONSTS.apiUrl for Backands API URL
console.log(message, object) and console.error(message, object), to debug your code
Automated actions will have a response that matches the format expected by the triggering call (such as the return value of a CREATE call). On Demand actions, though, will return whatever is returned by the custom server code, which can be any properly-formatted JSON.

#Error Handling

If your code results in an error (for example, if you write the following: throw new Error("An error occurred!")), the request will return HTTP status 417, and the response body will contain the associated error message.
Transactional database script

Transactional database scripts are SQL scripts that run within the same transaction context as the triggering action, provided the event occurs in the object event "During the data save before the object is committed". This means that if the Create, Update or Delete request fails then your script will be rolled back like any other transaction.

#Send Email

Send Email actions, in addition to common parameters, allow you to also supply the usual email fields: To, Cc, Bcc, From, Subject and Message. In addition, you can provide an object ID to obtain a deep object to use in the action.

#Queries
You can create and call your own custom queries in your application. You can use parameters as tokens that will be replaced with the actual parameter values, similar to how Custom Actions work. You can create your own filters, sorting, and paging, as well as use aggregation to summarize information.

#Security & Authentication
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


#Sign In


Use the Backand provider with the following parameters to get an OAuth2 access token:

username - Backand's username.

password - Backand's .password

appName - The application name.
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
#Sign Up

With the /user/signup api you can enable other users to sign up to your app. The sign up process consists of the following steps:

From your app's registration page, call the /user/signup action
The email sent contains a verification link. When the user clicks the included link, Backand verifies the user's email address and completes the sign-up process by redirecting the user to a pre-defined success page (typically the application's sign-in page).
The sign up api uses the SignUpToken in the request header. You can find this token in the Security & Auth Configuration menu 
(*) Configuration of the sign up process is located in the App left side menu: Security & Auth --> Configuration

The parameters for the sign-up call are:
SignUpToken - A security token to prevent malicious attacks. this should be added to the header

user - A JSON object representing the user's first name, last name, email, and password.

```
{
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "password": "string",
  "confirmPassword": "string"
}
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
#Anonymous Access

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
#REST API CRUD Operations
Once authentication is completed, you can perform all relevant [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations in your application:


##List of Objects


Call /objects/{name} with the following parameters to get a list of items:

pageSize - The number of returned items in each getList call (default 20).

pageNumber - The page number starting with 1 (1-based, default 1).

filter - An array of JSON objects where each item has the properties fieldName, operator and value. The operator options depend on the field type.

sort - An array of JSON objects where each item has the properties fieldName and order. The order options are "asc" or "desc".

search - Free text filter search.

deep - When set to true, brings the related parent items in the relatedTables property.
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
##One Object


Call /objects/{name}/{id} with a specific item id and with the following parameters to get a specific item:

id - The item's id, which is the primary key value for the item's database table

deep - When set to true, brings the related parent items in the relatedTables property
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
##Create


POST to /objects/{name} with a new object to create (stored in the data portion of the AJAX call) using the following parameters:

returnObject - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running
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
##Update


PUT to /objects/{name}/{id} to update an existing object (with changes provided in the data portion of the AJAX call). You must also supply the following parameters:

id - The item's id, which is the primary key value for the item's database table returnObject - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running.
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
##Delete


DELETE to /objects/{name}/{id} to remove an item:

id - The item's id, which is the primary key value for the item's database table.
```
  self.delete = function (name, id) {
      return $http({
          method: 'DELETE',
          url : Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id
      })
  };
```
##Custom Actions
Custom actions can be called by sending a GET request to /objects/action/{objectName}/{id} The custom action is associated with an object list in order to simplify security configuration. It can also be associated with a specific object id, which is used as an input to the action, but this is optional. You can also define additional parameters in the request, which are passed through to the function. The action returns a custom JSON response.

actionName - The custom action name.

parameters - A JSON object with all predefined parameters for the action.
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
Note: You can find a configuration and testing environment for each of the custom actions in the right side menu for your application on an object's Actions tab. 
##Queries
You can run custom pre-configured queries by sending a GET request to /query/data/{queryName} The query syntax must follow the SQL rules of the underlying database that your application is using. You can include predefined parameters in your request that are then used by your query.

parameters - A JSON object with all predefined query parameters provided.
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
Note: You can find a configuration and testing environment for custom queries in the Application dashboard's right side menu, under "Queries". 
#Experience more
To dive deeper into Backand's functionality, clone our Angular demo application on [github](https://github.com/backand/angular-yeoman-todos)

