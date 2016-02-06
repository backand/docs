## What is SendGrid?
SendGrid is a cloud-based SMTP provider that allows you to send email without having to maintain email servers. SendGrid manages all of the technical details, from scaling the infrastructure to ISP outreach and reputation monitoring to whitelist services and real time analytics.

## Who is SendGrid for?
SendGrid is for anyone that needs to send email, whether it’s transactional email or marketing emails and campaigns. We send billions of emails each month, so we can handle your email regardless of volume.

## Send Email with SendGrid API
SendGrid has an API that you can use to send emails, along with a lot of other functionality. By translating their provided cURL commands to Angular $http calls, you can easily integrate SendGrid with Backand.

To send an email with SendGrid, you need to create a server side action. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action SendGrid, add 'to'' and 'message' to the Input Parameters (like that: to, message ), and paste the following code in the code editor. When finished, the code editor window will contain the following:

```javascript
/* globals
  $http - Service for AJAX calls 
  CONSTS - CONSTS.apiUrl for Backands API URL
  Config - Global Configuration
  socket - Send realtime database communication
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {

    var res = $http({
        method:"POST",
        url:"https://api.sendgrid.com/api/mail.send.json",
        data:
            "from=" +userProfile.username+
            "&to=" + parameters.to +
            "&subject=hellow"+
            "&html=" + parameters.message,

         headers: {"Accept":"Accept:application/json",
                "Content-Type": "application/x-www-form-urlencoded",
                "Authorization":"Bearer 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"

         }
     });
     return res;
}
```
In the example app we're building, the app's users can send messages to an email sent by the client side in the 'to' parameter.  Make sure to replace the 'Authorization' header above with your SendGrid API key (after you register with SendGrid you should waite for your account to be provisioned and than you'll see the API KEY under Settings/ API Keys).

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  method: 'GET',
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
  params: {
    name: 'SendGrid',
    parameters: {
      message: 'Anything you can put your mind on',
      to: '<your destination email>'
    }
  }
});

```

Replace <your object name> with the object associated with the action you created and <your destination email> .

Once this is done, you'll be able to easily trigger emails via SendGrid using Backand's custom action API.
