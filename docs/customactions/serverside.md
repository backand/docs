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
* $http - a service for AJAX calls
* `$http({method:"GET",url:CONSTS.apiUrl + "/1/objects/objectexample" , headers: {"Authorization":userProfile.token}});`
* CONSTS - CONSTS.apiUrl for Backands API URL
* `console.log(message, object)` and `console.error(message, object)`, to debug your code
Automated actions will have a response that matches the format expected by the triggering call (such as the return value of a CREATE call). On Demand actions, though, will return whatever is returned by the custom server code, which can be any properly-formatted JSON.

##Error Handling

If your code results in an error (for example, if you write the following: `throw new Error("An error occurred!"))`, the request will return HTTP status 417, and the response body will contain the associated error message.

