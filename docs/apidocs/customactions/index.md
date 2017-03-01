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

-- A Note About The Authorization Header:  
Sending the authorization header to the $http function is optional. When you make a $http request with an authorization header and the value of the userProfile.token (as in the examples above), the request will run in the context of the current user and with his assigned role. Alternatively, if you choose not to send an authorization header, the action will run in the context of an admin role. Send the authorization header with the request if you are going to use information about the current user in the action, otherwise you do not need to do so.

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

### Server-Side Node.js Code

Using Backand, you can develop distributed Node.js actions and host them with your Backand application - no additional servers needed! You can use the Server-Side Node.js action to work with any NPM package, build sophisticated action behaviors, perform complex coding tasks, and more.

For Server-Side Node.js Code actions, you develop the code on your local machine. The code is then deployed to, and runs on, Backand's server. It functions just like any other Node.js project and can be fully debugged locally and, once you've finished making changes, you can use the "deploy" command to publish the changes to your Backand application.

Follow these steps to create and run a Server-Side Node.js Code Action:

* First name the action, and use the init command by copy-pasting "backand action init..." to a command line within the folder in which you'll be developing your action. The action init command creates two folder levels on your local file system. The top level folder will be the name of the object controlling the action, while its child folder is the name of the action you are working with. We recommend that you always run the **action init** command from the root folder of the app's project when creating additional actions. This means you will have a sub-folder for each object, and under it, a sub -folder for each action.
* Next, build your Node.js code in the **action** folder like any Node other project using your preferred IDE. Add as many npm 
packages as you need. Note - Your code **must** start with the index.js file - Backand uses this as the starting point for your Node.JS action.
* To debug your code locally, run the debug.js file. Debug.js is provided by the init command, and is ignored when deploying the code to Backand.
* To run your code on the Back& server, use the deploy command by copy-pasting "backand action deploy..." into the command line used in the first step. The 
deploy command will be available on the action page **after** the action has been initialized with "backand action init...".

You are now ready to develop your code locally, and test the action on the Backand Dashboard for your application!

### Backand CLI

Backand uses the Backand CLI to control deployment and initialization. The CLI requires that Node.js and NPM are both installed. You can install them on your local machine by following the instructions at [https://nodejs.org/](https://nodejs.org/).

Once you've set up Node and NPM, run the following command to install the Backand CLI tool: 

```
    $ npm install -g backand

    # or use sudo (with caution) if required by your system permissions
    # sudo npm install -g backand
```

### Initialize action

To initialize the node.js code for the action on your local machine, use the following command on the command line in the folder that will host your action's code:

```
    $ backand action init --app <app name> --object <object name> --action <action name>  --master <master token> --user <user token>
```

The parameters for this call are:
  **--app**:		  The current app name  
  **--object**:		The object that the action belongs to  
  **--action**:		The action name  
  **--master**:		The master token of the app (obtained from the Social & Keys section of the app's Security & Auth configuration)  
  **--user**:		  The token of the current user (available from the TEam section of the app's Security & Auth configuration - simply click on key icon next to an authorized user)  


### Deploy action

To deploy your local Node.js code to Back&, use the following command on the command line:

```
    $ backand action deploy --app <app name> --object <object name> --action <action name>  --master <master token> --user <user token>
```

The parameters for this call are:
  **--app**:		  The current app name  
  **--object**:		The object that the action belongs to  
  **--action**:		The action name  
  **--master**:   The master token of the app (obtained from the Social & Keys section of the app's Security & Auth configuration)  
  **--user**:     The token of the current user (available from the TEam section of the app's Security & Auth configuration - simply click on key icon next to an authorized user)  
  **--folder**:   (Optional) The folder to deploy. By default the deployment occurs in the current folder
  
### Add Backand SDK to Node.js

To work with objects or other actions in Backand, you need to install the Backand SDK for Node.js. The Backand SDK ships with several code samples that demonstrate how to access other Backand functionality.

```
    $ npm install backandsdk --save
```

## Transactional Database Scripts

Transactional database scripts are SQL scripts that run within the same transaction context as the triggering action, provided that the event occurs during the object event "During the data save before the object is committed". This means that if the Create, Update or Delete request fails then your script will be rolled back like any other transaction.


## Send Emails

Send Email actions, in addition to common parameters, allow you to also supply the usual email fields: To, Cc, Bcc, From, Subject and Message. You can additionally provide an object ID to obtain a deep object to use in the action.

