## What is salesforce CRM?
[salesforce CRM](https://www.salesforce.com/crm) salesforce CRM The world’s #1 CRM solution gives your sales teams the
power to close deals like never before with an array of cloud-based tools that increase productivity, keep pipeline filled with solid leads, and score more wins. No software. No hardware. No speed limits.

## Build mobile app with Back& and salesforce CRM
Build custom mobile application ...


## Build  API
salesforceCRM has an [API](https://api.salesforceiq.com/) that you can use to send manage (CRUD) and data in the salesforceCRM. By translating their provided cURL commands to Angular $http calls, you can easily integrate salesforceCRM with Backand.

We have created an action template that will give you jump start with salesforceCRM. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following is the content of the ready action template call
"salesforce CRM" under the "CRM & ERP" section:

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
	var baseUrl = "https://eu11.salesforce.com/services/data/v37.0/";

	var loginSalesForce = function() {
    	var client = "-- Your Client Id --"; //Consumer Key
    	var secret = "-- Your Secret Id --"; //Consumer Secret

    	var username = "-- User Username --";
    	var password = "-- User Password --";

    	var loginUrl = "https://login.salesforce.com/services/oauth2/token";

        var response = $http({
            method: "POST",
            url: loginUrl,
            data: "grant_type=password" +
                "&username=" + username +
                "&password=" + password +
                "&client_id=" +  client +
                "&client_secret=" + secret,
            headers: {
                "Accept":"application/json",
                "Content-Type": "application/x-www-form-urlencoded"
            }
        });

        console.log(response.access_token);
        return response.access_token;
	}

    //curl https://eu11.salesforce.com/services/oauth2/token -d username=itay@domain.com -d password=pass123456 -d
    grant_type=password -d client_id=3MVGxxxxxxxxx2fryplFKWxad9i8kqvIke4.2RMNS.2tE4aHabV4RkcxxxxxxEhGosZLleSv8W -d
    client_secret=471458111111182016

    //get access token first. Check if access token exists in cookie session

    //get list of Opportunity

    var accessToken = cookie.get('sf_access_token');
    if(accessToken == null){
        accessToken = loginSalesForce();
        cookie.put('sf_access_token', accessToken);
    }

    var opps = $http({
        method: "GET",
        url: baseUrl + "sobjects/Opportunity",
        headers: {"Authorization": "Bearer " + accessToken}
    });

	return opps;
}

** In the above code you need to get the access token for every call. For better performance you can save it in
Backand server side cookie and only get it after it expires.

## Setup access and get client and secret keys

Sign-in to Salesforce CRM with Admin rights and do the following steps:
1. Open Setup under the gear icon
2. Open App Manager
3. Click on 'New Connected App'
4. Provide 'Connected App Name', 'API Name' and 'Contact Email'
5. Check 'Enable OAuth Settings'
  a. Check 'Enable for Device Flow'
  b. Under 'Selected OAuth Scopes' select 'Full Access' or any other permissions you need
6. Click 'Save'
7. Copy 'Consumer Key' into client variable in the code
8. Copy 'Consumer Secret' into secret variable in the code

You need now to enable the server side security in SalesForce with the following steps:
1. From App Manager, select the new App and Click 'Manage'
2. Click 'edit Polices'
3. Change 'IP Relaxation' to 'Relax IP Restriction' or add Backand IP to your organization IP restriction
4. Change 'Timeout Value' to 24 hours
5. click 'Save'


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

Once this is done, you'll be able to get accounts to any front-end code and add much more calls of salesforce CRM.
