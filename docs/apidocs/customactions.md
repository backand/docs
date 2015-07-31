In Backand's system, you can create server-side activity called Actions. These actions can be used for the purpose of security, integration, performance, notification and data integrity, among others, providing you with more flexibility in your app's design. There are two types of Actions that can be created. The first are initiated via a direct web request. These are known as "On Demand" actions. Additionally, you can create automated actions that take place based upon a data interaction event. These automated actions can occur whenever you create, update, or delete an item in your system. On Demand actions are associated with a specific object, and can be found on the Object --> {name} page in the Actions tab. The automated Create, Update and Delete actions are associated with a specific object that is compatible with a specific row in a table, while On Demand actions make association with a specific role optional.

There are 3 kinds of actions that can be created for either action type:

- Server side JavaScript code actions
- Transactional database script actions
- Send Email actions

All 3 types of actions use the following common parameters:

* A Where condition - a SQL where clause that determines if the action will be performed.
* Input Parameters, added to the query string of the request that triggers the action, that will serve as variable values that you can supply to your action's code. These parameters will serve as tokens in the action definition and will be replaced with the actual values when the code executes.

# Server-side JavaScript Code

You can run standard JavaScript on the server. It runs on the [V8 engine](http://en.wikipedia.org/wiki/V8_(JavaScript_engine)). To execute the JavaScript action, put your code into the following function:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
    // write your code here
    return {};
}
```

The function parameters are:

* **userInput**: This parameter is only provided for Create and Update actions, and is the object that was sent to the action. It is null for Delete actions, as well as for On Demand actions.
* **dbRow**: This parameter is populated in After Create, Update, and Delete automated actions, and if you supply an optional ID to an On Demand action. The dbRow parameter will contain the row's entry in the database prior to any changes made.
* **parameters**: This parameter represents the variables sent in the query string for the action.
* **userProfile**: This parameter stores the current username, the user's role, and the access token used by the user to perform the action. It is of the format {"username": "string", "role": "string", "token": "string"}.

In addition to the above parameters, you can also make use of the following global objects:

* $http - a service for HTTP calls, similar to Angular $http but without the promise, since it is a server side function it always runs in sync;
    * GET example: 

        ```
            var response = $http({method:"GET",url:CONSTS.apiUrl + "/1/objects/objectexample", 
                                   params:{filter:[{fieldName:"fieldexample", operator:"contains", value:"somestring"}]}, 
                                  headers: {"Authorization":userProfile.token}});
         ```
    
    * POST example: 
    
        ```
            var response = $http({method:"POST",url:CONSTS.apiUrl + "/1/objects/objectexample", 
                                  data:{fieldexample1:"somevalue",fieldexample2:"somevalue"}, 
                                  headers: {"Authorization":userProfile.token}});
        ```
    
    * PUT example: 
    
        ```
            var response = $http({method:"PUT",url:CONSTS.apiUrl + "/1/objects/objectexample/5", 
                                  data:{fieldexample1:"somevalue",fieldexample2:"somevalue"}, 
                                  headers: {"Authorization":userProfile.token}});
        ```
    
    * DELETE example: 
    
        ```javascript
            var response = $http({method:"DELETE",url:CONSTS.apiUrl + "/1/objects/objectexample/5", fieldexample2:"somevalue"}, 
                                  headers: {"Authorization":userProfile.token}});
        ```

* CONSTS - CONSTS.apiUrl for Backands API URL
* `console.log(object)` and `console.error(object)`, to debug your code

Automated actions will have a response that matches the format expected by the triggering call (such as the return value of a CREATE call). On Demand actions, though, will return whatever is returned by the custom server code, which can be any properly-formatted JSON.

##Error Handling

If your code results in an error (for example, if you write the following: `throw new Error("An error occurred!"))`, the request will return HTTP status 417, and the response body will contain the associated error message.

##3rd Parties Inegrations

You can use the above server side javascript to integrate with 3rd parties that have a rest api integration capabilities.
Following are examples for such integrations:

### Upload File to S3
For security reasons you can create the following action that uses your S3 account credential in the server side. From the client, you can call this action without your account credentials but under the Backand security which means that only authorized users may use it. In this example the action parameters are the file name and a binary sream of the content of the file, it return a url of the uploaded file.
Create the following action under the relevant object, the relevant object can be the object that you would like to store the url of the uploaded file for display purpose:
* **Action Name** what ever name that you want
* **Event Trigger** On Demand
* **type** Server Side Javascript Code
* **Input Parameters** filename, filedata
* **JavaScript Code** put the following code under the // write your code here
```
var data = 
    {
        // enter your aws key
        "key" : <your account key>, 

        // enter your aws secret key
        "secret" : <your account secret>, 

        // this should be sent in post body
        "filename" : parameters.filename, 
        "filedata" : parameters.filedata,         

        // enter your s3 bucket name
        "bucket" : <your S3 bucket name>

    }
    // this is a short and simple backand's S3 api, call this only from the server side
    var response = $http({method:"PUT",url:CONSTS.apiUrl + "/1/file/s3" , 
               data: data, headers: {"Authorization":userProfile.token}});

    // returns {url:"http://someurl"}
    return response;
```
Following is the client side controller code that uses the above action:
```
    self.s3FileUpload = function(filename, filedata, success, error)
    {
      return $http ({
        method: 'POST',
        url: Backand.getApiUrl() + '/1/objects/action/<the relevant object name>?name=<the action name>',
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
    
    self.imageChanged = function(data) {

      self.loading = true;

      //read file content
      var photoFile = data.files[0];
      var reader = new FileReader();

      reader.onload = function(e) {
        self.s3FileUpload({fileName: photoFile.name, fileData: e.currentTarget.result});
      };
      reader.readAsDataURL(photoFile);
    };
```
Following is the markup:
```
    <div>
        <img ng-src="{{ noteCtrl.note.image }}" style="max-width:250px;" />
        <input
            onchange="angular.element(this).scope().noteCtrl.imageChanged(this)"
            type="file" accept="*/*" style="width: 83px;"/>
    </div>
```
You can review this example in [simple-noterious-app](https://github.com/backand/simple-noterious-app)

# Transactional Database Scripts

Transactional database scripts are SQL scripts that run within the same transaction context as the triggering action, provided the event occurs in the object event "During the data save before the object is committed". This means that if the Create, Update or Delete request fails then your script will be rolled back like any other transaction.

# Send Emails

Send Email actions, in addition to common parameters, allow you to also supply the usual email fields: To, Cc, Bcc, From, Subject and Message. In addition, you can provide an object ID to obtain a deep object to use in the action.

