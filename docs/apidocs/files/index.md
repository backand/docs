### Introduction
Backand's Files provides you the functionality to upload and delete files in Backand storage. This is done in the server side through Backand's actions. It does not requires any additional authentication and it is up to you to decide if and under what restrictions to expose this functionality to the client side. For example, you can restrict certain roles, handle the name of the files, associate the files with objects and manage count of the amount of files per users. The upload action returns a url that links to the file you uploaded. This is a public url. The storage is managed per Backand's app.  

### Angular client code

Following is a working example in [codepen](http://codepen.io/relly/pen/mVMOwZ)

Javascript:

```
var myApp = angular.module('myTodoApp', ['backand']);

// Backand security configuration for your app
myApp.config(function(BackandProvider) {
  // enter your app name here
  BackandProvider.setAppName('filetest01');
  // enter your app anonymous token here
  BackandProvider.setAnonymousToken('de8d5df4-2178-4ad2-a4f3-5933bc653e97');
  BackandProvider.setApiUrl("http://localhost:4110");
})

myApp.controller('DemoCtrl', ['$scope', '$http', 'Backand', DemoCtrl]);

function DemoCtrl($scope, $http, Backand) {

  // Create a server side action in backand
  // by going into the items object actions tab 
  // and click on the S3 icon.
  // Name your action files
  // Backand consts:
  var baseUrl = '/1/objects/';
  var objectName = 'items';
  var filesActionName = 'files';
  
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
      });
    };
    reader.readAsDataURL(file);
  };

  // register to change enent on input file 
  function initUpload() {
    var fileInput = document.getElementById('fileInput');

    fileInput.addEventListener('change', function(e) {
      imageChanged(fileInput);
    });
  }

   // call to Backand action with the file name and file data  
  function upload(filename, filedata) {
    // By calling the files action with POST method in will perform 
    // an upload of the file into AWS S3
    return $http({
      method: 'POST',
      url: Backand.getApiUrl() + '/1/objects/action/items?name=' + filesActionName,
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
    // a deletion of the file from AWS S3
    $http({
      method: 'DELETE',
      url: Backand.getApiUrl() + '/1/objects/action/items?name=' + filesActionName,
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

HTML

```
<body class="container" ng-app="myTodoApp" ng-controller="DemoCtrl" ng-init="initCtrl()">
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

### Server side code
Both upload and delete are written in the same action and it is the method the you are using to call it that determines the functionality. When the method is POST then the action performs upload and when it is DELETE the action performs delete. You can use the userProfile for restrictions or file name manipulations.

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

