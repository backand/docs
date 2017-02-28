# Backand features
This section contains documentation on the features offered by Backand. We will cover the following functional areas:

* Security and Authentication - a summary of the security features offered by Backand
* Backand Storage - an overview of our file storage options
* Backand Hosting - an overview of our hosting options
* Custom Actions - an overview of the ability to add custom actions and event-based functionality
* Queries - an overview of the on-demand query functionality
* NoSQL Query Language - an overview of our NoSQL-style query language

## Security Concepts

This section describes how to manage your app's users within Backand's user security paradigm. An important element to note up front is that, by default, [Backand](https://www.backand.com) provides two independent user objects. One user object is handled entirely by Backand, and is responsible for authentication and role-based security. The data for this user object is stored in Backand's database, and as such cannot partake of your app's custom logic. The second user object is offered in the default data model for a Backand application, and is titled 'users'. This object represents a user of your application, as opposed to a generic Backand user, and allows you to associate the user with various other objects in your application. Backand provides sync actions that allow you to keep these two user object lists up-to-date (see [Link your app's users with Backand's registered users](security.md#Link your app's users with Backand's registered users) for more info). We highly recommend running the [todos-with-users app](https://github.com/backand/todos-with-users) in addition to reading this documentation. This simple app covers most of the user management use cases for a Backand application, such as allowing users to read all of your app's data but only allowing them to create and update their own objects, or restricting anonymous users to read-only access, or creating an Admin role that has full read-write-update-delete access to your application's objects.

### Authentication

In this section, we'll cover Backand's authentication mechanisms.

#### OAuth 2.0
```shell
# To obtain an access token using a username and password, use the following
# command:
curl https://api.backand.com/token -d username=guest@backand.com -d password=guest1234 -d grant_type=password -d appName=bkndexample
# To obtain an access token using the Social Media master key, use the
# following command:
curl -X GET https://api.backand.com/1/user/facebook/token?accessToken=EAAPutOqBPlkBANfUQXdmp7xseF16JpSknTGxZBBZAwd1TDigDUqC9i5NixlDOKFNpNQQwqJFHHPs059STwG0qzodlfzOvnE2q4EPxXM43ZBZBUOV44lCjpmFhMwJmeXUgRSBlwxJaKrvZAH7vW7NdBQa3dMLQZAv4RTXRZAcgqNVQZDZD&appName=bkndexample&signupIfNotSignedIn=true
# You can then use the access token to call any Backand API for your
# application
curl https://api.backand.com/1/objects/items -H "Authorization: Bearer OfKDiw6fQwfhmUcoh3WuXken_Sgj18za8SDqn1Ed2S_qI88HWNwGBSpG2ktUkSbNIwcGn6dwos7ivvVARC1UIiCf8fzlNynFNZamCZPDpbOhRjheq3RnU6HJiCbqlks57PLvMnf9PxGK8D3FU8MeaPyUk7mmbAtvgc6GF-9s13YpFjVQkM5XRZ-CsuWjaRoukygLMOivj1iPYxBB4c-hu6ocZKQljiSnw6rroYbD4spmUIkwDnC0rz1jP9ln5KNI3KmO0KLcWJF6E_zWfepdFw"
```

The default authentication setup for [Backand](https://www.backand.com) applications relies on [OAuth2](http://oauth.net/2/) to provide tokenized authentication. By logging in with your username (your email address), your password, and your app name, you receive an authentication token that is valid for 24 hours. This token is required for all communication with Backand, and as such we highly recommend that you use [Backand's SDK](https://github.com/backand/angularbknd-sdk) to help you manage the access token. You can change the default expiration of 24 hours by using a refresh token, which allows you to reuse the authentication token indefinitely. The refresh token is an encrypted hash of the master and user keys. You can revoke one (or all) of your user's refresh tokens by changing the refresh token and requiring users to re-authenticate. For more information, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#user-authentication).


Parameters:
--signupIfNotSignedIn - (Optional, default false) - If the user tries to sign in without first registering for the application, the user will receive an error message ("The user is not signed up to {appName}"). If this value is set to true, then the user will be automatically registered with the app if they have not yet been signed up


#### Basic authentication
```shell
# Perform basic authentication via header
curl https://api.backand.com/1/objects/items -u <master key>:<user key>
# Perform basic authentication using URL parameters
curl https://api.backand.com/1/objects/items?authorization=basic+<master key>:<user key>
```

You can use [basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication) to ease working with other servers - simply enter the master key as the username, and the user key as the password. You can use this approach to obtain a list of Items by using the following API call:

You can also send the basic authorization token through as a query string.

<aside class="warning">Do not use this method from the client side as it exposes your app's secret token, which can be used to perform any action in your system. This method is for server-side calls only.</aside>

#### Anonymous authentication
```shell
# Perform anonymous authentication through headers
curl https://api.backand.com/1/objects/items -H "AnonymousToken: <anonymous token>"
# You can also send the anonymous token through in a query string:
curl https://api.backand.com/1/objects/items?AnonymousToken=<anonymous token>
```

Backand also offers Anonymous Access, which allows you to access your application without the need to authenticate via username and password. By passing this value in a request header (e.g. AnonymousToken=<your token here>) you can perform api actions for your application

### Sign Up

Registering with [Backand](https://www.backand.com), and creating an application, automatically sets you as a user with an 'Admin' role in your new project (see [roles](#roles) for more info). By default your application is marked public which mean any user can register to your application.

These users are assigned a default role 'User', which has full CRUD access to your app. You needs to be configured when you enable public usage of your app (see [roles](#roles) for more details).

```javascript
  //Create new Security Action in Before Create trigger with the following
  //code (this will update the role to 'Public'):
  userInput.Role = 'Public';
```

<aside class="notice">
For security reasons you cannot change the role from the sign-up API - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -&gt; Registered Users page, or by creating a custom server-side action with Admin rights (see JavaScript tab).
</aside>

#### Saving Additional Parameters During Sign-up

In many cases, we would like to collect additional information from the user during sign up. We can automatically populate the related fields on the Users object as a part of the sign-up request by using the **parameters** parameter for the sign-up call. The server, during the 'Create My App User' action, will translate these key-value pairs into the appropriate fields on the 'Users' object - as such, it is important that all fields provided already exist on the Users object. The following example demonstrates specifying a user's "company" name as a part of the sign-up:

1. Update the Model and add the field **company** to the **users** object:
    1. Go to **Objects --> Model**
    2. Add this line as a data field in the **users** object model: **"company": {"type": "string"}**
    3. Click on **Validate & Update**

2. When calling Backand.signup(), send the **parameters** objecct through as the last input parameter for the request. The code will resemble the following:

```javascript
    Backand.signup(firstName, lastName, username, password, password, {company: self.company}).then(...);
```

3. There's no need for server-side modificcations - values for "parameters" are handled automatically by the action 'Create My App User' (found in the **Security Actions** section).

4. Modify the UI to collect the 'company' name for the user.

Now, when sign-up is completed, you can see the company name in the **users** object's Data tab.

#### Private app

You can change the default public app to be private. This can be changed in the dashboard by setting the Public flag for your application to 'false' in the Security & Auth --> Configuration menu. For a private app, the registration steps are as follow:

* First, set your app's registration page on the Security & Auth --> Configuration page.
* Next, if you wish to use automated email verification, enable Sign-up Email Verification on the Security & Auth --> Configuration page
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page.
* Once you have entered the emails you wish to invite, click the 'Invite User(s)' button. This will send an email to each new user with a link to the registration page that you created.

#### Email verification process

The user enters his/her email, name and password on your registration page:

* After the user submits their registration, Backand sends a verification email to authenticate the user's identity if Sign-up Email Verification is enabled
* If Sign-up Email Verification is not enabled, at this point registration is complete.
* If Sign-up Email Verification is enabled, after the user clicks on the link in the verification email, Backand completes the registration process and redirects to the 'Custom Verified Email Page' URL for your application. Configure this on the Security & Auth --> Configuration page  

For more information, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#sign-up).

### SSO (Single Sign On)
Many organizations make use of tools like Active Directory in order to provide a central source of user-based authentication. This system is often used to drive a Single Sign On (SSO) feature in the organization, allowing users to simply use one set of credentials for all of the apps that they need to work with. Backand provides a method to incorporate SSO functionality using an override server-side action. This action allows you to perform your own authentication (via web-service calls to the domain controller, for example), and lets you return one of three actions.

* Returning 'allow' allows the user access to the requested resource = in this case, you can return additional information in the 'additionalTokenInfo' variable, which is added to the Backand authentication result. You may access this later by using the getUserDetails function of the Backand SDK.
* Returning "deny" overrides Backand's auth, and provides an 'Access Denied' mesage.
* Returning 'ignore' allows you to ignore the custom action, instead relying upon Backand's default authentication methods.

You can configure this action in the Backand dashboard, under Security & Auth => Security Actions.

### Remove user from the app
There are two ways to remove a user from the application. You can permanently remove a user from the application by deleting a user from the Registered Users grid. This requires the user to register again if they wish to continue using your app. Alternatively, you can un-check the 'approved' checkbox in the user's row on the Security & Auth --> Users page. This allows you to reinstate the user simply by re-checking the 'approved' column.

### Anonymous Access
By default the anonymous access is turned on with 'User' role access, which mean any one can register to your app and make CRUD actions on any object.
If you wish to disable anonymous access to your application, go to Security & Auth --> Configuration and set 'Anonymous Access' to off.

<aside class="notice">To better control the permission of your app we recommend to change the anonymous users role to have less access. For more information on anonymous access, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#anonymous-access).</aside>

### Link your app's users with Backand's registered users
```javascript
// Validate Backand Registered User Action
function backandCallback(userInput, dbRow, params, userProfile) {
  var validEmail = function(email)
    {
        var re = /\\S+@\\S+\\.\\S+/;
        return re.test(email);
    }

    // write your code here
  if (!userInput.email){
        throw new Error("Backand user must have an email.");
    }

    if (!validEmail(userInput.email)){
        throw new Error("The email is not valid.");
    }
    if (!userInput.firstName){
        throw new Error("Backand user must have a first name.");
    }
    if (!userInput.lastName){
        throw new Error("Backand user must have a last name.");
    }
}

// Create Backand Registered User Action
function backandCallback(userInput, dbRow, params, userProfile) {

  var randomPassword = function(length){
      if (!length) length = 10;
      return Math.random().toString(36).slice(-length);
  }
    if (!parameters.password){
        params.password = randomPassword();
    }

    var backandUser = {
        password: params.password,
        confirmPassword: params.password,
        email: userInput.email,
        firstName: userInput.firstName,
        lastName: userInput.lastName
    };

    // uncomment if you want to debug
    //console.log(params);
    var x = $http({method:"POST",url:CONSTS.apiUrl + "1/user" ,data:backandUser, headers: {"Authorization":userProfile.token, "AppName":userProfile.app}});

    // uncomment if you want to return the password and sign in as this user
    //return { password: params.password };
    return { };
}
```
[Backand](https://www.backand.com) maintains an internal registered users object which is used to manage your app's
security. However, most apps will have their own 'users' object, which is used when implementing the app's business logic. Recognizing this, we have created automatic and custom trigger actions that can be used to synchronize the two objects. If you have an object in your system named 'users', and if that object has the fields 'email', 'firstName', and 'lastName', then every user that is registered with [Backand](https://www.backand.com) for your app will automatically have an entry created in your custom 'users' object. This takes place no matter how the user is added - both the sign-up API and the [Backand](https://www.backand.com) dashboard will create an automated user record! Additionally, every time you add a user instance to your users object, a new user is created in Backand's internal registered users object. This new user will be given a randomized password, which can then be provided to the user for access to your application.

Below we'll look more in-depth at how this process is managed. Additionally, we'll explore what happens when the users object has an unexpected name (i.e. something other than 'users'), or if the fields of the users object are named differently.

There are 3 sync actions located in Configuration -> Security & Auth that are triggered by Backand user registration:

1. Create My App User
1. Update My App User
1. Delete My App User.

You can directly modify those JavaScript actions if needed.

<aside class="notice">By default, Backand sets the Where Condition of the action to 'false' if it detects that you do not have a users object in your model, meaning that the action will never run.</aside>

If you have a users object in your model, Backand adds the following actions, which run before and during user creation

* Before Create: Validate Backand Register User
* During Create: Create Backand Register User


This action implements the other side of the relationship - after creating an instance in your custom 'users' object, this code creates the corresponding entry in Backand's internal users object. If have named your custom user object anything other than 'users', you can add the above action into that object manually to achieve the same functionality. You will need to adjust the backandUser object creation to use the columns in your specific object, as the userInput object may have a different structure than that assumed above.

### Roles & Security Templates
Each user has a role. When you created your app, you were automatically assigned with an 'Admin' role. The 'Admin' role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.


## Backand Storage
Backand provides you with the ability to upload and delete files to and from Backand's robust storage. This is done on the server-side through Backand's Actions. It doesn't require any additional authentication and it is up to you to decide if and under what restrictions to expose this functionality to the client side. For example, you can restrict certain roles, handle the name of the files, associate the files with objects and manage counts of the amount of files per user.

The files.upload command returns a url that links to the file you uploaded. This is a public url. The storage is managed per Backand app.  

### Custom Server-Side Action Code
```javascript
//Use the following JavaScript as the basis for your file upload action.
//Refer to the template function for information on global variables offered.
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
  console.log(userProfile); // gets the current user role and id that enables you to perform security restrictions
	// upload file
    if (request.method == "POST"){
        var url = files.upload(parameters.filename, parameters.filedata);
        return {"url": url};
    }
    // delete file
    else if (request.method == "DELETE"){
        files.delete(parameters.filename);
        return {};    
    }

}
```
Both upload and delete are written in the same action and the method you use to call it determines the functionality. When the method is POST then the action performs upload and when it is DELETE the action performs delete. You can use the userProfile for restrictions or file name manipulations. You do not need to copy this code. It is ready for you when you click on the Backand *File Storage* action template.

### Angular Client-Side Code
```html
<!-- Angular HTML -->
<body class="container" ng-app="app" ng-controller="DemoCtrl" ng-init="initCtrl()">
  <h2>Backand Simple Upload File</h2>
  <br/>
  <form role="form" name="uploadForm">
    <div class="row">
        <img ng-src="{{imageUrl}}" ng-show="imageUrl" />
        <input id="fileInput" type="file" accept="*/*" ng-model="filename" />
        <input type="button" value="x" class="delete-file" title="Delete file" ng-disabled="!imageUrl" ng-click="deleteFile()" />

    </div>
  </form>
</body>
```

```javascript
// Client-side JavaScript
var myApp = angular.module('app', ['backand']);

// Backand security configuration for your app
myApp.config(function(BackandProvider) {
  // enter your app name here
  BackandProvider.setAppName('YourAppName');
  // enter your app anonymous token here
  BackandProvider.setAnonymousToken('YourAnonymousToken');
})

myApp.controller('DemoCtrl', ['$scope', '$http', 'Backand', DemoCtrl]);

function DemoCtrl($scope, $http, Backand) {

  // Create a server side action in backand
  // Go to any object's actions tab
  // and click on the Backand Storage icon.
  // Backand consts:
  var baseUrl = '/1/objects/';
  var baseActionUrl = baseUrl + 'action/'
  var objectName = 'YourObjectName';
  var filesActionName = 'YourServerSideActionName';

  // Display the image after upload
  $scope.imageUrl = null;

  // Store the file name after upload to be used for delete
  $scope.filename = null;

  // input file onchange callback
  function imageChanged(fileInput) {

    //read file content
    var file = fileInput.files[0];
    var reader = new FileReader();

    reader.onload = function(e) {
      upload(file.name, e.currentTarget.result).then(function(res) {
        $scope.imageUrl = res.data.url;
        $scope.filename = file.name;
      }, function(err){
        alert(err.data);
      });
    };

    reader.readAsDataURL(file);
  };

  // register to change event on input file
  function initUpload() {
    var fileInput = document.getElementById('fileInput');

    fileInput.addEventListener('change', function(e) {
      imageChanged(fileInput);
    });
  }

   // call to Backand action with the file name and file data  
  function upload(filename, filedata) {
    // By calling the files action with POST method in will perform
    // an upload of the file into Backand Storage
    return $http({
      method: 'POST',
      url : Backand.getApiUrl() + baseActionUrl +  objectName,
      params:{
        "name": filesActionName
      },
      headers: {
        'Content-Type': 'application/json'
      },
      // you need to provide the file name and the file data
      data: {
        "filename": filename,
        "filedata": filedata.substr(filedata.indexOf(',') + 1, filedata.length) //need to remove the file prefix type
      }
    });
  };

  $scope.deleteFile = function(){
    if (!$scope.filename){
      alert('Please choose a file');
      return;
    }
    // By calling the files action with DELETE method in will perform
    // a deletion of the file from Backand Storage
    $http({
      method: 'DELETE',
      url : Backand.getApiUrl() + baseActionUrl +  objectName,
      params:{
        "name": filesActionName
      },
      headers: {
        'Content-Type': 'application/json'
      },
      // you need to provide the file name
      data: {
        "filename": $scope.filename
      }
    }).then(function(){
      // Reset the form
      $scope.imageUrl = null;
      document.getElementById('fileInput').value = "";
    });
  }

  $scope.initCtrl = function() {
    initUpload();
  }
}
```

We created a simple example in Angular to experience the upload functionality.
You can see the code in action at [codepen](http://codepen.io/backand/pen/ZQaYEV). The relevant parts of the code are available in the JavaScript pane to the right.

## Backand Hosting
Backand offers high-performance, reliable, and secure hosting for AngularJS applications through the hosting and deployment tool. Built off of AWS, Backand Hosting gives you an effortless way to deploy your Angular project to a hosted server.

Your project files will be available on **https://hosting.backand.io/Your-App-Name**, providing you an easy way to see what gets deployed for your project.


<aside class="notice">
The Backand CLI (command-line interface) requires Node.js and Node Package Manager (NPM). To install Node and NPM, follow the instructions at [https://nodejs.org](https://nodejs.org). This one process handles both NodeJS and NPM.
</aside>

### First Time Installation
```bash
  $ npm install -g backand
  # or use sudo (with caution)
```

The hosting and deployment functionality is provided with the Backand node package. Simply install the package from the command line as follows:


### Updating a Prior Version
```bash
  $ npm update -g backand
  # or use sudo (with caution)
```
If you've installed the Backand node package before, you will need to perform an update to get the new hosting and deployment functionality. Follow these steps to update your locally-installed version of Backand


### Deploying the Angular Project
```bash
  backand sync --app {{appName}} --master {{master-token}} --user {{user-token}} --folder /path/to/project/folder
```
The Backand CLI, which we installed using NPM, provides the capability to deploy your project, as well as sync your local project folder. To deploy and sync, use the following command from the command line:


The parameters for this call are:

  **--app**: The current app name

  **--master**: The master token of the app (get it from Social & Keys)

  **--user**: The token of the current user (get it from Team and click on key icon)

  **--folder**: The path of the local Angular folder to sync and deploy

### Browsing to your app
Your app is now on air, just browse to **https://hosting.backand.com/YourAppName** and see it live.

### Configuring Sync in Gulp

```bash
  # Install the backand hosting gulp plugin with NPM
  npm install backand-hosting-s3
```
```javascript
  // Include the sync module in your project
  var backandSync = require('../sync-module');

  // add your app's credentials to the sts task
  gulp.task('sts', function(){
      var masterToken = "your master backand token";
      var userToken = "your user backand token";
      return backandSync.sts(masterToken, userToken);
  });

  //Configure the task to sync to the 'src' folder
  gulp.task('dist',['sts'], function() {   
      var folder = "./src";
      return backandSync.dist(folder, appName); //appName - if you have only one app you don't need this parameter
  });

  // Add a task to clean the cache
  gulp.task('clean', function() {
      return backandSync.clean();
  });
```
Backand can integrate with your existing [gulpjs](http://gulpjs.com) configuration, allowing you to easily control deployment as you would the rest of your project. Below, we'll create a Gulp task to perform the deployment. This can then be used to deploy your application as a part of your standard build process.

To create the new Gulp deployment task:

1. Install the Backand hosting gulp plugin using NPM


2. At the top of gulpfile.js add the `require` call


3. Set your Backand credentials for the task. Credentials will be stored in file `.backand-credentials.json`:

4. Configure the task to Sync folder './src':

5. Syncing is performed via a local cache file named '.awspublish-<bucketname>'. This file cache can become corrupted
 after multiple uses, so we need to create one more task to perform cleanup after the deployment has completed:

## Realtime Database Communications
Backand's real-time communication functionality is based on the popular open-source Socket.io framework, which lets you add real-time functionality to your application. Backand’s Real-Time Database Communication sends events and JSON-formatted data to any authorized connected client. With Real-Time Communication from Backand, you can send real-time information to your application based on server-side logic in your application's custom actions. With this new feature events are picked up as they happen, rather than having to wait for a user-driven event to trigger a data reload.

Backand’s real-time database communications are completely secure. They provide you with total control over data access, allowing you to restrict transmission of sensitive data to only authorized roles. And since all communication is SSL-encrypted, you don’t have to worry about the security of your data as it is transmitted across the web.

Using the real-time capability can enhance your app with instant updates to any Angular page, including updating charts, counters, logs, and other data-driven elements.

###Setup

```html
<!-- Backand SDK -->
<script src="//cdn.backand.net/vanilla-sdk/1.0.9/backand.js"></script>
<script src="//cdn.backand.net/angular1-sdk/1.9.5/backand.provider.js"></script>   
```

```javascript
  // Configure the SDK to run the socket
  BackandProvider.runSocket(true);
```
1. Upgrade to Backand SDK 1.9.5 or above.
2. Include the Backand SDK in your index.html file:
3. Update Angular configuration section

### Angular client code - Sockets
```javascript
Backand.on('items_updated', function (data) {
  //Get the 'items' object that have changed
  console.log(data);
});
```

Backand's SDK already includes all the code needed for the client-side Socket.IO JavaScript integration. All you need to do is add a listener in your Angular controller or service. In the event that you need to implement your client on a platform other than JavaScript, review the relevant [iOS](http://socket.io/blog/socket-io-on-ios/) or [Android](http://socket.io/blog/native-socket-io-and-android/) documentation.

Using Backand's SDK, you can add event listening to your application with a simple function. You don't even need to provide an emit event to track changes - Backand will automatically update you whenever you change data using Backand's REST API out of the box.

### Server side code - Sockets
```javascript
// Emit to users
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitUsers("items_updated",userInput, ["user2@gmail.com","user1@gmail.com"]);
}

// Emit to a role
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitRole("items_updated",userInput, "Role");
}

// Emit to all
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitAll("items_updated", userInput);
}
```
When working with a Custom JavaScript Action in the Backand dashboard, you can add the "emit" command to notify the client of updates based upon events and sent data that are important to your use case. The actions can be based on database triggers (Create, Update or Delete), or in an on-demand action called from client-side logic.

There 3 type of emit commands you can use in order to notify consuming applications of event data:

* socket.emitUsers: `socket.emitUsers('event_Name',object, []);`

This command sends event only to users specified in the second argument, which should be formatted as an array. To make this array dynamic, you'll need to use a Backand Query action. First call the Query to obtain the users, then convert the JSON into an array of user IDs (email addresses).

* socket.emitRole: `socket.emitUsers('event_Name',object, 'role');`

This command sends the event to all the users in a specified role. Be aware that this command will override any data security filter you may have configured for your application.

* socket.emitAll: `socket.emitAll('event_Name',object);`

This command sends event to all connected users, regardless of their role or permission settings. Be aware that this command vertices any data security filters you may have configured for your application. Use this emit type for general event broadcasting.

## Custom Actions

## Queries

## NoSQL Query Language
