## What is salesforceIQ?
[salesforceIQ](https://www.twilio.com) SalesforceIQ gives you the simplicity of a spreadsheet with the horsepower of a full-featured CRM. With Automatic Data Capture and enterprise-level intelligence under the hood, SalesforceIQ acts like your own personal assistant so you can focus on what matters most: selling.

## Build mobile app with Back& and salesforceIQ
Build custom mobile application that uses your customer info can be done by leveraging Back& and salesforceIQ API. To
 eliminate the needs for additional server, security integrations and more, you can use salesforceIQ template Action
 with all other Back& functionality.


## Build  API
salesforceIQ has an [API](https://api.salesforceiq.com/) that you can use to send manage (CRUD) and data in the salesforceIQ. By translating their provided cURL commands to Angular $http calls, you can easily integrate salesforceIQ with Backand.

We have created an action template that will give you jump start with salesforceIQ. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following is the content of the ready action template call
"salesforceiq CRM" under the "CRM & ERP" section:

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

	var API_KEY = '-- YOUR API --';
    var API_SECRET = '-- YOUR SECRET --';

	//get accounts
    var response = $http({
        method: "GET",
        url: "https://api.salesforceiq.com/v2/accounts",
        headers: {
            "Accept":"application/json",
            "Authorization": 'basic ' + btoa(API_KEY + ':' + API_SECRET)
        }
    });

	return response;
}
```

## Setup a Trial account and get the API keys

After you sign-up to salesforceIQ (if you don't have an account yet), do the following steps:
1. Open Settings under the gear icon
2. Open 'Integration' tab under 'My Account Settings'
3. Under 'Create New Custom Integration' click 'Custom'
4. Provide Name as 'Backand API' and Description
5. Copy the 'API Key' and 'API Secret' into the JavaScript Action code
6. Click 'Save'

** Now you should be able to get all your accounts.


##Setup client-side code:

Next, add the following JavaScript code to your app's client-side code base:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: '<your action name>'
    }
});

```

Replace 'your object name' with the object associated with the action you created and 'your action name'.

Once this is done, you'll be able to get accounts to any front-end code and add much more calls of salesforceIQ.
