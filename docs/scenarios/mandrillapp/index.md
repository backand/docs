## What is Mandrill?
Mandrill is an email infrastructure service that focuses on transactional emails. Using Mandrill, you can send personalized e-commerce messages on a one-to-one basis, or automated transactional messages for things like password resets, order confirmations, and welcome messages.

## Send Email with Mandrillapp API
Mandrill has an API that you can use to send emails, along with a lot of other functionality. By translating their provided cURL commands to Angular $http calls, you can easily integrate Mandrill with Backand.

To send an email with Mandrill, you need to create a server side action. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action mandrillapp, add message and name to the Input Parameters, and paste the following code in the code editor. When finished, the code editor window will contain the following:

```javascript
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
            "message":{"html":parameters.message,
            "subject":"Example for Mandrill Integration",
            "from_email":userProfile.username,"from_name":parameters.name,
            "to":[{"email":userProfile.username,"name":parameters.name,"type":"to"}],
            "headers":{"Reply-To":"message.reply@backand.com"}}}
    });
    console.log(response);
	return {};
}
```
In the example app we're building, the app's users can send messages to themselves. This is done through the use of userProfile.username, which is the email address used to register with the application. Make sure to replace the 'key' property above with your Mandrill API key.

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  method: 'GET',
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>/1',
  params: {
    name: 'mandrillapp',
    parameters: {
      message: 'Anything you can put your mind on',
      name: '<your name here>'
    }
  }
});

```

Replace <your object name> with the object associated with the action you created.

Once this is done, you'll be able to easily trigger emails via Mandrill using Backand's custom action API.
