#What is Mandrill?
Mandrill is an email infrastructure service that you can use Mandrill to send personalized, one-to-one e-commerce emails, or automated transactional emails like password resets, order confirmations, and welcome messages.

#Send Email with Mandrillapp API
Mandrill has an API that you can use to send emails and a lot of other functionalities. 
To send an email with Mandrill, you need to create a server side action. You can either trigger this action to an object CRUD action or to call it on demand from the client. The following example shows the on demand option. Go to one of your objects Actions tab and create a new on demand, server side javascript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action SendEmail, add message and name to the Input Parameters and Paste the following code in the code editor so it will show the following:

```
/* globals
  $http - Service for AJAX calls 
  CONSTS - CONSTS.apiUrl for Backands API URL
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here
    var response = $http({
      method: "POST",
      url: "https://mandrillapp.com/api/1.0/messages/send.json",
      data: {"key":<enter your mandrill key>,
            "message":{"html":parameter.message,
            "subject":"Example for Mandrill Integration",
            "from_email":userProfile.username,"from_name":parameters.name,
            "to":[{"email":"itay@backand.com","name":"itay to","type":"to"}],
            "headers":{"Reply-To":"message.reply@backand.com"}}}
    });
    console.log(response);
	return {};
}
```
in this example the app user can send any message that he wants to itay@backand.com. Please replace the "key" property with your mandrill key.

In your app, use the following code:
```
return $http ({
  method: 'GET',
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>/1',
  params: {
    name: 'mandrillapp',
    parameters: {
      message: 'Anything you can put your mind on',
      name: 'Relly'
    }
  }
});

```
Replace <your object name> with the object associated with the action you created.
