Today many AngularJS developers are using Ionic to build the user interface for their mobile apps.  AngularJS provides the app structure.  But these mobile apps still need a backend.  In this blog we’ll give you the recipe you need to build an Ionic mobile app with Backand’s Backend as a Service (BaaS). Review the working demo [here](https://github.com/backand/simple-rest-ionic).

# What is Ionic?

The [Ionic](http://ionicframework.com/) framework is a mobile programming framework for constructing hybrid mobile apps. Hybrid mobile apps use a web technology (HTML, JS, CSS) to build cross-platform mobile apps (iOS, Android).  Ionic is built on top of the [Cordova](https://cordova.apache.org/) hybrid app technology.

The major advantage of using Ionic over Cordova itself or its commercial version [PhoneGap](http://phonegap.com/) is that it provides a plethora of mobile user interface templates both for the entire structure of the app (side menu, tabs) and for common parts (navigation top bar).

Ionic is built using the well known [AngularJS](https://angularjs.org/) web MVC framework.  Hence, developing Ionic apps is done at a higher level of abstraction than using plain HTML and jQuery. To facilitate working with the mobile device APIs in the AngularJS way, Ionic developed [ngCordova](http://ngcordova.com/), a set of AngularJS components for interacting with the various mobile device capabilities (like taking pictures).  Ionic also provides a set of convenient command line tools for building and running apps.

# The Perfect Recipe for Building Mobile Apps

In this scenario we’ll provide you with the recipe you need to use the Ionic template for a mobile app with a side menu.

## Backand

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

## Ionic

Now create your mobile app, naming it ‘PlaylistsApp’, with the Ionic command-line tools.

## JS Libraries

Your Ionic app has a ‘www’ folder which is a single page application (SPA) built with AngularJS.  You will need to include the Backand libraries in your application. Either download them or use bower to obtain them. Then, copy them into your ‘index.html’.

* [angularbknd-sdk](https://github.com/backand/angularbknd-sdk), install with bower using:
    `bower install angularbknd-sdk`
* Include in your ‘index.html':
```html
     <script src="lib/angularbknd-sdk/dist/backand.min.js"></script>
     <script src="lib/angular-cookies/angular-cookies.js"></script>
```

## Authorization

Backand uses authorization headers in HTTP requests. The token for authorization is obtained upon sign in to Backand and is then stored using cookies via ‘ngCookies’. The Backand sdk adds authorization headers to each HTTP request using an ‘$http’ interceptor.

In your main ‘js’ file, ‘app.js’ requires ‘backand':

```javascript
     var myApp = angular.module('starter', ['ionic', 
       'starter.controllers', 'backand', 'starter.services']);
```

And in your app ‘config’ add:

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

Sign In

## In your controllers module require ‘Backand’ and ‘ngCookies':


```javascript
    var myModule = angular.module('starter.controllers', ['backand', 'ngCookies']);
```

In your login controller require ‘Backand':

```javascript
    myModule.controller('loginCtrl', function($scope, Backand) {});
```
To perform the login call, remember that ‘playlists’ is our Backand database name:


```javascript
    var appName = 'playlists';
    Backand.signin(username, password, appName).then(
      function(data){}, function(error){});
```

## CRUD

Now you will need to define a service to make the database operations.  In your services module require ‘backand':


```javascript
    var myServices = angular.module('starter.services', ['backand'])
```

Define a ‘DatabaseService’, require ‘Backand':

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

## Using Backand Data

In your controller, require ‘DatabaseService’, and make calls like:

```javascript
    myModule.controller('playlistsCtrl',    
      function($scope, DatabaseService) {
 
      $scope.playlists = [];
      
      // read all playlists for a user
      DatabaseService.readAll('playlists').then(
        function9data){
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
