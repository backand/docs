### Introduction
Backand provides you with the ability to upload and delete files to and from Backand's robust storage. This is done on the server-side through Backand's Actions. It doesn't require any additional authentication and it is up to you to decide if and under what restrictions to expose this functionality to the client side. For example, you can restrict certain roles, handle the name of the files, associate the files with objects and manage counts of the amount of files per user. 

The files.upload command returns a url that links to the file you uploaded. This is a public url. The storage is managed per Backand app.  

### Backand Server-Side Code
Both upload and delete are written in the same action and the method you use to call it determines the functionality. When the method is POST then the action performs upload and when it is DELETE the action performs delete. You can use the userProfile for restrictions or file name manipulations. You do not need to copy this code. It is ready for you when you click on the Backand *File Storage* action template.

```
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

### Angular Client Code
We created a simple example in Angular to experience the upload functionality. 
You can execute the below code in [codepen](http://codepen.io/backand/pen/ZQaYEV)

HTML

```
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


Javascript:

```
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



