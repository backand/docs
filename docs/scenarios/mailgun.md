# What is Mailgun?
Mailgun is an email automation service provided by Rackspace. It offers a complete cloud-based email service for sending, receiving, and tracking email messages sent by your registered websites and applications.

# Send Email with Mailgun API
Mailgun provides an API that can be used to easily send email, among many other features. By translating Mailgun's provided cURL examples to JavaScript $http calls, you can easily integrate Mailgun with Backand.

To send an email with Mailgun, you need to create a server side action.You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action ,ao;gim, add message and name to the Input Parameters, and paste the following code in the code editor. When finished, the code editor window will contain the following:

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
