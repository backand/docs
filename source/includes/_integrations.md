# Integrations
The following set of documentation looks at integrating Backand with a number of different types of providers. Also included are some simple Backand-powered applications that can be used either as a starting point for your development, or as a guide for integrating Backand into a larger application.

## ToDo App with users
Recently, we released a tutorial on the [Backand GitHub page](https://github.com/backand/angular-yeoman-todos/tree/todo-with-users) that shows off a lot of the features available by default in a Backand-powered application. After completing the tutorial, you'll have a simple ToDo list application with full support for user and role-based security. Below we'll discuss each of the features demonstrated in the tutorial, and cover any items remaining that are necessary for a full deployment.

### RESTful API Out-of-the-Box

One of the first things we do in the tutorial, after actually creating the application with Backand, is create a new database using our JSON-based schema language. This powerful tool allows you to quickly build a database and the associated tables and objects without having a client installed. More importantly, once the database has been created, Backand automatically creates a RESTful API based on your underlying database schema. This allows you to have immediate access to your database via a series of REST API calls that create, retrieve, update, and delete records at whim. With some simple JavaScript, you can quickly build out a data layer that would take months in a stand-alone project! Furthermore, the moment you make a change to your schema, Backand will detect the change and update the API endpoints accordingly. This automated updating greatly eases the process of migrating database schema changes, and takes some of the load of server programming off your shoulders, allowing you to focus on your application's functionality.

### Security Roles

While the RESTful API is indeed a powerful tool, the main goal of this tutorial is to explore user- and role-based security. Through Backand's dashboard you are granted a powerful array of tools that you can leverage to secure your underlying application. You can assign user-specific security settings for each object in your app, including everything from table update calls to custom triggers and actions.

However, doing this for every user that signs up for your application would be incredibly tedious. Luckily, Backand provides User Roles that you can assign to the users in your application. Roles set a basic template for interacting with your application's API, allowing you to restrict all users with that role to only specific actions in your application. These roles can also be overridden at an endpoint level, allowing you to grant roles specific access to resources while still maintaining their general security profile.

After the tutorial has been completed, you will have three roles for your users – administrators who have full control, users who can view all records in the application (but only update or delete those that they create themselves), and read-only users that are only able to view the ToDo items in your application.

### Login and Anonymous Access

In addition to security roles, this tutorial will introduce you to the user access functionality available in Backand. You'll implement a mechanism for inviting users to register for your application, and enable two-step verification for your application's users, and all of the complexity will be managed by Backand! Furthermore, you'll learn how to enable anonymous access to your application through a few simple administrative dashboard settings. After the tutorial has been completed, you will have a fully secure application that allows you to invite users to the system, automatically assign them a security role, and even enable anonymous access to your ToDo list should you wish to do so.

### Remaining Items

While the tutorial will walk you through the entire application – from inception to completion – there will still be a few tasks remaining before you can show off your new Backand-powered ToDo list app. The first section of the tutorial focuses on setting up a local application development environment, which allows you to develop and run the code on your local machine. However, once you've implemented the code and are ready to show it off, you still need to track down a web server to use for deployment. Luckily this web server does not need to be particularly robust, as your application requires zero server-side customization – simply deploy the application code to a publicly-accessible folder and you are set!

Once you've published your application on a web server, you have a few more steps remaining in order to fully deploy the application. As a part of the tutorial you set several internal sign-up and authentication URLs that point at your local development instance. Obviously these will no longer function once you have deployed the code! Simply change the appropriate user security settings in your application's Backand dashboard, and your application will be ready to show the world!

### Summary

This tutorial gives you a quick look at the power offered by Backand's back end tools and APIs. You'll explore creating a database and implementing a RESTful API for that data with zero effort. You'll customize your application's security and learn how to restrict users based on role and based on endpoint. You'll even spend some time working with custom server-side actions that execute on-demand. By the time you have finished the tutorial, you will have implemented a secure ToDo list application with user authentication – and all with minimal effort!

## Ionic With Backand
Today many AngularJS developers are using Ionic to build the user interface for their mobile apps.  AngularJS provides the app structure.  But these mobile apps still need a backend.  In this blog we’ll give you the recipe you need to build an Ionic mobile app with Backand’s Backend as a Service (BaaS). Review the working demo [here](https://github.com/backand/simple-rest-ionic).

### What is Ionic?

The [Ionic](http://ionicframework.com/) framework is a mobile programming framework for constructing hybrid mobile apps. Hybrid mobile apps use a web technology (HTML, JS, CSS) to build cross-platform mobile apps (iOS, Android).  Ionic is built on top of the [Cordova](https://cordova.apache.org/) hybrid app technology.

The major advantage of using Ionic over Cordova itself or its commercial version [PhoneGap](http://phonegap.com/) is that it provides a plethora of mobile user interface templates both for the entire structure of the app (side menu, tabs) and for common parts (navigation top bar).

Ionic is built using the well known [AngularJS](https://angularjs.org/) web MVC framework.  Hence, developing Ionic apps is done at a higher level of abstraction than using plain HTML and jQuery. To facilitate working with the mobile device APIs in the AngularJS way, Ionic developed [ngCordova](http://ngcordova.com/), a set of AngularJS components for interacting with the various mobile device capabilities (like taking pictures).  Ionic also provides a set of convenient command line tools for building and running apps.

### The Perfect Recipe for Building Mobile Apps

In this scenario we’ll provide you with the recipe you need to use the Ionic template for a mobile app with a side menu.

#### Backand

Create an app in Backand using the free database feature. For this example we will demonstrate simple ‘playlists’ app
 with 3 objects: users, playlists and songs.
You can use the following JSON mode:


```json
[
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "name": {
        "type": "string"
      },
      "is_approved": {
        "type": "boolean"
      },
      "list": {
        "collection": "playlists",
        "via": "user"
      }
    }
  },
  {
    "name": "playlists",
    "fields": {
      "name": {
        "type": "string"
      },
      "description": {
        "type": "text"
      },
      "user": {
        "object": "users"
      },
      "songs": {
        "collection": "songs",
        "via": "playlist"
      }
    }
  },
  {
    "name": "songs",
    "fields": {
      "name": {
        "type": "string"
      },
      "order_date": {
        "type": "datetime"
      },
      "playlist": {
        "object": "playlists"
      }
    }
  }
]
```

#### Ionic

Now create your mobile app, naming it ‘PlaylistsApp’, with the Ionic command-line tools.

#### JS Libraries

Your Ionic app has a `www` folder which is a single page application (SPA) built with AngularJS.  You will need to include the Backand libraries in your application. Either download them or use bower to obtain them. Then, copy them into your `index.html`.

* [angular1-sdk](https://github.com/backand/angular1-sdk), install with bower using:

  ```bash
    bower install angular1-sdk
  ```

* Include in your `index.html`:

  ```html
     <script src="lib/angularbknd-sdk/dist/backand.min.js"></script>
     <script src="lib/angular-cookies/angular-cookies.js"></script>
  ```

#### Authorization

Backand uses authorization headers in HTTP requests. The token for authorization is obtained upon sign in to Backand and is then stored using cookies via `ngCookies`. The Backand SDK adds authorization headers to each HTTP request using an `$http` interceptor.

In your main JavaScript file, `app.js` requires `backand`:

```javascript
     var myApp = angular.module('starter', ['ionic',
       'starter.controllers', 'backand', 'starter.services']);
```

And in your app's `config` add:

```javascript
     myApp.config(function($stateProvider, $urlRouterProvider,
       $httpProvider)    {
        $httpProvider.interceptors.push(httpInterceptor);
     });
```

Defining the interceptor as:
```javascript
     function httpInterceptor($q, $log, $cookieStore) {
       return {
         request: function(config) {
           config.headers['Authorization'] =
             $cookieStore.get('backand_token');
           return config;
         }
       };
     }
```

#### Sign In

In your controllers module require `Backand` and `ngCookies`:


```javascript
    var myModule = angular.module('starter.controllers', ['backand', 'ngCookies']);
```

In your login controller require `Backand`:

```javascript
    myModule.controller('loginCtrl', function($scope, Backand) {});
```
To perform the login call, remember that `playlists` is our Backand database name:


```javascript
    var appName = 'playlists';
    Backand.signin(username, password, appName).then(
      function(data){}, function(error){});
```

#### CRUD

Now you will need to define a service to make the database operations.  In your services module require `backand`:


```javascript
    var myServices = angular.module('starter.services', ['backand'])
```

Define a `DatabaseService`, require `Backand`:

```javascript
    .service('DatabaseService', function($http, Backand){    

      var baseUrl = '/1/object/data/';

      return {

        // read all rows in the object
          readAll: function(objectName) {  
            return $http({
              method: 'GET',
              url: Backand.getApiUrl() + baseUrl + objectName
           }).then(
          function(response) {
              return response.data.data;
            });
          },

        // read one row with given id
          readOne: function(objectName, id) {
          return $http({
            method: 'GET',
            url: Backand.getApiUrl() + baseUrl + self.objectName
                + '/' + id
          }).then(
            function(response) {
              return response.data;
            });
          }
      };
    });
```

#### Using Backand Data

In your controller, require `DatabaseService`, and make calls like:

```javascript
    myModule.controller('playlistsCtrl',    
      function($scope, DatabaseService) {

      $scope.playlists = [];

      // read all playlists for a user
      DatabaseService.readAll('playlists').then(
        function(data){
        $scope.playlists = data;
      });

      // a click handler for selecting a playlist
      $scope.clickPlaylist = function(id){
                DatabaseService.readOne('playlists', id).
           then(function(data){});
      });

    });
```

Now you can add more Tables and build more views by repeating the above steps. You can also add server side code or send emails in the Backand cloud service under the Action tab.

To review in Github: [ionic demo](https://github.com/backand/simple-rest-ionic).

## Retrieving data from Facebook using the Graph API

Once you've integrated with Facebook, you can use Facebook's [Graph API](https://developers.facebook.com/docs/graph-api) to augment your user data with information directly from the user's Facebook account. To do so, you'll just need to grab the user's Facebook User ID, and use the Graph API to retrieve data, update or publish new content, or any of the other functionality Facebook has to offer.

### Obtaining the Facebook User ID
To store the user's Facebook user ID (FUID) in Backand, follow these steps:

1. Open Security & Auth >> Security actions menu and edit the **"beforeSocialSignup"** action
1. Change the **Where Condition** of this action to "true" (this is set at the bottom of the content pane)
1. Uncomment the provided code that saves the Facebook user id: `userInput.fuid = parameters.socialProfile.additionalValues.id;`
1. Save the Action

Next, you need to add the FUID field to the "users" object:

1. Open the model page, from the **Objects** menu
1. Edit the model to add a new field - **fuid**, type string - to the "users" object
1. Click **Validate & Update** to validate the model, then **Ok** in the confirmation dialog to save your changes.

From this point onward, any user that signs up to your app using Facebook as their social media provider will have their FUID automatically populated in the **users** object.

### Getting Facebook data into Backand code
```javascript--persistent
	var fuid = "176102422846507";
	var facebookAccessToken = "EAAWouLyM4lEBADjVlK9yslq9YNjx7S5UUx9oKUo6BWxsj9qc77ZCuKZAPvBQUIulpieNJIJ0Uit3K0UFR0oxjxl68DupTb0uoJFXPQFUdTOlneLEprG6b8WxuYN3AX6m05hKpFbBPKczCab1OUetevdvkZCO6rtPUQEUtc68gZDZD";

    var response = $http({
        method: "GET",
        url:"https://graph.facebook.com/v2.8/" + fuid + "/friends?fields=id,name,gender",
        params:{
            "access_token": facebookAccessToken
        },
        headers:{"Content-Type":"application/json"}
    });

    console.log(response);
```
Once you have the Facebook user ID, you can use the Graph API to access any available Facebook data.

* See the Facebook Graph API docs at [https://developers.facebook.com/docs/graph-api](https://developers.facebook.com/docs/graph-api)
* You'll also need a valid Facebook Access Token for your application. Use [Facebook's access token tool](https://developers.facebook.com/tools/accesstoken/) to obtain one.
* Facebook offers [additional tools](https://developers.facebook.com/tools-and-support/)

At this point, you're ready to work with the API. For example, you can use this sample code to get a list of a user's friends in Facebook:



### Getting a Facebook profile image
```html
<a href="#">http://graph.facebook.com/{fuid}/picture</a>
```
You can easily obtain a user's Facebook profile using a single URL reference. The template is available on the right - simply replace **{fuid}** with the user's Facebook User ID.


```html
<a href="http://graph.facebook.com/{fuid}/picture?type=large" target="_blank">http://graph.facebook
.com/{fuid}/picture?type=large</a>
```
You can pull in a larger version of the profile picture using the HTML to the right:

Finally, you can refer to [Facebook's Docs](https://developers.facebook.com/docs/graph-api/reference/user/picture/) on their Graph API, and use the information to create whatever type of integration with Facebook that you desire.


## Amazon AWS and S3 Integrations
Amazon Web Services, or AWS, offer a lot of useful functionality for web developers. From hosting, to distributed computing, to even simple data storage, Amazon has backed many of the web's largest websites since its inception. Below we'll look at Amazon S3, and how we can integrate the data storage service into a Backand-powered application.

### A Quick S3 Overview

Amazon's Simple Storage Service, or S3, is an online file storage web service offered as a part of the Amazon Web Services suite of functionality. It was first launched in the US in 2006, and uses standard web service interfaces for communication. Service consumers can simply authorize their app with S3, then perform a series of web service calls to manage their S3 storage bucket. By using this service, a developer can offload concerns about data availability, reliability, and storage space to Amazon's server farms, simply paying a usage-based fee each month for the data stored.

### Server-side Action

[Backand's Noterious app](https://github.com/backand/simple-noterious-app) has an excellent example of how simple an integration with S3 can be. The functionality revolves around the creation of server-side actions to wrap the web service requests made over HTTP. The actual integration looks like this:

```javascript
var data =
    {
        // enter your aws key
        "key" : "AKIAIJX26SY3COIWV4FQ",

        // enter your aws secret key
        "secret" : "ogun3pSOyw8FtP5i17s2STBPO9jQ8cs+lnpxwg82",

        // this should be sent in post body
        "filename" : parameters.filename,
        "filedata" : parameters.filedata,         

        "region" : "US Standard",
        "bucket" : "backand-free-upload"

    }
var response = $http({method:"PUT",url:CONSTS.apiUrl + "/1/file/s3" ,
               data: data, headers: {"Authorization":userProfile.token}});

return response;
```

The above code is inserted into a new Custom Server-Side JavaScript Action created using the Backand dashboard. It pulls the file information from the parameters passed to the action, then sends a request to S3's file upload endpoint, putting the file into the bucket `backand-free-upload`. By customizing the above code, you can achieve any underlying file structure you desire.

### Client-side Integration

Of course the server-side code is only half of the story – we need to integrate this wrapped API call into our application for it to be useful. Going back to the Noterious demo application, you can see an example of this taking place in the [Notes model class](https://github.com/backand/simple-noterious-app/blob/6da40ba8b235c35d77f1845a643e80a352d1dc20/src/app/common/models/notes-model.js#L44):

```javascript
//Send file content in base64 to Backand on-demand action
self.s3FileUpload = function(filename, filedata, success, error)
{
  return $http ({
    method: 'POST',
    url: Backand.getApiUrl() + '/1/objects/action/notes?name=upload2s3',
    headers: {
      'Content-Type': 'application/json'
    },
    data:
    {
      "filename":filename,
      "filedata": filedata.substr(filedata.indexOf(',')+1, filedata.length) //need to remove the file prefix type
    }
  });
};
```

The above code is pretty straightforward. It defines a function – `s3FileUpload` – that takes input from your application in the form of file data, and then communicates with your new custom JavaScript action via an HTTP POST request using JavaScript's built-in AJAX capabilities. Simply call this function from your Angular code, and you will be uploading files in no time!

## Stripe Integration
Once you've decided how you want to monetize your web app, you'll likely begin looking at ways to integrate your code with a vendor that will accept and process payments on your behalf. While this would be a daunting effort to perform in a custom-built environment, Backand's built-in security and stability help to ease many of the requirements that are imposed upon payment applications. Below we'll look at using a third-party vendor – [Stripe](http://www.stripe.com/) – to process payments, and examine how to integrate Stripe with a Backand application.

A quick note: In our [Stripe Example Application](https://github.com/backand/stripe-example), we used [angular-stripe](https://github.com/bendrucker/angular-stripe) – a popular third-party tool that allows you to easily interface with Stripe from your Angular code – to make calls to Stripe's Stripe.js file, which provides most of the payment tokenization functionality. This is obviously a convenience, and you are free to call Stripe.js directly from your application, but the below text assumes you are also using this library. It is also important to note that the code below – particularly the client side code – was adapted from the Stripe example application, and as such may not be directly usable without some modification. Please study the code to understand the content, rather than simply trying to cut-and-paste the client-side components.

### Why use a vendor?

The first question that you are likely asking is "Why should I use a third-party vendor for my payment system?" The short answer, albeit not a very good one, is that a solid and reliable payments platform is not only extremely challenging, but extremely expensive as well. Setting aside the data concerns (which we have covered elsewhere), you'll need significant liquid assets available to even begin working with financial systems directly. Furthermore, you'll be accepting full fraud risk for your users – if any of your users tries to use your payments system for illegal or unethical ends, then you will be liable for the damages caused. These are just a few of the many concerns that would come with building a payment system from the ground up without using a payment vendor.

With a payment vendor, on the other hand, you are able to offload all of the above issues to the vendor, allowing you to focus instead on integrating payments into your application. Stripe performs underwriting (which handles any potentially fraudulent individuals by simply preventing them from accepting payments), payment processing, communication with financial systems, and all necessary reporting as the transactions move through the various banks and financial APIs. While it would be possible to perform all of this work yourself, you'll save yourself years of labor by simply leveraging someone else's efforts.

### Server-side Action

We'll begin the application by adding a custom server-side action. This action will execute JavaScript that accepts a payment token as an argument, and then uses that token to register a payment with Stripe. Simply use the following code in your server-side action to log your payment:

```javascript
  // Secret key - copy from your Stripe account   
  // - https://dashboard.stripe.com/account/apikeys -
  // or use Backand's test account
  var user = btoa("sk_test_hx4i19p4CJVwJzdf7AajsbBr:");

  response = $http({
    method: "POST",
    url: "https://api.stripe.com/v1/charges",
    params: {
        "amount":parameters.amount,
        "currency":"usd",
        "source": parameters.token
    },
    headers: {
        "Authorization": "Basic " + user,
        "Accept" : "application/json",
        "Content-Type": "application/x-www-form-urlencoded"
    }
  });

return {"data":response};
```

The above code simply takes all of the information needed for a payment – specifically the amount (in pennies) and the payment method token – and sends them to Stripe via a POST request to their [RESTful API](https://stripe.com/docs/api). You can create as many of these actions as you like, wrapping additional Stripe API calls at will.

### Client-side Integration

Once you've got the server-side action, you are just about ready to start accepting payments. There are two steps that need to take place on the client side before you can do so. First, we'll look at setting the API Key.

The API Key tells the Stripe.js file that the actions being taken are to take place under your Stripe account. On the server side, this is the Secret Key that you obtain from the Stripe dashboard. The client side needs something more secure, though, as anyone with your secret key could impersonate your app and cause security issues. Recognizing this, Stripe provides you with a public key that you can use for client-side code. This is secured against a secret key on your Stripe account, and cannot be used to actually register payments or perform transfers. To set your public key, use the following code:

```javascript
  .config(function (stripeProvider) {
    //Enter your Stripe publish key or use Backand test account
    stripeProvider.setPublishableKey('pk_test_pRcGwomWSz2kP8GI0SxLH6ay');
  })
```

Once you've set your publishable key, you're ready to start tokenizing payment methods. The code below performs two tasks. It first creates a token from the payment data entered into your app's UI, then it sends that token to the custom action that we created in the previous section by calling a convenience method. We'll first add the tokenization and payment success code:

```javascript
  self.charge = function(){

    self.error = "";
    self.success = "";

    //get the form's data
    var form = angular.element(document.querySelector('#payment-form'))[0];

    //Use angular-stripe service to get the token
    return stripe.card.createToken(form)
      .then(function (token) {
        console.log('token created for card ending in ', token.card.last4);

        //Call Backand action to make the payment
        return BackandService.makePayment(form.amount.value, token.id )
      })
      .then(function (payment) {
        self.success = 'successfully submitted payment for $' + payment.data.data.amount/100.0;
      })
      .catch(function (err) {
        if (err.type && /^Stripe/.test(err.type)) {
          self.error = 'Stripe error: ' +  err.message;
        }
        else {
          self.error = 'Other error occurred, possibly with your API' + err.message;
        }
      });
  }
```

Once this code has been created, we need to write one additional function. This function wraps a HTTP request to the custom action we created previously. It assumes that the action's name is `makePayment`, so be sure to change it to something that matches your actual implementation:

```javascript
factory.makePayment = function(amount, token){
    return $http({
      method: 'GET',
      url: Backand.getApiUrl() + '/1/objects/action/items?name=makePayment',
      params:{
        parameters:{
          token: token,
          amount: amount
        }
      }
    });
  }
```

And with that, we've added the basic code necessary to contact Stripe and record a payment.

### Conclusion

The above code gives us the basis of a payments system. It can accept payments from a user, which are recorded against your own Stripe account. You can use this as a basis for a larger application, that provides a deeper integration with Stripe's API. Simply follow the pattern of wrapping the API calls in server-side actions, then calling those actions via the Backand API, and you'll have a solution built very rapidly that is both secure and scalable from day one.

## Mandrill
Mandrill is an email infrastructure service that focuses on transactional emails. Using Mandrill, you can send personalized e-commerce messages on a one-to-one basis, or automated transactional messages for things like password resets, order confirmations, and welcome messages.

### Send Email with Mandrill API (No Attachment)
Mandrill has an API that you can use to send emails, along with a lot of other functionality. By translating their provided cURL commands to Angular $http calls, you can easily integrate Mandrill with Backand.
> Server side action code

```javascript--persistent
/* globals
  $http - Service for AJAX calls
  CONSTS - CONSTS.apiUrl for Backands API URL
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here
    var response = $http({
      method: 'POST',
      url: 'https://mandrillapp.com/api/1.0/messages/send.json',
      data: {'key':<enter your mandrill key>,
            'message':{'html':parameters.message,
            'subject':'Example for Mandrill Integration',
            'from_email':userProfile.username,'from_name':parameters.name,
            'to':[{'email':userProfile.username,'name':parameters.name,'type':'to'}],
            'headers':{'Reply-To':'message.reply@backand.com'}}}
    });
    console.log(response);
	return {};
}
```

To send an email with Mandrill, you need to create a server side action. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more about how to create actions [here](#custom-actions). Name the action `mandrillapp`, add `message` and `name` to the Input Parameters, and paste the code on the right into the code editor.

In the example app we're building, the app's users can send messages to themselves. This is done through the use of `userProfile.username`, which is the email address used to register with the application. Make sure to replace the `key` property above with your Mandrill API key.

> Client-side code

```javascript--persistent
return $http ({
  method: 'GET',
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>/1',
  params: {
    name: 'mandrillapp',
    parameters: {
      message: 'Anything you can put your mind on',
      name: '<your name here>'
    }
  }
});
```
Next, add the JavaScript code to the right to your app's client-side code base. Replace `<your object name>` with the object associated with the action you created. Once this is done, you'll be able to easily trigger emails via Mandrill using Backand's custom action API.

### Send Email with Mandrill API (with Attachments)

Below, we’ll create a server-side Node JS action that imports the Mandrill SDK, allowing you to easily send email with attachments through Mandrill in your Backand application.

### Getting Started in Backand

To get started with a Mandrill action, you’ll need to first create a new Server-Side NodeJS action in Backand. To do so, open up your app’s dashboard at [https://www.backand.com](https://www.backand.com), and navigate to an object that will host the action. On the Actions tab of this object, create a new 'On Demand' action. Name this action, then select 'Server-Side Node.JS Action' as the action’s type. Follow the documentation provided to download the Backand CLI and initialize your action locally.

### Initializing the Mandrill Action

Once you have the Server-Side Node.js action ready to go, the next step is to create an action in Mandrill. Navigate to [http://www.mandrill.com/](http://www.mandrill.com/) and create a new action in the MailChimp dashboard provided when you log in. Once the action is ready, copy down the Mandrill API Key value and save it somewhere safe. This API key will be used by the Node.js action to connect and communicate with Mandrill.

### Updating the Code
```javascript--persistent
var mandrill = require('node-mandrill')(MANDRILL_API_KEY);
var request = require('request').defaults({ encoding: null });
```

Next, we’ll need to update the action’s code to communicate with Mandrill. We’ll start by  configuring the mandrill SDK requirements in `index.js`. Add these lines into `index.js` in your Node.js action folder structure, before the function `exports.backandCallback`:


<aside class="notice">Be sure to replace MANDRILL_API_KEY with the value copied down from the Mandrill dashboard.</aside>

```javascript--persistent
    var filePath = 'PATH_TO_ATTACHMENT_INCLUDING_FILENAME';
    var fileName = 'FILENAME_OF_ATTACHMENT';

    request.get(filePath, function (error, response, body) {
      if (!error && response.statusCode == 200) {
        sendEmail(fileName, response.headers['content-type'], new Buffer(body).toString('base64'));
      }
    });

    function sendEmail(fileName, fileType, fileContent) {
      mandrill('/messages/send', {
        message: {
          to: [{email: 'RECIPIENT_EMAIL', name: 'RECIPIENT_DISPLAY_NAME'}],
          from_email: 'SENDER',
          subject: 'EMAIL_SUBJECT',
          text: 'MESSAGE_BODY',
          attachments: [{
            'type': fileType,
            'name': fileName,
            'content': fileContent
          }]
        }
      }, function (error, res) {
        //uh oh, there was an error
        if (error) {
          //console.log(JSON.stringify(error));
          response(error, null);
        }
        //everything's good, lets see what mandrill said
        else {
          console.log(res);
          response(null, res);
        }


      });
    }
```

Next, we’ll modify the `backandCallback` function to send a message with Mandrill. Replace the contents of this function with the code on the right. This code does two things:

1. The initial code fetches the attachment data from the server on which it is stored. If this code succeeds, it calls `sendEmail` with the file data provided as a Base 64 string.
2. `sendEmail` then takes this data and contacts Mandrill to send the message.

### Configuring the Code

To tie the project together and finalize the above code, simply replace each of the placeholders with the appropriate value:

* `PATH_TO_ATTACHMENT_INCLUDING_FILENAME` - this is the URL to the attachment you wish to send. If fetching this fails, sending the message will not succeed.
* `FILENAME_OF_ATTACHMENT` - this is the file name that will be given to the attachment.
* `RECIPIENT_EMAIL` - This is the email for the message’s intended recipient.
* `RECIPIENT_DISPLAY_NAME` - This is the recipient’s display name.
* `SENDER` - This is the email from which the message was sent.
* `EMAIL_SUBJECT` - This is the subject of the email.
* `MESSAGE_BODY` - This is the content of the email.

You can also use the parameters argument to the action to send additional message data, whether that data originates in your app’s database or via the API call. Once these changes are made, your server-side action is ready to deploy!

### Testing and Deployment
You can debug and run the action locally using the provided `debug.js` file. Simply enter node `debug.js` on the command line to debug. Once you’ve finished your local testing, you can then deploy the action via the documentation provided in the Server-Side Action’s UI in your app’s dashboard at Backand.com - head to the Actions tab for the relevant object, and follow the instructions on using `backand action deploy` to deploy your code.

### Calling the Action

To call the action in your client-side code, simply use the Backand SDK’s action functionality as you would any other on-demand action:

```javascript
  return Backand.object.action.get('OBJECT_NAME', 'ACTION_NAME', {
    'parameters': {}
  })
```

Simply replace `OBJECT_NAME` with the name of the object controlling your action, and `ACTION_NAME` with the Server-Side Node.js Action’s name in Backand. You can provide any extra information or detail using the provided `parameters` object - this will be passed into the parameters argument of your action.

## Mailgun
Mailgun is an email automation service provided by Rackspace. It offers a complete cloud-based email service for sending, receiving, and tracking email messages sent by your registered websites and applications.

### Send Email with Mailgun API
Mailgun provides an API that can be used to easily send email, among many other features. By translating Mailgun's provided cURL examples to JavaScript $http calls, you can easily integrate Mailgun with Backand.

To send an email with Mailgun, you need to create a server side action.You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](#custom-actions). Name the action ,ao;gim, add message and name to the Input Parameters, and paste the following code in the code editor. When finished, the code editor window will contain the following:

```javascript
/* globals
  $http - service for AJAX calls - $http({method:"GET",url:CONSTS.apiUrl + "/1/objects/yourObject" , headers: {"Authorization":userProfile.token}});
  CONSTS - CONSTS.apiUrl for Backand's API URL
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here

	var apiBaseUrl = "https://api.mailgun.net/v3/sandbox<your mailgun sandbox key here>.mailgun.org/messages";

    var apiKey = "api:<your mailgun key here>";

    var encodedAuth = btoa(apiKey);
    //console.log(encodedAuth);

    	var response = $http(
	    {
	        method:"POST",
	        url: apiBaseUrl,
	        headers: {
	            "Authorization": "Basic " + encodedAuth,
	            "Content-Type" : "multipart/form-data; charset=utf-8",
	        },
	        data: {
	            from: userProfile.username,
	            to: userProfile.username,
	            subject: 'testing mailgun with backand',
	            text: parameters.message
	        }

	    }
	);

    console.log(response);
	return {};
}
```
in this example the app user can send any message that he wants to himself. Please replace the "apiKey" property, as well as the sandbox URL, with your associated mailgun values.

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  method: 'GET',
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>/1',
  params: {
    name: 'mailgun',
    parameters: {
      message: 'Anything you can put your mind on',
      name: '<your name here>'
    }
  }
});

```
Replace <your object name> with the object associated with the action you created, and you are ready to send messages via Mailgun!

## SendGrid
SendGrid is a cloud-based SMTP provider that allows you to send email without having to maintain email servers. SendGrid manages all of the technical details, from scaling the infrastructure to ISP outreach and reputation monitoring to whitelist services and real time analytics.

### Who is SendGrid for?
SendGrid is for anyone that needs to send email, whether it’s transactional email or marketing emails and campaigns. We send billions of emails each month, so we can handle your email regardless of volume.

### Send Email with SendGrid API
SendGrid has an API that you can use to send emails, along with a lot of other functionality. By translating their provided cURL commands to Angular $http calls, you can easily integrate SendGrid with Backand.

To send an email with SendGrid, you need to create a server side action. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action SendGrid, add 'to'' and 'message' to the Input Parameters (like that: to, message ), and paste the following code in the code editor. When finished, the code editor window will contain the following:

```javascript
/* globals
  $http - Service for AJAX calls
  CONSTS - CONSTS.apiUrl for Backands API URL
  Config - Global Configuration
  socket - Send realtime database communication
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {

    var res = $http({
        method:"POST",
        url:"https://api.sendgrid.com/api/mail.send.json",
        data:
            "from=" +userProfile.username+
            "&to=" + parameters.to +
            "&subject=hellow"+
            "&html=" + parameters.message,

         headers: {"Accept":"Accept:application/json",
                "Content-Type": "application/x-www-form-urlencoded",
                "Authorization":"Bearer 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"

         }
     });
     return res;
}
```
In the example app we're building, the app's users can send messages to an email sent by the client side in the 'to' parameter.  Make sure to replace the 'Authorization' header above with your SendGrid API key (after you register with SendGrid you should waite for your account to be provisioned and than you'll see the API KEY under Settings/ API Keys).

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  method: 'GET',
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
  params: {
    name: 'SendGrid',
    parameters: {
      message: 'Anything you can put your mind on',
      to: '<your destination email>'
    }
  }
});

```

Replace <your object name> with the object associated with the action you created and <your destination email> .

Once this is done, you'll be able to easily trigger emails via SendGrid using Backand's custom action API.

## PayPal
Once you've decided how you want to monetize your web app, you'll likely begin looking at ways to integrate your code with a vendor that will accept and process payments on your behalf. While this would be a daunting effort to perform in a custom-built environment, Backand's built-in security and stability help to ease many of the requirements that are imposed upon payment applications. Below we'll look at using a third-party vendor – [PayPal](http://www.paypal.com/) – to process payments, and examine how to integrate PayPal with a Backand application.

A quick note: The below text was adapted from the [PayPal developer docs](https://developer.paypal.com/docs/integration/direct/make-your-first-call/#make-an-api-call), and as such may not be directly usable without some modification. Please study the code to understand the content, rather than simply trying to cut-and-paste the client-side components.

PayPal provides many options for accepting payments, including Express Checkout, transactions, refunds, payment with agreementId, and so on. In this example, we will implement Express Chackout as it is the simplest and most straightforward method to enable.

### Why use a vendor?

The first question that you are likely asking is "Why should I use a third-party vendor for my payment system?" The short answer, albeit not a very good one, is that a solid and reliable payments platform is not only extremely challenging, but extremely expensive as well. Setting aside the data concerns (which we have covered elsewhere), you'll need significant liquid assets available to even begin working with financial systems directly. Furthermore, you'll be accepting full fraud risk for your users – if any of your users tries to use your payments system for illegal or unethical ends, then you will be liable for the damages caused. These are just a few of the many concerns that would come with building a payment system from the ground up without using a payment vendor.

With a payment vendor, on the other hand, you are able to offload all of the above issues to the vendor, allowing you to focus instead on integrating payments into your application. PayPal performs underwriting (which handles any potentially fraudulent individuals by simply preventing them from accepting payments), payment processing, communication with financial systems, and all necessary reporting as the transactions move through the various banks and financial APIs. While it would be possible to perform all of this work yourself, you'll save yourself years of labor by simply leveraging someone else's efforts.

### Server-side Action

We'll begin the application by adding a custom server-side action. This action will execute JavaScript that implements a 2 stage process. It performs the payment API call, and then the Approval API call. Each step uses an access token obtained from PayPal's oauth2/token call. To save a call to the oauth endpoint for an extra token, we'll cache the oauth token in a server side cookie (although if the cookie expires, you'll need to refresh the token from the oauth2/token endpoint. See the try-catch block below). The first stage prepares the payment and returns a url for redirecting the user to PayPal site. The second stage occurs after the user has acknowledged the payment and been brought back to your application, at which point you call the Approval API to complete the payment. THe code example below is a server-side action that implements all of the functionality necessary to register an Express Checkout payment with PayPal.

Please note: Don't forget to change the PayPal credentials in the code below. These credentials represent your association with PayPal, and must be correct in order for your customer's funds to be correctly routed. To obtain these credentials, you can [create a PayPal sandbox account](https://developer.paypal.com/developer/accounts/). Note that you will first need to register for PayPal if you have not already done so.

```javascript

   var paypalUrl = 'https://api.sandbox.paypal.com/';

   // PayPal has a 2 stage process where: // the first stage prepares the payment and a returns a url for the user to pay
   // and the sconde stage (where the payment is actually done) is where the application send an approval API call to PayPal
   if (parameters.process == 'payment') {
     var payment = postPayment();
     return payment.links[1].href;
   }
   else if (parameters.process == 'approval') {
     return postApproval();
   }

   function postPayment() {
     var authorization = "Bearer " + getAccessToken().access_token;
     var payment = {
       "intent": "sale",
       "redirect_urls": {
         "return_url": "http://localhost:3000/#/paypal",
         "cancel_url": "http://localhost:3000/#/paypal?fail=true"
       },
       "payer": {"payment_method": "paypal"},
       "transactions": [
         {
           "amount": {
             "total": parameters.amount,
             "currency": "USD"
           }
         }
       ]
     };
     try {
       return $http({
         method: 'POST',
         url: paypalUrl + 'v1/payments/payment',
         data: payment,
         headers: {
           "Content-Type": "application/json",
           "Accept-Language": "en_US",
           "Authorization": authorization

         }
       });
     }
     catch (err) {
       if (err.name == 401) {
         cookie.remove('paypal_token');
         return postPayment();
       }
       else {
         throw err;
       }
     }
   }

   function postApproval() {
     var authorization = "Bearer " + getAccessToken().access_token;
     var payer = {"payer_id": parameters.payerId};
     try {
       return $http({
         method: 'POST',
         url: paypalUrl + 'v1/payments/payment/' + parameters.paymentId + '/execute/',
         data: JSON.stringify(payer),
         headers: {"Content-Type": "application/json", "Accept-Language": "en_US", "Authorization": authorization}
       });
     }
     catch (err) {
       if (err.name == 401) {
         cookie.remove('paypal_token');
         return postApproval();
       }
       else {
         throw err;
       }
     }
   }

   function getAccessToken() {
     var token = cookie.get('paypal_token');
     if (!token) {
       var ClientId = 'YOUR_PayPal_CLIENT_ID';
       var Secret = 'YOUR_PayPal_SECRET_KEY';
       var user = btoa(ClientId + ":" + Secret);

       try {
         token = $http(
           {
             method: 'POST',
             url: paypalUrl + 'v1/oauth2/token',
             data: 'grant_type=client_credentials',
             headers: {
               "Accept-Language": "en_US",
               "Authorization": "Basic " + user
             }

           });
       }
       catch (err) {
         if (err.name == 401) {
           var e = new Error("Unauthorized (401), check client id and secret");
           e.name = err.name;
           throw e;
         }
         else {
           throw err;
         }
       }
       cookie.put('paypal_token', token);
     }
     return token;
   }

```

The above code takes the payment stage information in parameters.process, and executes the approprite PayPal API call. In the first stage, when directing the user to PayPal to confirm the payment, parameters.process will contain "Payment". This results in the function postPayment being called, which sends a POST command to PayPal with the payment information, along with a URL to redirect to when the user either accepts or cancels the payment. In the second payment stage, when contacting the Approval API to complete the payment, parameters.process will contain "Approval". This will result in the code calling postApproval, which issues a POST call to PayPal using the PayerID and PaymentID stored in the query string of the return URL. This function will return a PayPal payment object including all of the relevant payment details and the payment status.

### Client-side Integration

Once you've got the server-side action, you are just about ready to start accepting payments. The code below performs two tasks. It first calls a service to prepare the payment (the Payment API in PayPal), which returns a URL to which the user is redirected.

```javascript
      var self = this;

        self.charge = function () {
          self.error = "";
          self.success = "";

          //get the form's data
          var form = angular.element(document.querySelector('#paypal-form'))[0];

          //Call Backand action to prepare the payment
          var paypalUrl = BackandService.makePayPalPayment(form.amount.value)
            .then(function (payment) {
              var paypalResponse = payment;
              //redirect to PayPal - for user approval of the payment
              $window.location.href = paypalResponse.data;
            })
            .catch(function (err) {
              if (err.type) {
                self.error = 'PayPal error: ' + err.message;
              } else {
                self.error = 'Other error occurred, possibly with your API' + err.message;
              }
            });
        }
  ```

  Once the user has accepted or cenceled the payment, PayPal redirects them back to the URL that was provided to the server-side action. In this example, we use the same page by checking the query string parameter paymentid. This allows the code to detect which stage of the payment process we are at.

  ```javascript
        //check if this is a redirect from PayPal , after the user approves the payment
        // PayPal adds PayerID and  paymentId to the return url we give them

        if ($location.search().PayerID && $location.search().paymentId) {

          //Call Backand action to approve the payment by the facilitator
          BackandService.makePayPalApproval($location.search().PayerID, $location.search().paymentId)
            .then(function (payment) {
              // remove PayPal query string from url
              $location.url($location.path());
              self.success = 'successfully submitted payment for $' + payment.data.transactions[0].amount.total;
            }
          )
      }
```

Once this code has been added to your project, we need to implement the service that calls Backand's server side payment action. This service wraps a HTTP request to the on-demand custom action that we created earlier in the tutorial. It assumes the action name is 'PayPalPayment', so be sure to change it as appropriate to match your actual implementation:

```javascript
/Call PayPalPayment on demand action
    factory.makePayPalPayment = function (amount) {

      return $http({
        method: 'GET',
        url: Backand.getApiUrl() + '/1/objects/action/items/1',
        params: {
          name: 'PayPalPayment',
          parameters: {
            amount: amount,
            process:"payment"
          }
        }
      });
    };

    //Call PayPalApproval on demand action
    factory.makePayPalApproval = function (payerId, paymentId) {
        return $http({
        method: 'GET',
        url: Backand.getApiUrl() + '/1/objects/action/items/1',
        params: {
          name: 'PayPalPayment',
          parameters: {
            payerId: payerId,
            paymentId: paymentId,
            process:"approval"
          }
        }
      });
    };
```

And with that, we've added the basic code necessary to contact PayPal and record a payment.

### Conclusion

The above code gives us the basis of a payments system. It can accept payments from a user, which are recorded against your own PayPal account. You can use this as a starting point for a larger application that provides a deeper integration with PayPal's API. Simply follow the pattern of wrapping the API calls in server-side actions, then calling those actions via the Backand API, and you'll have a solution built very rapidly that is both secure and scalable from day one.

## Segment.io
There are a large number of web analytics tools available that can help your marketing team understand both how many users are using your product, and what they are doing while they use it. Implementing these services in your Backand application is as simple as any other third-party API integration. Segment.io allows you to implement an analytics API and send it to almost any notification tool available, depending on your infrastructure needs. Normally this type of integration would be done on the client side, but there are some instances where a server-side integration is useful. In this example, we will look at implementing a Segment.io integration with your Backand application using a custom server-side action.

### Server-side Action

We'll start by adding a new custom server-side action.  This action will execute JavaScript code that sends user identification data to segment.io (and, from there, to Woopra, or Intercom, or any other interested service):

```javascript
* globals
 $http - Service for AJAX calls
 CONSTS - CONSTS.apiUrl for Backands API URL
 */
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
  // write your code here

  var writeKey = 'YOUR_SEGMENT_IO_WRITE_KEY';
  var authorization = 'Basic ' + btoa(writeKey);
  var segmentUrl = 'https://api.segment.io/v1/identify';

  var segmontPostResponse = $http(
    {
      method: 'POST',
      url: 'https://api.segment.io/v1/identify',
      data: {
        "userId": parameters.userId,
        "traits": {
          "email": parameters.userId,
          "ActiveApps": parameters.activeApp
        },


        "integrations": {
          "All": false,
          "Woopra": true,
          "Intercome":true
        }
      },
      headers: {'Authorization': authorization, 'Accept': 'application/json', 'Content-Type': 'application/json'}

    });

  return segmontPostResponse;
}
```

The above code takes two parameters - userId and activeApp, with the userId being the user's email address. You can also send Segment.io data collected from Backand using a query to obtain the data you have stored on a given user.

### Client-side Integration

This code has no client-side component.

## Twilio
[Twilio](https://www.twilio.com) takes care of dealing with messy telecom hardware, exposing a globally available cloud API that developers can interact with to build intelligent and complex communications systems. As your app's usage scales up or down, Twilio automatically scales with you.

### Who is Twilio for?

Twilio is for anyone that needs to send SMS, MMS, or VoIP, along with a lot of other communication channels embedded into web, desktop, and mobile software.

### Send SMS with Twilio API
Twilio has an API that you can use to send SMS. By translating their provided cURL commands to Angular $http calls, you can easily integrate Twilio with Backand.

To send SMS with Twilio, you need to create a server side action. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action TwilioSendSMS, add to and message to the Input Parameters, and paste the following code in the code editor. When finished, the code editor window will contain the following:

```javascript
/* globals
  $http - Service for AJAX calls
  CONSTS - CONSTS.apiUrl for Backands API URL
  Config - Global Configuration
  socket - Send realtime database communication
  files - file handler, performs upload and delete of files
  request - the current http request
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here
	var ACCOUNT_SID = 'AC2933cadba659d1f15bd409333e3bc38b';
    var AUTH_TOKEN = 'e924350f6eefb5dd1ae011b49d5cb5dd';

    var FROM_PHONE_NUM = 'GetEat';

    var basicUrl = 'https://api.twilio.com/2010-04-01/Accounts/' + ACCOUNT_SID + '/';
    var action = 'Messages.json' // twilio have many services

    return $http({
        method: "POST",
        url: basicUrl + action,
        data:
            'Body=' + parameters.message +
            '&To='  + parameters.to +
            '&From='+ FROM_PHONE_NUM,
        headers: {
            "Accept":"Accept:application/json",
            "Content-Type": "application/x-www-form-urlencoded",
            "Authorization": 'basic ' + btoa(ACCOUNT_SID + ':' + AUTH_TOKEN)
        }
    });

}
```
In the example app we're building, the app's users can send a SMS message to a phone number. The phone number is sent, from the client side, in the 'to' parameter, while the message content is sent in the 'message' parameter.

### Setup a FREE account in Twilio

After you register with Twilio, you can get your Twilio phone number [here]( https://www.twilio.com/user/account/phone-numbers/getting-started):
1. To choose a different phone number from the one provided, click on *'Don't like this one? Search for a different number.'* and select SMS in capabilities.
2. Replace the FROM_PHONE_NUM in the code with the Twilio phone number obtained in the prior step (dont forget the (+) sign before the number)
3. Make sure you replace the 'ACCOUNT_SID' and 'AUTH_TOKEN' in the code with your Twilio API keys (from the getting started page). Simply click on 'Show API Credentials' on the right side of  'Get Started with Phone Numbers,' and than you'll see the ACCOUNT SID and AUTH TOKEN values.

### Setup client-side code:

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: 'TwilioSendSMS',
      parameters: {
        message: 'Anything you can put your mind on',
        to: '<your destination phone number>'
      }
    }
});

```

Replace 'your object name' with the object associated with the action you created and 'your destination phone number' with a vaild phone number (when using Twilio trial account you first need to validate this phone number)

Once this is done, you'll be able to easily trigger SMS via Twilio using Backand's custom action API.

## Netmera
Netmera is a cloud based service that can be used to send Push Notifications to various platforms, among other services such as Exception reporting. It offers a friendly site where campaigns ()push notifications) can be managed and customized and a REST API to send push notifications automatically. In this guide you can find out how to get started on Netmera and send push notifications with Backand.
### Get Started with Netmera
1. Register to Netmera
2. Download the Netmera SDK from [here](https://netmera.readme.io/docs/android-sdk-download) or their [starter app](https://cp.netmera.com/nm/admin/sdkDownload/overview/android/final?isNewProject=true).
3. Check out their guide to [set up your app/the starter app with the Netmera SDK](https://netmera.readme.io/docs/android-guide)
4. Follow this [Android guide](https://netmera.readme.io/docs/netmera-push-notification-android) and [iOS guid](https://netmera.readme.io/docs/netmera-push-notification-ios) to configure push notifications on the app and get the key from Google Cloud Messaging. If you use the starter app you only need the first 3 steps to get started (unless you want to trigger advanced features like tags).
5. For Ionic you would need to implement the SDK for [PhoneGap Plugin](https://netmera.readme.io/docs/phonegap)

### Integrating Netmera with Backand
Netmera has a rest API that can be used to remotely send push notifications. You can integrate Netmera with Backand by using Backand server-side actions. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code.

In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action by clicking on 'Netmera' under 'Push Notifications'.

**You have just created your first push notification action!**

In order to call the function from your angular app, use the following code:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: 'SendPushNotification',
      parameters: {
        notificationTitle: 'My First Push Notification',
	    notificationContent: 'Hello! This is my first notification!'
      }
    }
});
```
Replace ‘your object name’ with the object associated with the action you created, set a title for the 'notificationTitle' field, write any message you want in ‘notificationContent’ field and you’re good to go. Now you can start sending push notifications dynamically using Backand and Netmera.

## PushWoosh
Pushwoosh is a cloud based service that can be used to send Push Notifications to various platforms. It offers a friendly site where push notifications can be sent and customized and an API to send push notifications automatically. In this guide you can find out how to get started on PushWoosh with an example app for Android and send push notifications with Backand.
### Get Started with PushWoosh

1. Register an account in PushWoosh and proceed to create an application. Using the PushWoosh control panel, configure your application to support Android including [configuring GCM (Google Cloud Messaging](http://docs.pushwoosh.com/docs/gcm-configuration))
2. You can either get the starter app or to integrate in an existing app.
To get a starter app: clone the PushWoosh Android SDK:
 ```
  git clone https://github.com/Pushwoosh/pushwoosh-android-sdk.git
 ```
 open /Samples/Android-Simple directory in Android Studio and build the app.

3. To integrate in an existing app use the following [Android guide](http://docs.pushwoosh.com/docs/native-android-sdk) – include the SDK.jar and add the relevant code to your application.
  [Make the relevant changes to your AndroidManifest.xml file](http://docs.pushwoosh.com/docs/androidmanifestxml-modifications) – if you're using the starter app just change the App ID and Project ID.
4. To integrate in an existing app use the following [iOS guide](http://docs.pushwoosh.com/docs/apns-configuration)
5. For Ionic you would need to implement the SDK for [Cordova / PhoneGap](http://docs.pushwoosh.com/docs/cordova-phonegap) and check this example for [PhoneGap Build](http://docs.pushwoosh.com/docs/phonegap-build)

### Integrating PushWoosh with Backand
PushWoosh has an API that can be used to send Push Notifications. You can integrate PushWoosh with Backand by using Backand server-side actions. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code.

In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action by clicking on 'PushWoosh' under 'Push Notifications'.

**You have just created your first push notification action!**

In order to call the function from your angular app, use the following code:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: 'SendPushNotification',
      parameters: {
        notificationContent: 'Hello! This is a push notification!',
      }
    }
});
```
Replace ‘your object name’ with the object associated with the action you created and write any message you want in ‘notificationContent’ field and you’re good to go. Now you can start sending push notifications dynamically using Backand.

## Facebook Messenger Bot
<img align="right" src="https://www.backand.com/wp-content/uploads/2016/09/bot-iphone.png">


Facebook recently opened up their Messenger platform to enable bots to converse with users through Facebook Apps and on Facebook Pages. In support of this, the Facebook Messenger team created a comprehensive set of [documentation](https://developers.facebook.com/docs/messenger-platform/quickstart) describing the functionality on offer. While they offer a lot of functionality and platform-level integration for free, you still need to create a publicly-accessible application that Facebook can communicate with in order to automatically interact with your end users.

In this tutorial, we'll walk through creating your own messenger bot on the Backand backend-as-a-service system, and make it live - all in 10 minutes.

### Demo
You can chat with the simple bot example to see the end result at <a hreh="http://m.me/1150283998341709" target="\_blank">http://m.me/1150283998341709</a>

### Getting Started

Messenger bots are programs that receive messages from an interface, process those messages, and then send results back to the caller. These results are then displayed to the user by the controlling application - Facebook Messenger in this case. A bot's response can be as simple as echoing text back to the user, or as complex as ordering and shipping new computer components to the end user. The only restrictions are that the bot is authenticated to communicate with Facebook, and that Facebook has approved the bot and allows it to communicate with the world at large.

Luckily, Backand handles the majority of the server and connectivity functionality for you, allowing you to focus on building out responsive content.

### *Building the server*

Follow these steps to build out the back-end to your bot:

1. Sign up for free at [backand.com](https://www.backand.com) (if you don't already have an account).

2. Create a new app in the Backand dashboard, then navigate to that app's management page.

3. In the new app, open menu 'Objects --> Items', and click on the 'Actions' tab. In the Actions tab, click on the 'Facebook Messenger Bot' template.

4. Click 'Save'

At this point you have a back-end server that is ready to integrate with Facebook Messenger. However, you need to make a few more configuration changes before you're ready to test.


#### *Create a Facebook App and Page*

Once you have the back-end and are ready to integrate, the first thing you need to do is create a Facebook Page at [https://www.facebook.com/pages/create/?ref_type=pages_browser](https://www.facebook.com/pages/create/?ref_type=pages_browser). This page will serve as the central integration point between your Backand application and Facebook, as the messenger bot is authorized to interact with Facebook on behalf of this page.

##### Facebook App Creation

Once you have the app's page up and running, you need to configure the Facebook integration. To do so:

* Add a new app at [https://developers.facebook.com/quickstarts/?platform=web](https://developers.facebook.com/quickstarts/?platform=web). Name it and click "Create New Facebook App ID":

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-create-new-app.png)

* Add an email address, select your app's category, and add a web site for your app (any URL is OK):

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-create-new-app-info.png)

* Next, skip the quick start and go to the App Dashboard and click 'Add Product' under the heading 'Product Settings'. Once there, select 'Messenger' from the available options:

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-add-new-product-1.png)

##### *Setup Webhooks*

Once the app page is created, and the app is registered, you need to tell Facebook where to send its messages for processing. You can do this with the following steps:

* In the 'Webhooks' section, click 'Setup Webhooks'.

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-set-webhook1.png)

* Enter the URL for the webhook - this should link back to your Backand application, and will resemble the below:

```bash
https://api.backand.com/1/objects/action/items?
name=FBMessengerBot&Authorization=basic+<master token>:<user key>
```

_Note:_ The webhook URL uses Backand's [basic authentication](http://docs.backand.com/en/latest/apidocs/security/index.html#basic-authentication).
You can find the '&lt;master token&gt;' in the 'Security & Auth --> Social & Keys' section.
It also requires a '&lt;user key&gt;' for your app - you can find this in the 'Security & Auth --> Team' section. Simply click on the key icon near one of the Admins to obtain the user key.

* Verify Token: my_test_token

* Select *message_deliveries*, *messages*, *messaging_optins*, and *messaging_postbacks* under Subscription Fields.

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-setup-webhook.png)

##### *Get a Page Access Token*
Once you've configured the app in Facebook, it's time to tie it back into your Backand application. In the Token Generation section, select your Page. A 'Page Access Token' will be generated for you. Copy this 'Page Access Token', and navigate back to your Backand app. Open the 'Action' section of your 'Items' object, and paste the token into the Backand Action where indicated (PAGE_ACCESS_TOKEN).

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-select-page.png)

##### *Subscribe the App to the Page*
Finally, you need to subscribe to the webhooks available for your page. This is managed in the 'Webhooks' section of the Facebook configuration:

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-page-subscribe.png)

#### *Run the Bot*

At this point, you're ready to test your bot! Navigate to your Facebook Page and send it a message using Facebook Messenger. You should see the same message echoed back to you with a prefix of "Back& bot says:". This is also sent to the javascript console for immediate analysis.

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-facebook-page.png)

**At this point your basic bot is working. From here, you can customize the bot's server side to build out functionality for your users**

### How to share your bot

A bot is only useful if people choose to interact with it. Below are some methods you can use to advertise your bot and draw in users.

#### *Add a chat button to your webpage*

As a part of their initial documentation, Facebook provided an easy method to add a Chat button to your website. The documentation on how to do this is available [here](https://developers.facebook.com/docs/messenger-platform/plugin-reference).

##### *Create a shortlink*

You can also implement a shortlink that can be used to initiate a chat with your app. Simply use a URL of the form 'https://m.me/&lt;PAGE_USERNAME&gt;' to begin a Messenger chat.

#### What's next?

At this point, you have a chatbot interacting with Facebook Messenger, driven by Backand as a back-end. You can customize the bot's code to meet your needs by following the information available in the following section of *Customize the Bot's Code in a Backand Action*.

You can also review Facebook's [complete guide on Messenger integration](https://developers.facebook.com/docs/messenger-platform/product-overview/setup), which provides more comprehensive detail on the platform's capabilities. These can be used to enhance your message responses, adding web-enabled content that your users can follow.

Eventually, you'll want to get your bot approved for use by the public. You can find out how to do that [here](https://developers.facebook.com/docs/messenger-platform/app-review).

Finally, you can enhance your app's intelligence with an AI integration. Find out how to get started at [Wit.ai](https://wit.ai)!

### Customizing the Bot's Code in a Backand Action

#### *Receive Messages*

All callbacks and webhooks from Facebook will end up in your object's Facebook Action code. The JavaScript for this action is solely responsible for listening to the incoming POST calls, and responding appropriately. The template code handles all webhooks from Facebook by default. For example, receiving messages is handled by looking for the 'messagingEvent.message' field in the webhook, and then calling the 'receivedMessage()' function as follows:

```javascript
if (request.method == "POST"){

    var data = request.body;
    if (data.object == 'page') {

        // Iterate over each entry
        // There may be multiple if batched
        data.entry.forEach(function(pageEntry) {

            var pageID = pageEntry.id;
            var timeOfEvent = pageEntry.time;

            // Iterate over each messaging event
            pageEntry.messaging.forEach(function(messagingEvent) {
                if (messagingEvent.optin) {
                    //receivedAuthentication(messagingEvent);
                } else if (messagingEvent.message) {
                    receivedMessage(messagingEvent);
                } else if (messagingEvent.delivery) {
                    //receivedDeliveryConfirmation(messagingEvent);
                } else if (messagingEvent.postback) {
                    receivedPostback(messagingEvent);
                } else {
                    console.log("Webhook received unknown messagingEvent: ", messagingEvent);
                }
            });
        });
    }
}
```

#### *Send a Text Message*

In receivedMessage, we've added logic that can send a message back to the user. The default behavior is to echo back the text that was received, with some static modifications ('Back& bot says'...):

```javascript
function receivedMessage(event) {

    var senderID = event.sender.id;
    var recipientID = event.recipient.id;
    var timeOfMessage = event.timestamp;
    var message = event.message;

    console.log("Received message for user" + senderID + " and page " + recipientID + " at " + timeOfMessage + " with message");
    console.log(JSON.stringify(message));

    var messageId = message.mid;

    // You may get a text or attachment but not both
    var messageText = message.text;
    var messageAttachments = message.attachments;

    if (messageText) {

    // If we receive a text message, check to see if it matches any special
    // keywords and send back the corresponding example. Otherwise, just echo
    // the text we received.
    switch (messageText) {
          case 'image':
            //sendImageMessage(senderID);
            break;

          case 'button':
            //sendButtonMessage(senderID);
            break;

          case 'backand':
          case 'Backand':
            sendGenericMessage(senderID);
            break;

          case 'receipt':
            //sendReceiptMessage(senderID);
            break;

          default:
            sendTextMessage(senderID, messageText);
        }
    } else if (messageAttachments) {
        sendTextMessage(senderID, "Message with attachment received");
    }
};
```

*sendTextMessage* formats the message to be sent to Facebook, then calls the appropriate API endpoint:

```javascript
function sendTextMessage(recipientId, messageText) {
    var messageData = {
        recipient: {
          id: recipientId
        },
        message: {
          text: "Back& bot says: " + messageText
        }
    };

    callSendAPI(messageData);
};
```

*callSendAPI* calls the Send API to send the message back to the user:

```javascript
function callSendAPI(messageData) {
    try{

        var response = $http({
            method: "POST",
            url:"https://graph.facebook.com/v2.6/me/messages",
            params:{
                "access_token": PAGE_ACCESS_TOKEN
            },
            data: messageData,
            headers:{"Content-Type":"application/json"}
        });

        var recipientId = response.recipient_id;
        var messageId = response.message_id;

        console.log("Successfully sent generic message with id " + messageId + " to recipient " + recipientId);
    }
    catch(err){
        console.error("Unable to send message.");
        console.error(err);
    }

}
```

#### *Send a Structured Message*

*receivedMessage* can also send back other kinds of messages if it sees certain keywords. For example, if you send the message 'backand', it will call `sendGenericMessage()` - a function that sends back a Structured Message with a generic template.

```javascript
function sendGenericMessage(recipientId) {
    var messageData = {
        recipient: {
            id: recipientId
        },
        message: {
            attachment: {
                type: "template",
                payload: {
                    template_type: "generic",
                    elements: [{
                        title: "Messanger BAAS",
                        subtitle: "Backand as a service for Facebook Messanger",
                        item_url: "https://www.backand.com/features/",
                        image_url: "https://www.backand.com/wp-content/uploads/2016/01/endless.gif",
                        buttons: [{
                            type: "web_url",
                            url: "https://www.backand.com/features/",
                            title: "Open Web URL"
                        }, {
                            type: "postback",
                            title: "Call Postback",
                            payload: "Payload for first bubble",
                        }],
                    }, {
                        title: "3rd Party Integrations",
                        subtitle: "Connect your Bot to 3rd party services and applications",
                        item_url: "https://www.backand.com/integrations/",
                        image_url: "https://www.backand.com/wp-content/uploads/2016/01/3.png",
                        buttons: [{
                            type: "web_url",
                            url: "https://www.backand.com/integrations/",
                            title: "Open Web URL"
                        }, {
                            type: "postback",
                            title: "Call Postback",
                            payload: "Payload for second bubble",
                        }]
                    }]
                }
            }
        }
    };
    callSendAPI(messageData);
};
```

#### *Handle Postbacks*
Structured messages use 'postbacks' to communicate with your application when a user clicks on one of the provided enhanced objects. The postback message contains the payload that was created for the button in the original Structured Message. Buttons on Structured Messages support both opening URLs and communicating via postbacks.

In our webhook handler, we handle the postback by calling the function 'receivedPostback()':

```javascript
....
else if (messagingEvent.postback) {
    receivedPostback(messagingEvent);
}
...
```

This function sends a message back saying that the postback was called:

```javascript
function receivedPostback(event) {
    var senderID = event.sender.id;
    var recipientID = event.recipient.id;
    var timeOfPostback = event.timestamp;

    // The 'payload' param is a developer-defined field which is set in a postback
    // button for Structured Messages.
    var payload = event.postback.payload;

    console.log("Received postback for user " + senderID + " and page " + recipientID + "with payload '" + payload + "' " +
    "at " + timeOfPostback);

    // When a postback is called, we'll send a message back to the sender to
    // let them know it was successful
    sendTextMessage(senderID, "Postback called");
};
```

From here, you can expand the bot to provide a wealth of functionality to your page's users. Simply expand upon the above code until it meets your requirements.

## SalesforceIQ

Building and maintaining customer relationships is crucial for driving sales to your platform. However, it can also be a complex process requiring integrating data from multiple sources and, more importantly, ensuring that data is accessible when it is needed. Customer Relationship Management (CRM) software is designed to make this data management and integration process much easier, providing you with the tools you need to drive customers through your sales funnel. In this article, we'll look at integrating a Backand application with Salesforce IQ, providing you with all of the tools you need to effectively leverage your customer data in your Backand application.

### What is SalesforceIQ?
[SalesforceIQ](https://www.salesforceiq.com/) is an out-of-the-box CRM solution that quickly gives you access to dynamic information tied into a full CRM solution. With Automatic Data Capture and enterprise-level intelligence under the hood, SalesforceIQ acts like your own personal assistant so you can focus on what matters most: selling. SalesforceIQ, in addition to providing easy integrations with tools like Google and Microsoft Exchange, also gives you the capability to dynamically access and manage your data through a series of robust APIs.

### Connecting a Backand application with SalesforceIQ
Integrating Backand and SalesforceIQ is as simple as leveraging the full-featured [API](https://api.salesforceiq.com/) provided by Salesforce from within your Backand application. While traditionally you'd need to make these calls to the API from a server, you can achieve the same functionality in Backand by using our custom template action. This action provides an easy-to-use code template for making calls to the SalesforceIQ API, letting you update your user tracking data based upon user activity in your Backand app, or even update your Backand app's UI based upon what you know about your user in SalesforceIQ.

Working with the [SalesforceIQ API](https://api.salesforceiq.com/) provides you with full capabilities to create, retrieve, and update data on your users, as well as manage their information in SalesforceIQ. Communicating with their API is as simple as translating the cURL commands provided by Salesforce in their documentation into the appropriate $http calls that you can make from JavaScript. Simply provide the required authentication and identification headers, construct the URL, make the call, and handle the response when it arrives, dispatching it either directly to your application via a synchronous function call return, or emitting the data as an event using our Socket-based real-time communications functionality.

### The SalesforceIQ Action Template
```javascript--persistent
/* globals
  $http - Service for AJAX calls
  CONSTS - CONSTS.apiUrl for Backands API URL
  Config - Global Configuration
  socket - Send realtime database communication
  files - file handler, performs upload and delete of files
  request - the current http request
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {

    var API_KEY = '-- YOUR API --';
    var API_SECRET = '-- YOUR SECRET --';

    //get accounts
    var response = $http({
        method: "GET",
        url: "https://api.salesforceiq.com/v2/accounts",
        headers: {
            "Accept":"application/json",
            "Authorization": 'basic ' + btoa(API_KEY + ':' + API_SECRET)
        }
    });

    return response;
}
```

We have created an action template that will give you jump start with salesforceIQ. You can either trigger this action from an object's database transaction event actions, or create a new on-demand action that you can call from your app's client code. The sample JavaScript is provided by the template action, which is available as "SalesforceIQ" in the "CRM & ERP" section of action templates.:

This code provides you with all of the basic tools you need to get connected to the SalesforceIQ API. It takes in your SalesforceIQ API Key and API Secret, and performs a call to the "/accounts" endpoint to fetch accounts.

### Connecting this action to your SalesforceIQ account
To connect to SalesforceIQ, you'll first need to register for an account if you haven't done so. Once you've signed up, follow these steps to obtain your API Key and API Secret:

1. Open Settings under the gear icon
2. Open the 'Integration' tab under 'My Account Settings'
3. Under 'Create New Custom Integration,' click 'Custom'
4. Set the name to 'Backand API,' and provide a description
5. Copy the 'API Key' and 'API Secret' from the integration page into the JavaScript Action code
6. Click 'Save'

When this is completed, you should now be able to access all of your SalesforceIQ accounts from the custom JavaScript action.


### Setup client-side code:
```javascript--persistent
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: '<your action name>'
    }
});

```

Once you've configured the action to connect to SalesforceIQ, you'll need to call the action from your client-side code. To do so, use the following JavaScript to construct a GET request and trigger your SalesforceIQ action:

Simply replace 'your object name' with the object that contains your SalesforceIQ custom action, and replace 'your action name' with the name of the action that you provided while creating the integration.

With these changes, you're now able to pull in any and all SalesforceIQ accounts available via their API! You can use a similar pattern to construct additional calls to the SalesforceIQ API - simply replace the URL and parameters in the custom SalesforceIQ action with the URL and parameters for the object you want to retrieve.

## Salesforce CRM
As mentioned in our article on integrating with SalesforceIQ, building and maintaining customer relationships is crucial for driving sales to your platform. However, it can also be a complex process requiring integrating data from multiple sources and, more importantly, ensuring that data is accessible when it is needed. Customer Relationship Management (CRM) software is designed to make this data management and integration process much easier, providing you with the tools you need to drive customers through your sales funnel. In this article, we'll look at integrating a Backand application with Salesforce CRM, providing you with all of the tools you need to effectively leverage your customer data in your Backand application.

### What is Salesforce CRM?
[Salesforce CRM](https://www.salesforce.com/crm) is the world's foremost CRM solution, and gives your sales teams the tools they need to close deals. Salesforce CRM is also built in the cloud, meaning that your sales team can increase their productivity and keep the sales pipeline filled with solid leads, all without the need to deploy additional hardware or work around speed limitations. Through Salesforce's [REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm ) you can get easy programmatic access to all of your customer data.

### Connecting a Backand application with Salesforce CRM
Integrating Backand and SalesforceIQ is as simple as leveraging the full-featured [REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm ) provided by Salesforce from within your Backand application. While traditionally you'd need to make these calls to the API from a server, you can achieve the same functionality in Backand by using our custom template action. This action provides an easy-to-use code template for making calls to the Salesforce REST API, letting you update your user tracking data based upon user activity in your Backand app, or even update your Backand app's UI based upon what you know about your user in Salesforce.


### The Salesforce CRM Action Template
```javascript--persistent
/* globals
  $http - Service for AJAX calls
  CONSTS - CONSTS.apiUrl for Backands API URL
  Config - Global Configuration
  socket - Send realtime database communication
  files - file handler, performs upload and delete of files
  request - the current http request
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here
	var baseUrl = "https://eu11.salesforce.com/services/data/v37.0/";

  // This function manages authenticating with Salesforce, providing the access
  // token as a return value
	var loginSalesForce = function() {
    	var client = "-- Your Client Id --"; //Consumer Key
    	var secret = "-- Your Secret Id --"; //Consumer Secret

    	var username = "-- User Username --";
    	var password = "-- User Password --";

    	var loginUrl = "https://login.salesforce.com/services/oauth2/token";

        var response = $http({
            method: "POST",
            url: loginUrl,
            data: "grant_type=password" +
                "&username=" + username +
                "&password=" + password +
                "&client_id=" +  client +
                "&client_secret=" + secret,
            headers: {
                "Accept":"application/json",
                "Content-Type": "application/x-www-form-urlencoded"
            }
        });

        console.log(response.access_token);
        return response.access_token;
	}

    //get access token first. Check if access token exists in cookie session
    var accessToken = cookie.get('sf_access_token');
    if(accessToken == null){
        accessToken = loginSalesForce();
        cookie.put('sf_access_token', accessToken);
    }

    //get list of Opportunities
    var opps = $http({
        method: "GET",
        url: baseUrl + "sobjects/Opportunity",
        headers: {"Authorization": "Bearer " + accessToken}
    });

	return opps;
}
```

We have created an action template that will give you jump start with salesforceCRM. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following is the content of the ready action template call
"salesforce CRM" under the "CRM & ERP" section:

<aside class="notice">The above code fetches the access token every time you call the action. You can improve the performance of this call by caching the token into a Backand Server-Side Cookie, and only performing the retrieval when the token has expired.</aside>

### Setup access and get client and secret keys
Follow these steps to obtain your Salesforce CRM authentication information:

1. Sign in to Salesforce CRM as a user with Admin rights
2. Open Setup, found under the gear icon
3. Open App Manager
4. Click on 'New Connected App'
5. Provide 'Connected App Name', 'API Name' and 'Contact Email'
6. Check 'Enable OAuth Settings'
  1. Check 'Enable for Device Flow'
  2. Under 'Selected OAuth Scopes' select 'Full Access,' or any other permissions you need
7. Click 'Save'
8. Copy 'Consumer Key' into the client variable in the code
9. Copy 'Consumer Secret' into the secret variable in the code

Once you've obtained the consumer key and the consumer secret, you'll need to enable server-side security in Salesforce. To do so, follow these steps:

1. From App Manager, select your new App and Click 'Manage'
2. Click 'Edit Polices'
3. Change 'IP Relaxation' to 'Relax IP Restriction,' or add Backand's IP to your organization's IP restrictions
4. Change 'Timeout Value' to 24 hours
5. Click 'Save'

With that completed, you should now have full access to your CRM objects using the Salesforce REST API.

### Setup client-side code:

```javascript--persistent
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: '<your action name>'
    }
});

```
Once you've configured the action to connect to Salesforce, you'll need to call the action from your client-side code. To do so, use the following JavaScript to construct a GET request and trigger your Salesforce action:

Simply replace 'your object name' with the object that contains your Salesforce custom action, and replace 'your action name' with the name of the action that you provided while creating the integration.

With these changes, you're now able to pull in any and all Salesforce accounts available via their API! You can use a similar pattern to construct additional calls to the Salesforce API - simply replace the URL and parameters in the custom SalesforceIQ action with the URL and parameters for the object you want to retrieve.
