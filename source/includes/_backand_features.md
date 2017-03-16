# Backand Features
This section contains documentation on the features offered by Backand. We will cover the following functional areas:

* Security and Authentication - a summary of the security features offered by Backand
* Backand Storage - an overview of our file storage options
* Backand Hosting - an overview of our hosting options
* Custom Actions - an overview of the ability to add custom actions and event-based functionality
* Queries - an overview of the on-demand query functionality
* NoSQL Query Language - an overview of our NoSQL-style query language

## The User Object and Security

This section describes how to manage your app's users within Backand's user security paradigm. An important element to note up front is that, by default, [Backand](https://www.backand.com) provides two independent user objects. One user object is handled entirely by Backand, and is responsible for authentication and role-based security. The data for this user object is stored in Backand's database, and as such cannot partake of your app's custom logic. The second user object is offered in the default data model for a Backand application, and is titled 'users'. This object represents a user of your application, as opposed to a generic Backand user, and allows you to associate the user with various other objects in your application. Backand provides sync actions that allow you to keep these two user object lists up-to-date (see [Link your app's users with Backand's registered users](#link-your-apps-users-with-backands-registered-users) for more info). We highly recommend running the [todos-with-users app](https://github.com/backand/todos-with-users) in addition to reading this documentation. This simple app covers most of the user management use cases for a Backand application, such as allowing users to read all of your app's data but only allowing them to create and update their own objects, or restricting anonymous users to read-only access, or creating an Admin role that has full read-write-update-delete access to your application's objects.

### Authentication with OAuth 2.0
```shell
# To obtain an access token using a username and password, use the following
# command. This uses environment variables to ease presentation of the
# information, and supplies the fields as data arguments to the cURL command.
# $USERNAME - user name (email address) to authenticate
# $PASSWORD - password for the user
# $APP_NAME - the name of the backand application you are connecting to
curl https://api.backand.com/token -d username=$USERNAME -d password=$PASSWORD -d grant_type=password -d appName=$APP_NAME

# To obtain an access token using a Social Media master key (for connecting via
# social media providers, like Facebook in the example below), use the following
# command with parameter values as follows:
# $SOCIAL_MEDIA_KEY - your app's social media access token
# $APP_NAME - your Backand app's name
curl -X GET https://api.backand.com/1/user/facebook/token?accessToken=$SOCIAL_MEDIA_KEY&appName=$APP_NAME&signupIfNotSignedIn=true

# You can then use the access token returned by this call to call any Backand API
# for your application:
# $ACCESS_TOKEN - the token obtained via a call to the token endpoint
curl https://api.backand.com/1/objects/items -H "Authorization: Bearer $ACCESS_TOKEN"
```

The default authentication setup for [Backand](https://www.backand.com) applications relies on [OAuth2](http://oauth.net/2/) to provide tokenized authentication. By logging in with your username (your email address), your password, and your app name, you receive an authentication token that is valid for 24 hours. This token is required for all communication with Backand, and as such we highly recommend that you use [Backand's SDK](https://github.com/backand/vanilla-sdk) to help you manage the access token. You can change the default expiration of 24 hours by using a refresh token, which allows you to reuse the authentication token indefinitely. The refresh token is an encrypted hash of the master and user keys. You can revoke one (or all) of your user's refresh tokens by changing the refresh token and requiring users to re-authenticate. For more information, see the [API Description](#vanilla-sdk).

####Parameters

* **signupIfNotSignedIn** - (Optional, default false) - If the user tries to sign in without first registering for the application, the user will receive an error message ("The user is not signed up to {appName}"). If this value is set to true, then the user will be automatically registered with the app if they have not yet been signed up

#### Sample response
```json
// Sample response
{
  "access_token": "ThuuBiuTS9grkzbPW-yL5z3dd_Q48-Ml4oHCffgbWRUDr1rFIY_nYIqaL-he09sCicEVNE_wJCxZ4QS0E3SlG-fZOSOOHDlzORrVagcGvBtZoqTByvhiXgcwXPOkmeD8U1bZaAi8vLEr_wUY6f_rse9o8GKs5cjRpBZENurSytsXhXsv6XpSFcZUr5n7Za_nu5HDth2bOuM_2e-Kn3yOc2GDz9qXjZm_UQ9oMfvJnzjY16Qsw7_ynZAbRa4m6lRtXwsHunqv_R_8uhMGNwTxyg",
  "token_type": "bearer",
  "expires_in": 86399,
  "appName": "bkndkickstart",
  "username": "start@backand.io",
  "role": "User",
  "firstName": "Backand",
  "lastName": "Start",
  "fullName": "Backand Start",
  "regId": 950007,
  "userId": "2"
}
```

The call to the token endpoint returns all of the information you need to use a token to connect to your Backand application. The access_token provided is authenticated with a single user in your app, and is provided an expiration time in seconds. You are also given details of the authenticated user, including their username, first and last name, user ID, and their security role.
### Basic authentication
```shell
# Perform basic authentication via header
curl https://api.backand.com/1/objects/items -u <master key>:<user key>
# Perform basic authentication using URL parameters
curl https://api.backand.com/1/objects/items?authorization=basic+<master key>:<user key>
```

You can use [basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication) to ease working with other servers - simply enter the master key as the username, and the user key as the password. You can use this approach to obtain a list of Items by using the following API call:

You can also send the basic authorization token through as a query string.

<aside class="warning">Do not use this method from the client side as it exposes your app's secret token, which can be used to perform any action in your system. This method is for server-side calls only.</aside>

### Anonymous authentication
```shell
# Perform anonymous authentication through headers
curl https://api.backand.com/1/objects/items -H "AnonymousToken: <anonymous token>"
# You can also send the anonymous token through in a query string:
curl https://api.backand.com/1/objects/items?AnonymousToken=<anonymous token>
```

Backand also offers Anonymous Access, which allows you to access your application without the need to authenticate via username and password. By passing this value in a request header (e.g. AnonymousToken=<your token here>) you can perform api actions for your application

### User Registration

Registering with [Backand](https://www.backand.com), and creating an application, automatically sets you as a user with an 'Admin' role in your new project (see [Roles and Security Templates](#roles-security-templates) for more info). By default your application is marked public which mean any user can register to your application.

These users are assigned a default role 'User', which has full CRUD access to your app. Your roles will need to be configured when you enable public usage of your app (see [Roles and Security Templates](#roles-security-templates) for more details).


```javascript--general
  //Create new Security Action in Before Create trigger with the following
  //code (this will update the role to 'Public'):
  userInput.Role = 'Public';
```
<aside class="notice">
For security reasons you cannot change the role from the sign-up API - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -&gt; Registered Users page, or by creating a custom server-side action with Admin rights (see JavaScript tab).
</aside>

### Saving Additional Parameters During Sign-up

In many cases, we would like to collect additional information from the user during sign up. We can automatically populate the related fields on the Users object as a part of the sign-up request by using the `parameters` parameter for the sign-up call. The server, during the 'Create My App User' action, will translate these key-value pairs into the appropriate fields on the 'Users' object - as such, it is important that all fields provided already exist on the Users object. The following example demonstrates specifying a user's "company" name as a part of the sign-up:

```javascript
    Backand.signup(firstName, lastName, username, password, password, {"company": self.company}).then(...);
```
1. Update the Model and add the field `company` to the `users` object:
    1. Go to `Objects` --> `Model`
    2. Add this line as a data field in the `users` object model: `"company": {"type": "string"}`
    3. Click on `Validate & Update`
2. When calling `Backand.signup()`, send the `parameters` object through as the last input parameter for the request. The code will resemble the following:
3. There's no need for server-side modifications - values for "parameters" are handled automatically by the action 'Create My App User' (found in the `Security Actions` section).
4. Modify the UI to collect the `company` name for the user.

Now, when sign-up is completed, you can see the company name in the `users` object's Data tab.

### Private app

You can change your app, which is public by default, to be private. This can be changed in the dashboard by setting the Public flag for your application to 'false' in the Security & Auth --> Configuration menu. For a private app, the registration steps are as follow:

* First, set your app's registration page on the Security & Auth --> Configuration page.
* Next, if you wish to use automated email verification, enable Sign-up Email Verification on the Security & Auth --> Configuration page
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page.
* Once you have entered the emails you wish to invite, click the 'Invite User(s)' button. This will send an email to each new user with a link to the registration page that you created.

### Email verification process

The user enters his/her email, name and password on your registration page:

* After the user submits their registration, Backand sends a verification email to authenticate the user's identity if Sign-up Email Verification is enabled
* If Sign-up Email Verification is not enabled, at this point registration is complete.
* If Sign-up Email Verification is enabled, after the user clicks on the link in the verification email, Backand completes the registration process and redirects to the 'Custom Verified Email Page' URL for your application. Configure this on the Security & Auth --> Configuration page  

For more information, see our [Vanilla SDK documentation](#vanilla-sdk)

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

<aside class="notice">To better control the permission of your app we recommend to change the anonymous users role to have less access. For more information on anonymous access, see the <a href="#backand-init">API Description</a>.</aside>

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
[Backand](https://www.backand.com) maintains an internal registered users object which is used to manage your app's security. However, most apps will have their own 'users' object, which is used when implementing the app's business logic. Recognizing this, we have created automatic and custom trigger actions that can be used to synchronize the two objects. If you have an object in your system named 'users', and if that object has the fields 'email', 'firstName', and 'lastName', then every user that is registered with [Backand](https://www.backand.com) for your app will automatically have an entry created in your custom 'users' object. This takes place no matter how the user is added - both the sign-up API and the [Backand](https://www.backand.com) dashboard will create an automated user record! Additionally, every time you add a user instance to your users object, a new user is created in Backand's internal registered users object. This new user will be given a randomized password, which can then be provided to the user for access to your application.

Below we'll look more in-depth at how this process is managed. Additionally, we'll explore what happens when the users object has an unexpected name (i.e. something other than 'users'), or if the fields of the users object are named differently.

There are 3 sync actions located in Configuration -> Security & Auth that are triggered by Backand user registration:

1. Create My App User
1. Update My App User
1. Delete My App User

You can directly modify those JavaScript actions if needed.

<aside class="notice">By default, Backand sets the Where Condition of the action to 'false' if it detects that you do not have a users object in your model, meaning that the action will never run.</aside>

If you have a users object in your model, Backand adds the following actions, which run before and during user creation

* Before Create: Validate Backand Register User
* During Create: Create Backand Register User


This action implements the other side of the relationship - after creating an instance in your custom 'users' object, this code creates the corresponding entry in Backand's internal users object. If have named your custom user object anything other than 'users', you can add the above action into that object manually to achieve the same functionality. You will need to adjust the backandUser object creation to use the columns in your specific object, as the userInput object may have a different structure than that assumed above.

### Roles & Security Templates
Each user has a role. When you created your app, you were automatically assigned with an 'Admin' role. The 'Admin' role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.

### Security Actions
Backand offers a number of pre-defined security actions that can be used to manage your application's security processes. These are available in the app dashboard under **Security & Auth -> Security Actions**. See [the Security Actions Documentation](#security-actions84) for more details.

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
Backand offers high-performance, reliable, and secure hosting for AngularJS applications through the hosting and deployment tool. Built as a cloud-based service, Backand Hosting gives you an effortless way to deploy your Angular project to a hosted server.

Your project files will be available on **https://hosting.backand.io/Your-App-Name**, providing you an easy way to see what gets deployed for your project.


<aside class="notice">
The Backand CLI (command-line interface) requires Node.js and Node Package Manager (NPM). To install Node and NPM, follow the instructions at <a href="https://nodejs.org">https://nodejs.org</a>. This one process handles both NodeJS and NPM.
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

```html    
<!-- Backand Socket Client -->
<script src="lib/socket.io-client/dist/socket.io.js"></script>
<!-- Backand SDK -->
<script src="//cdn.backand.net/vanilla-sdk/1.0.9/backand.js"></script>
<script src="//cdn.backand.net/angular1-sdk/1.9.5/backand.provider.js"></script>   
```

```javascript--persistent
  // Configure the SDK to run the socket
  BackandProvider.runSocket(true);
```

Backand's real-time communication functionality is based on the popular open-source Socket.io framework, which lets you add real-time functionality to your application. Backand’s Real-Time Database Communication sends events and JSON-formatted data to any authorized connected client. With Real-Time Communication from Backand, you can send real-time information to your application based on server-side logic in your application's custom actions. With this new feature events are picked up as they happen, rather than having to wait for a user-driven event to trigger a data reload.

Backand’s real-time database communications are completely secure. They provide you with total control over data access, allowing you to restrict transmission of sensitive data to only authorized roles. And since all communication is SSL-encrypted, you don’t have to worry about the security of your data as it is transmitted across the web.

Using the real-time capability can enhance your app with instant updates to any Angular page, including updating charts, counters, logs, and other data-driven elements.

###Setup


1. Upgrade to Backand SDK 1.9.5 or above.
2. Include the Backand SDK and Socket.io in your index.html file
3. Update Angular configuration section

```shell--persistent
bower install socket.io-client
```
<aside class="notice">You can also install socket using a package manager like bower</aside>

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
In Backand's system, you can create server-side activity called Actions. These actions can be used for the purpose of security, integration, performance, notification and data integrity, among others, providing you with more flexibility in your app's design. There are two types of Actions that can be created. The first are initiated via a direct web request. These are known as "On Demand" actions. Additionally, you can create automated actions that take place based upon a data interaction event. These automated actions can occur whenever you create, update, or delete an item in your system. On Demand actions are associated with a specific object, and can be found on the `Object --> {name}` page in the Actions tab. The automated Create, Update and Delete actions are associated with a specific object that is compatible with a specific row in a table, while On Demand actions make association with a specific role optional.

There are 4 kinds of actions that can be created:

* Server side JavaScript code actions
* Server side node.js code actions
* Transactional database script actions
* Send Email actions

All 4 types of actions use the following common parameters:

* A Where condition - a SQL where clause that determines if the action will be performed.
* Input Parameters, added to the query string of the request that triggers the action, that will serve as variable values that you can supply to your action's code. These parameters will serve as tokens in the action definition and will be replaced with the actual values when the code executes.

### Server-side JavaScript Code

```javascript
function backandCallback(userInput, dbRow, parameters, userProfile) {
    // write your code here
    return {};
}
```

You can run standard JavaScript on the server. It runs on the [V8 engine](http://en.wikipedia.org/wiki/V8_(JavaScript_engine)). To execute the JavaScript action, put your code into the following function:


The function parameters are:

* `userInput`: This parameter is only provided for Create and Update actions, and is the object that was sent to the action. It is null for Delete actions, as well as for On Demand actions.
* `dbRow`: This parameter is populated in After Create, Update, and Delete automated actions, and if you supply an optional ID to an On Demand action. The dbRow parameter will contain the row's entry in the database prior to any changes made.
* `parameters`: This parameter represents the variables sent in the query string for the action.
* `userProfile`: This parameter stores the current username, the user's role, and the access token used by the user to perform the action. It is of the format {"username": "string", "role": "string", "token": "string"}.
* `CONSTS`: Primarily used to obtain the API URL from `CONSTS.apiUrl`
* `config`: Global configuration. You can maintain a global JSON configuration for your app. Your JSON configuration is consumed in the Config action. To update the configuration JSON, go to section `General` in the `Settings` menu on the Backand dashboard.
* `socket`: `socket` is an object that allows you to send real-time communication events and data to the client. `socket` has 3 methods: `emitUsers`, `emitRole`, and `emitAll`. Refer back to our documentation on [Realtime Database Communication here](#realtime-database-communications) for more information.

#### Making HTTP Requests from a custom action
```javascript
// GET example:
var response = $http({method:"GET",url:CONSTS.apiUrl + "/1/objects/objectexample",
                       params:{filter:[{fieldName:"fieldexample", operator:"contains", value:"somestring"}]},
                      headers: {"Authorization":userProfile.token}});

// POST example:
var response = $http({method:"POST",url:CONSTS.apiUrl + "/1/objects/objectexample",
                      data:{fieldexample1:"somevalue",fieldexample2:"somevalue"},
                      headers: {"Authorization":userProfile.token}});

// PUT example:
var response = $http({method:"PUT",url:CONSTS.apiUrl + "/1/objects/objectexample/5",
                      data:{fieldexample1:"somevalue",fieldexample2:"somevalue"},
                      headers: {"Authorization":userProfile.token}});

// DELETE example:
var response = $http({method:"DELETE",
                      url:CONSTS.apiUrl + "/1/objects/objectexample/5",
                      fieldexample2:"somevalue"},
                      headers: {"Authorization":userProfile.token}});
```

In addition to the above parameters, you're also given access to `$http`. `$http` is a service for HTTP calls, similar to Angular's $http without the promise (since it is a server side function it always runs in sync). [See the full API description for more details](#vanilla-sdk).

To make a request, simply feed a parameter hash into the $http object. The available parameters are:

* method - `GET`, `POST`, `PUT`, or `DELETE`
* url - Build this using `CONSTS.apiUrl` as a base
* params - parameters for the call, such as filter data (**GET only**)
* data - data for the call, such as field data for a `POST` call
* headers - the headers to use with the request, typically just the authorization header

#### A Note About The Authorization Header:  
Sending the authorization header to the $http function is optional. When you make a $http request with an authorization header and the value of the userProfile.token (as in the examples above), the request will run in the context of the current user and with his assigned role. Alternatively, if you choose not to send an authorization header, the action will run in the context of an admin role. Send the authorization header with the request if you are going to use information about the current user in the action, otherwise you do not need to do so.

#### Debugging
Debugging should be done using either console.log or console.error. For example, to dump the contents of variable
`object`:

`console.log(object)`

`console.error(object)`

These are sent to the Logging section of the Backand dashboard, under `Log` -> `Console`.

#### Error Handling
If your code results in an error (for example, if you write the following: `throw new Error("An error occurred!")`), the request will return HTTP status 417, and the response body will contain the associated error message.

These are also sent to the Logging section of the Backand dashboard, under `Log` -> `Server Side Exceptions`.

#### Return values
Triggered actions will have a response that matches the format expected by the triggering call (such as the return value of a `CREATE` call).

On Demand actions, though, will return whatever value is returned by the custom server code, which can be any properly-formatted JSON string.

### Server-Side Node.js Code
Using Backand, you can develop distributed Node.js actions and host them with your Backand application - no additional servers needed! You can use the Server-Side Node.js action to work with any NPM package, build sophisticated action behaviors, perform complex coding tasks, and more.

For Server-Side Node.js Code actions, you develop the code on your local machine. The code is then deployed to, and runs on, Backand's server. It functions just like any other Node.js project and can be fully debugged locally and, once you've finished making changes, you can use the "deploy" command to publish the changes to your Backand application.

Follow these steps to create and run a Server-Side Node.js Code Action:

* First name the action, and use the init command by copy-pasting "backand action init..." to a command line within the folder in which you'll be developing your action. The action init command creates two folder levels on your local file system. The top level folder will be the name of the object controlling the action, while its child folder is the name of the action you are working with. We recommend that you always run the **action init** command from the root folder of the app's project when creating additional actions. This means you will have a sub-folder for each object, and under it, a sub -folder for each action.
* Next, build your Node.js code in the `action` folder like any Node other project using your preferred IDE. Add as many npm
packages as you need. Note - Your code **must** start with the index.js file - Backand uses this as the starting point for your Node.JS action.
* To debug your code locally, run the debug.js file. Debug.js is provided by the init command, and is ignored when deploying the code to Backand.
* To run your code on the Back& server, use the deploy command by copy-pasting "backand action deploy..." into the command line used in the first step. The
deploy command will be available on the action page **after** the action has been initialized with "backand action init...".

You are now ready to develop your code locally, and test the action on the Backand Dashboard for your application!

#### Backand CLI
```bash
    $ npm install -g backand
    # or use sudo (with caution)
```

Backand uses the Backand CLI to control deployment and initialization. The CLI requires that Node.js and NPM are both installed. You can install them on your local machine by following the instructions at [https://nodejs.org/](https://nodejs.org/).


Once you've set up Node and NPM, use NPM to install the Backand CLI as a global package.

#### Initialize action
```bash
    $ backand action init --app <app name> --object <object name> --action <action name>  --master <master token> --user <user token>
```

To initialize the node.js code for the action on your local machine, use the `action` command on the command line in the folder that will host your action's code

The parameters for this call are:

*  `--app`:		  The current app name  
*  `--object`:		The object that the action belongs to  
*  `--action`:		The action name  
*  `--master`:		The master token of the app (obtained from the Social & Keys section of the app's Security & Auth configuration)  
*  `--user`:		  The token of the current user (available from the TEam section of the app's Security & Auth configuration - simply click on key icon next to an authorized user)  


#### Deploy action
```bash
    $ backand action deploy --app <app name> --object <object name> --action <action name>  --master <master token> --user <user token>
```

To deploy your local Node.js code to Back&, use the `deploy` command with the Backand CLI

The parameters for this call are:

*  `--app`:		  The current app name  
*  `--object`:		The object that the action belongs to  
*  `--action`:		The action name  
*  `--master`:   The master token of the app (obtained from the Social & Keys section of the app's Security & Auth configuration)  
*  `--user`:     The token of the current user (available from the TEam section of the app's Security & Auth configuration - simply click on key icon next to an authorized user)  
*  `--folder`:   (Optional) The folder to deploy. By default the deployment occurs in the current folder

#### Add Backand SDK to Node.js
```bash
    $ npm install backandsdk --save
```

To work with objects or other actions in Backand, you need to install the Backand SDK for Node.js. The Backand SDK ships with several code samples that demonstrate how to access other Backand functionality.


### Transactional Database Scripts

Transactional database scripts are SQL scripts that run within the same transaction context as the triggering action, provided that the event occurs during the object event "During the data save before the object is committed". This means that if the Create, Update or Delete request fails then your script will be rolled back like any other transaction.


### Send Emails

Send Email actions, in addition to common parameters, allow you to also supply the usual email fields: To, Cc, Bcc, From, Subject and Message. You can additionally provide an object ID to obtain a deep object to use in the action.



## Queries
You can create and call your own custom queries in your application. You can use parameters as tokens that will be replaced with the actual parameter values, similar to how [custom actions](#custom-actions) work. You can create your own filters, sorting, and paging, as well as use aggregation to summarize information.

Click [NoSQL Query Language](#nosql-query-language) for instruction on how to build queries with noSQL language or
use common MySQL syntax.

## NoSQL Query Language
This query language is inspired by [MongoDB](https://www.mongodb.com/).

A query consists of these parts:

1. fields to be extracted
2. table to extract the records from
3. expression for filtering the table rows
4. groupby - fields to group the data under
5. aggregate functions to be applied to columns in fields
6. orderby - fields to order the return data by
7. limit - an integer number of records to return.

Only the table and expression parameters are mandatory.

### Construction and Translation
```SQL
  SELECT fields with aggregation
  FROM table
  WHERE expression
  GROUP BY groupby
  ORDER BY orderby
  LIMIT limit
```

The NoSQL queries are then constructed into a SQL query. Each component of a NoSQL query hash corresponds to a different portion of the SQL query that ends up being run against your database. See the example query to the right to get a feeling of how the query translates into SQL statements.

### Format
```JSON
{
  "object": "String",
  "q": "Expression",
  "fields": "Array of String",
  "groupBy": "Array of String",
  "aggregation": "Object mapping fields to aggregate functions"
}
```
NoSQL queries are constructed using JSON objects. The keys are mapped into their respective SQL keywords in the back-end before interacting with your database.

For example, the shortest query you can write would be `{ "object": "table", "q": "query expression" }`    

This NoSQL object is converted into the following SQL

`SELECT * FROM table WHERE query`

### Example - String Comparison
```JSON
{
    "object": "employees",
    "q": {
        "position" : "Sales Manager"  
    },
    "fields": ["name", "salary"]
}
```

A simple query can pluck the `name` and `salary` fields from an object named `employees`. The query sample to the right executes this query for all employees with a position of "Sales Manager"

### Example - Constant Value Comparison
```JSON
{
    "object": "employees",
    "q": {
        "age": { "$lt" : 25 }
    }  
}
```
Queries can also be used to compare an object's  fields to constant values using common comparison operators. For example, to retrieve all fields for all employees under the age of 25, you can use the query to the right.

### Example - Range-based comparison
```JSON
{
  "object": "employees",
  "q": {
    "age": {
      "$between": [25, 40]
    }
  }  
}
```
You can use the `$between` operator to retrieve all records with values that lie between two specified endpoints. For example, the query to the right retrieves all employees with an age between 25 and 40.

### Example - Geographic Data Comparison
```JSON
{
    "object": "city",
    "q": {
        "location" : { "$within" : [[32.0638130, 34.7745390], 25000]
    }
  }
}
```

You can use the `$within` operator to locate all records within a geographic range. The parameters for this comparison are provided as an array of two elements. The first element is a tuple containing latitude and longitude, e.g. `[32.0638130, 34.7745390]`, while the second is a distance in meters. To retrieve all cities within 25 km (25000m) of a given latitude and longitude, you can use a query like the one to the right.

### Expressions

An expression can be either an AND expression, an OR expression, or a UNION query.

#### AND expressions
```JSON
{ "position": "Sales Manager", "age" : { "$lt" : 25 }, "city": "Boston" }
```

An AND expression is a conjunction of conditions on fields. An AND expression is JSON of the form `{ A: condition, B: condition, ... }`

For example, to retrieve all employees that are 25-years-old, a Sales manager, AND live in Boston, you could use the query on the right.


#### OR expressions
```JSON
{ "$or": [ { "num_employees": { "$gt": 30 } }, { "location": "Palo Alto" }  ]  }
```

An OR expression is a disjunction of conditions, `{ $or: [ Expression1, Expression2, ...   ] }`

For example, use the query to the right to find all offices that are either larger than 30 employees, or located in Palo Alto.


#### UNION queries
```JSON
{
    "$union":   [
        {
            "object" : "Employees",
            "q" : {
                "$or" : [
                    {
                        "Budget" : {
                            "$gt" : 20
                        }
                    },
                    {
                        "Location" : {
                            "$like" :  "Palo Alto"
                        }
                    }
                ]
            },
            "fields": ["Location", "country"]
        },
        {
            "object" : "Person",
            "q" : {
                "name": "john"
            },
            "fields": ["City", "country"],
            "limit": 11
        }
    ]
}
```
A UNION query is a union of the results of queries, and will have the general format of `{ $union: [ Query1, Query2, ...   ] }`. The query to the right, for example, combines a query looking for employees with a budget of 20 OR a location like Palo Alto with a query that retrieves the city and country fields for all entries in the `Person` table with the name "john", limiting the results to 11 records. The end result will combine both queries into a single response object.



### Conditions on Fields

Formally, a condition on a field is a key-value expression of the form `{ Key : ValueExpression }`, where the fields are defined as follows:

* `Key` - name of the field
* `ValueExpression` - An expression which has one of the following forms:
  1. Constant - is the field value equal to the constant
  2. Comparison with a comparison operator to a constant
  3. Inclusion or exclusion in result of a sub query
  4. Negation of another comparison

You can perform a number of different tests on objects using conditions. Using conditions, you can:

1. Test equality of field to a constant value, e.g.  `{ A: 6 }` => Is A equal to 6?
2. Compare a field using a comparison operator, e.g. `{ A: { $gt: 8 }}` => Is A greater than 8? The set of comparison operators is quite extensive and includes: `$lte`, `$lt`, `$gte`, `$gt`, `$eq`, `$neq`, `$not`, `$within`, `$between`
3. Test if the value of the field is `IN`  or `NOT IN` the result of a sub-query.
4. Test for the negation of a comparison. For example, to test if the location field is not Boston, we can use`{ "location": { "$not" : "Boston" }}`
5. Test for presence of a value. For example, if we want to test if a middle name field exists, we can do use `{ "middleName": {"$exists": true} }`


Negation may sometimes be swapped for comparison. For example, to test if the location field is not equal to Paris, we can use negation: `{ "location": { "$not" : {"$eq": "Paris" } } }`

Another option is to use a not-equal operator: `{ "location": { "$neq": "Paris" } }`

### Sub Queries
> This JSON can be used as a sub-query to retrieve the ID of all departments in the city of New York.

```JSON
{ "object": "department", "q": { "city" : "New York" }, "fields" : ["id"]}
```

>Using the above sub-query, we can now test a new field - dept_id - with respect to the results of the sub-query. We simply use the `$in` operator, and the query, as seen below.

```JSON
{
  "dept_id": {
    "$in": {  
      "object": "department",
      "q": { "city" : "New York" },
      "fields" : ["id"]
    }
  }
}
```
> In this example, the '`deptId` field is a reference field referring the employees table to the department table. We can now use this sub-query as a part of a larger query retrieving all employees employed in departments that are located in New York.

```JSON
{
  "object": "employees",
  "q" : {
    "deptId" : {
      "$in": {
        "object": "department",
        "q": {
          "city" : "New York"
        },
        "fields" : ["id"]
      }
    }
  }
}
```
>If we wanted to look at a more complex query, we could modify this a bit. Let's say we wanted to retrieve all employees whose department is located in New York, but the employee is located in Boston. To accomplish this, we use an AND expression to combine the two conditions:

```JSON
{
    "object": "employees",
    "q" : {
        "deptId":
        {
            "$in": {
                "object": "department",
                "q": {
                    "city" : "New York"
                },
                "fields" : ["id"]
            }
        },
        "location": "Boston"
    }
}
```
You can use sub-queries to established a reduced scope of your objects to work with. They can then be composed into larger queries, providing you with extra flexibility in retrieving objects from your app.

<aside class="notice">This technique relies upon retrieving a single field from the sub-query. Using more than one field would prove more complex and is neither recommended nor supported.</aside>

### Group By Queries
> To group by 'Country', and then concatenate the 'Location' field, use the following example code:

```JSON
{
    "object" : "Employees",
    "q" : {
        "$or" : [
            {
                "Budget" : {
                    "$gt" : 20
                }
            },
            {
                "Location" : {
                    "$like" :  "Palo Alto"
                }
            }
        ]
    },
    "fields": ["Location", "Country"],
    "order": [["Budget", "desc"]],
    "groupBy": ["Country"],
    "aggregate": {
        "Location": "$concat"
    }
}
```
A group by query aggregates on fields, and then applies aggregation operators to the specified fields.



### Algorithm to Generate SQL from JSON Queries
```javascript
  transformJson(json, sqlSchema, isFilter, callback)
```    
>The result is a structure with the following fields:

```JSON
{
    "str": "<SQL statement for query>",
    "select": "<select clause>",
    "from": "<from clause>",
    "where": "<where clause>",
    "group": "<group by clause>",
    "order": "<order by clause>",
    "limit": "<limit clause>"
}
```

The algorithm transforms from JSON to SQL using a top-down transformation.

#### Usage

You can call the function by using the Javascript call `transformJson`. The parameters to this are as follows:

1. `json` - JSON query or filter
2. `sqlSchema` - JSON schema of database
3. `isFilter` - boolean - true if 'json' is a filter
4. `callback` - 'function(err, result)', called upon completion

#### Escaping

All constants appearing in the JSON query are escaped when transformed into SQL.

#### Filters
>  Variables take the form of:

```javascript--general
  {{<variable name>}}
```

You also have the ability to mark a particular NoSQL query as a filter. This allows you to use variables in your query, which are populated on the server side from either parameters sent in with the filter, or from database data in your system.


<aside class="notice">Variables should be enclosed in quotes (i.e. '{{variable_name}}' instead of {{variable_name}}) so that the final object sent to the server can be marked as valid JSON.</aside>

Variables are not escaped when used as part of a filter or query - only constants can be escaped by Backand. With this in mind, you want to make sure that variables tied directly to user input are properly sanitized before being sent to the back-end. The SQL statement generated for the filter object will include the variables you provide. The variables will be substituted for the equivalent values prior to the execution of the query.
