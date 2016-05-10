## Introduction
In Backand's system, you can create server-side activity called Actions. These actions can be used for the purpose of security, integration, performance, notification and data integrity, among others, providing you with more flexibility in your app's design. There are two types of Actions that can be created. The first are initiated via a direct web request. These are known as "On Demand" actions. Additionally, you can create automated actions that take place based upon a data interaction event. These automated actions can occur whenever you create, update, or delete an item in your system. On Demand actions are associated with a specific object, and can be found on the Object --> {name} page in the Actions tab. The automated Create, Update and Delete actions are associated with a specific object that is compatible with a specific row in a table, while On Demand actions make association with a specific role optional.

There are 4 kinds of actions that can be created for either action type:

- Server side JavaScript code actions
- Server side node.js code actions
- Transactional database script actions
- Send Email actions

All 4 types of actions use the following common parameters:

* A Where condition - a SQL where clause that determines if the action will be performed.
* Input Parameters, added to the query string of the request that triggers the action, that will serve as variable values that you can supply to your action's code. These parameters will serve as tokens in the action definition and will be replaced with the actual values when the code executes.

## Server-side JavaScript Code

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

* $http: a service for HTTP calls, similar to Angular's $http without the promise (since it is a server side
function it always runs in sync). [See the full API description for more details](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#rest-api-crud-operations):
    * GET example: 

			var response = $http({method:"GET",url:CONSTS.apiUrl + "/1/objects/objectexample", 
			                       params:{filter:[{fieldName:"fieldexample", operator:"contains", value:"somestring"}]}, 
			                      headers: {"Authorization":userProfile.token}});

    * POST example: 
    
			var response = $http({method:"POST",url:CONSTS.apiUrl + "/1/objects/objectexample", 
			                      data:{fieldexample1:"somevalue",fieldexample2:"somevalue"}, 
			                      headers: {"Authorization":userProfile.token}});

    * PUT example: 
    
			var response = $http({method:"PUT",url:CONSTS.apiUrl + "/1/objects/objectexample/5", 
			                      data:{fieldexample1:"somevalue",fieldexample2:"somevalue"}, 
			                      headers: {"Authorization":userProfile.token}});

    * DELETE example: 

			var response = $http({method:"DELETE",url:CONSTS.apiUrl + "/1/objects/objectexample/5", fieldexample2:"somevalue"}, 
			                      headers: {"Authorization":userProfile.token}});

* CONSTS: CONSTS.apiUrl for Backand's API URL

* Config: Global configuration. You can maintain a global JSON configuration for your app. Your JSON configuration is consumed in the Config action. To update the configuration JSON, go to section "General" in the "Settings" menu on the Backand dashboard.

* Emit: Emit is a function that allows you to send real-time communication events and data to the client. Emit has 3 methods: socket.emitUsers, socket
.emitRole, and socket.emitAll. Read more about [Realtime Database Communication here](http://docs.backand.com/en/latest/apidocs/realtime/index.html).

### Debugging

Debugging should be done using either console.log or console.error. For example, to dump the contents of variable 
'object':
'console.log(object)'

'console.error(object)'

### Error Handling

If your code results in an error (for example, if you write the following: 'throw new Error("An error occurred!")'), 
the request will return HTTP status 417, and the response body will contain the associated error message.

### Return values

Triggered actions will have a response that matches the format expected by the triggering call (such as the return value
of a CREATE call).

On Demand actions, though, will return whatever value is returned by the custom server code, which can be any properly-formatted JSON string.

## Server-side node.js Code

You can develop distributed node.js actions and host them on Backand - Hassle-Free Server-Side - No additional servers
 needed!
Using server-side node.js action you can implement any npm package on Back&, build sophisticated solutions, complex 
coding and more.

The server side node.js code is developed on your local computer, and runs on Back& server.
The project is been developed like any other node.js project and can be fully debug locally. To have it running on 
Back& server you just call the deploy command.

Follow these steps to initiate and run the action with the server side node.js code:

* First, name the action and use the init command by copy paste "backand action init...". The action init command 
creates two levels of folders on your local file system. The top level folder is the name of the object and the 
second level folder is the name of the action. We recommend to run the **action init** command for additional actions
 from the root folder of the app's project. This means you will have sub folder for each object and under it sub 
 folder for each action.
* Second, build your node.js code in the **action** folder like any other project using your local IDE. Add npm 
packages as many as you need.
* Your code **must** start with the index.js file.
* To debug your code locally, run the debug.js file (the debug.js file is ignored on the Back& server).
* In order to run your code on Back& server, use the deploy command by copy paste "backand action deploy...". The 
deploy command will be available in this page **after** running the init action.
* To test the action on Back& you can do it from the Test section on this page.

### Backand CLI

The Backand CLI (command-line interface), which is used to control deployment, requires Node.js and npm to be installed. 
Both can be installed by following the instructions at [https://nodejs.org/](https://nodejs.org/).

Run the following command to install Backand CLI tool: 

```
    $ npm install -g backand

    # or use sudo (with caution) if required by your system permissions
    # sudo npm install -g backand
```

### Initiate action

To initiate on your local folder the node.js code for the action, use the following command on the command line

```

    $ backand action init --app <app name> --object <object name> --action <action name>  --master <master token> --user <user token>
```

  **--app**:		  The current app name  
  **--object**:		The object that the action belongs to  
  **--action**:		The action name  
  **--master**:		The master token of the app (get it from Social & Keys)  
  **--user**:		  The token of the current user (get it from Team and click on key icon)  


### Deploy action

To deploy your local node.js code to Back&, use the following command on the command line:

```

    $ backand action deploy --app <app name> --object <object name> --action <action name>  --master <master token> --user <user token>
```

  **--app**:		  The current app name  
  **--object**:		The object that the action belongs to  
  **--action**:		The action name  
  **--master**:		The master token of the app (get it from Social & Keys)  
  **--user**:		  The token of the current user (get it from Team and click on key icon)  
  **--folder**:   Optional, By default the deployment is from the current folder, specify for other location
  
### Add Backand SDK to node.js

In order to call objects or other actions in Back& you need to install Back& SDK for node.js. Code example on how to 
use it is included in index.js in remark.

```
    
    $ npm install backandsdk --save
```

## Transactional Database Scripts

Transactional database scripts are SQL scripts that run within the same transaction context as the triggering action, provided that the event occurs during the object event "During the data save before the object is committed". This means that if the Create, Update or Delete request fails then your script will be rolled back like any other transaction.

## Send Emails

Send Email actions, in addition to common parameters, allow you to also supply the usual email fields: To, Cc, Bcc, From, Subject and Message. You can additionally provide an object ID to obtain a deep object to use in the action.

