## What is Mandrill?
Mandrill is an email infrastructure service that focuses on transactional emails. Using Mandrill, you can send personalized e-commerce messages on a one-to-one basis, or automated transactional messages for things like password resets, order confirmations, and welcome messages.

## Send Email with Mandrill API (No Attachment)
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
      method: 'POST',
      url: 'https://mandrillapp.com/api/1.0/messages/send.json',
      data: {'key':<enter your mandrill key>,
            'message':{'html':parameters.message,
            'subject':'Example for Mandrill Integration',
            'from_email':userProfile.username,'from_name':parameters.name,
            'to':[{'email':userProfile.username,'name':parameters.name,'type':'to'}],
            'headers':{'Reply-To':'message.reply@backand.com'}}}
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


## Send Email with Mandrill API (with Attachments)

Below, we’ll create a server-side Node JS action that imports the Mandrill SDK, allowing you to easily send email with attachments through Mandrill in your Backand application.

### Getting Started in Backand

To get started with a Mandrill action, you’ll need to first create a new Server-Side NodeJS action in Backand. To do so, open up your app’s dashboard at [https://www.backand.com](https://www.backand.com), and navigate to an object that will host the action. On the Actions tab of this object, create a new 'On Demand' action. Name this action, then select 'Server-Side Node.JS Action' as the action’s type. Follow the documentation provided to download the Backand CLI and initialize your action locally.

### Initializing the Mandrill Action

Once you have the Server-Side Node.js action ready to go, the next step is to create an action in Mandrill. Navigate to [http://www.mandrill.com/](http://www.mandrill.com/) and create a new action in the MailChimp dashboard provided when you log in. Once the action is ready, copy down the Mandrill API Key value and save it somewhere safe. This API key will be used by the Node.js action to connect and communicate with Mandrill.

### Updating the Code

Next, we’ll need to update the action’s code to communicate with Mandrill. We’ll start by  configuring the mandrill SDK requirements in index.js. Add these lines into index.js in your Node.js action folder structure, before the function exports.backandCallback:

```javascript
var mandrill = require('node-mandrill')(MANDRILL_API_KEY);
var request = require('request').defaults({ encoding: null });
```

**Note:** Be sure to replace MANDRILL_API_KEY with the value copied down from the Mandrill dashboard.

Next, we’ll modify the backandCallback function to send a message with Mandrill. Replace the contents of this function with the following code:

```javascript
    var filePath = 'PATH_TO_ATTACHMENT_INCLUDING_FILENAME';
    var fileName = 'FILENAME_OF_ATTACHMENT';

    request.get(filePath, function (error, response, body) {
      if (!error && response.statusCode == 200) {
        sendEmail(fileName, response.headers['content-type'], new Buffer(body).toString('base64'));
      }
    });

    function sendEmail(fileName, fileType, fileContent) {
      mandrill('/messages/send', {
        message: {
          to: [{email: 'RECIPIENT_EMAIL', name: 'RECIPIENT_DISPLAY_NAME'}],
          from_email: 'SENDER',
          subject: 'EMAIL_SUBJECT',
          text: 'MESSAGE_BODY',
          attachments: [{
            'type': fileType,
            'name': fileName,
            'content': fileContent
          }]
        }
      }, function (error, res) {
        //uh oh, there was an error
        if (error) {
          //console.log(JSON.stringify(error));
          response(error, null);
        }
        //everything's good, lets see what mandrill said
        else {
          console.log(res);
          response(null, res);
        }


      });
    }
```

This code does two things:

1. The initial code fetches the attachment data from the server on which it is stored. If this code succeeds, it calls sendEmail with the file data provided as a Base 64 string.

2. sendEmail then takes this data and contacts Mandrill to send the message.

### Configuring the Code

To tie the project together and finalize the above code, simply replace each of the placeholders with the appropriate value:

* PATH_TO_ATTACHMENT_INCLUDING_FILENAME - this is the URL to the attachment you wish to send. If fetching this fails, sending the message will not succeed.

* FILENAME_OF_ATTACHMENT - this is the file name that will be given to the attachment.

* RECIPIENT_EMAIL - This is the email for the message’s intended recipient.

* RECIPIENT_DISPLAY_NAME - This is the recipient’s display name.

* SENDER - This is the email from which the message was sent.

* EMAIL_SUBJECT - This is the subject of the email.

* MESSAGE_BODY - This is the content of the email.

You can also use the parameters argument to the action to send additional message data, whether that data originates in your app’s database or via the API call. Once these changes are made, your server-side action is ready to deploy!

### Testing and Deployment

You can debug and run the action locally using the provided debug.js file. Simply enter node debug.js on the command line to debug. Once you’ve finished your local testing, you can then deploy the action via the documentation provided in the Server-Side Action’s UI in your app’s dashboard at Backand.com - head to the Actions tab for the relevant object, and follow the instructions on using backand action deploy to deploy your code.

### Calling the Action

To call the action in your client-side code, simply use the Backand SDK’s action functionality as you would any other on-demand action:

```javascript
  return Backand.object.action.get('OBJECT_NAME', 'ACTION_NAME', {
    'parameters': {}
  })
```

Simply replace OBJECT_NAME with the name of the object controlling your action, and ACTION_NAME with the Server-Side Node.js Action’s name in Backand. You can provide any extra information or detail using the provided parameters object - this will be passed into the parameters argument of your action.
