## What is Twilio?
[Twilio](https://www.twilio.com) takes care of dealing with messy telecom hardware, exposing a globally available cloud API that developers can interact with to build intelligent and complex communications systems. As your app's usage scales up or down, Twilio automatically scales with you.

## Who is Twilio for?

Twilio is for anyone that needs to send SMS, MMS, or VoIP, along with a lot of other communication channels embedded into web, desktop, and mobile software.

## Send SMS with Twilio API
Twilio has an API that you can use to send SMS. By translating their provided cURL commands to Angular $http calls, you can easily integrate Twilio with Backand.

To send SMS with Twilio, you need to create a server side action. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action TwilioSendSMS, add to and message to the Input Parameters, and paste the following code in the code editor. When finished, the code editor window will contain the following:

```javascript
/* globals
  $http - Service for AJAX calls
  CONSTS - CONSTS.apiUrl for Backands API URL
  Config - Global Configuration
  socket - Send realtime database communication
  files - file handler, performs upload and delete of files
  request - the current http request
*/
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
	// write your code here
	var ACCOUNT_SID = 'AC2933cadba659d1f15bd409333e3bc38b';
    var AUTH_TOKEN = 'e924350f6eefb5dd1ae011b49d5cb5dd';

    var FROM_PHONE_NUM = 'GetEat';

    var basicUrl = 'https://api.twilio.com/2010-04-01/Accounts/' + ACCOUNT_SID + '/';
    var action = 'Messages.json' // twilio have many services

    return $http({
        method: "POST",
        url: basicUrl + action,
        data:
            'Body=' + parameters.message +
            '&To='  + parameters.to +
            '&From='+ FROM_PHONE_NUM,
        headers: {
            "Accept":"Accept:application/json",
            "Content-Type": "application/x-www-form-urlencoded",
            "Authorization": 'basic ' + btoa(ACCOUNT_SID + ':' + AUTH_TOKEN)
        }
    });

}
```
In the example app we're building, the app's users can send a SMS message to a phone number. The phone number is sent, from the client side, in the 'to' parameter, while the message content is sent in the 'message' parameter. 

## Setup a FREE account in Twilio

After you register with Twilio, you can get your Twilio phone number [here]( https://www.twilio.com/user/account/phone-numbers/getting-started):
1. To choose a different phone number from the one provided, click on *'Don't like this one? Search for a different number.'* and select SMS in capabilities.
2. Replace the FROM_PHONE_NUM in the code with the Twilio phone number obtained in the prior step (dont forget the (+) sign before the number)
3. Make sure you replace the 'ACCOUNT_SID' and 'AUTH_TOKEN' in the code with your Twilio API keys (from the getting started page). Simply click on 'Show API Credentials' on the right side of  'Get Started with Phone Numbers,' and than you'll see the ACCOUNT SID and AUTH TOKEN values.

##Setup client-side code:

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: 'TwilioSendSMS',
      parameters: {
        message: 'Anything you can put your mind on',
        to: '<your destination phone number>'
      }
    }
});

```

Replace 'your object name' with the object associated with the action you created and 'your destination phone number' with a vaild phone number (when using Twilio trial account you first need to validate this phone number)

Once this is done, you'll be able to easily trigger SMS via Twilio using Backand's custom action API.
