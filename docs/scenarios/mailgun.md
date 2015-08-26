#What is Mailgun?
Mailgun is an email automation service provided by Rackspace. It offers a complete cloud-based email service for sending, receiving and tracking email sent through your websites and applications.

#Send Email with Mailgun API
Mailgun has an API that you can use to send emails and a lot of other functionalities. By translating the curl to $http you can easily integrate Mailgun with Backand.
To send an email with Mailgun, you need to create a server side action. You can either trigger this action to an object CRUD action or to call it on demand from the client. The following example shows the on demand option. Go to one of your objects Actions tab and create a new on demand, server side javascript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action SendEmail, add message and name to the Input Parameters and Paste the following code in the code editor so it will show the following:

```
/* globals
  $http - service for AJAX calls - $http({method:"GET",url:CONSTS.apiUrl + "/1/objects/yourObject" , headers: {"Authorization":userProfile.token}});
  CONSTS - CONSTS.apiUrl for Backands API URL
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here
	
	var apiBaseUrl = "https://api.mailgun.net/v3/sandbox<your sendbox key here>.mailgun.org/messages";

    var apiKey = "api:<you mailgun key here>";
                  
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
	            subject: 'testing mailgum with backand', 
	            text: parameters.message
	        }
	        
	    }
	);
	
    console.log(response);
	return {};
}
```
in this example the app user can send any message that he wants to himself. Please replace the "key" property with your mailgun key.

In your app, use the following code:
```
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
Replace <your object name> with the object associated with the action you created.
