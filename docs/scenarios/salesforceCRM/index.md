As mentioned in our article on integrating with SalesforceIQ, building and maintaining customer relationships is crucial for driving sales to your platform. However, it can also be a complex process requiring integrating data from multiple sources and, more importantly, ensuring that data is accessible when it is needed. Customer Relationship Management (CRM) software is designed to make this data management and integration process much easier, providing you with the tools you need to drive customers through your sales funnel. In this article, we'll look at integrating a Backand application with Salesforce CRM, providing you with all of the tools you need to effectively leverage your customer data in your Backand application.

## What is Salesforce CRM?
[Salesforce CRM](https://www.salesforce.com/crm) is the world's foremost CRM solution, and gives your sales teams the tools they need to close deals. Salesforce CRM is also built in the cloud, meaning that your sales team can increase their productivity and keep the sales pipeline filled with solid leads, all without the need to deploy additional hardware or work around speed limitations. Through Salesforce's [REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm ) you can get easy programmatic access to all of your customer data.

## Connecting a Backand application with Salesforce CRM
Integrating Backand and SalesforceIQ is as simple as leveraging the full-featured [REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm ) provided by Salesforce from within your Backand application. While traditionally you'd need to make these calls to the API from a server, you can achieve the same functionality in Backand by using our custom template action. This action provides an easy-to-use code template for making calls to the Salesforce REST API, letting you update your user tracking data based upon user activity in your Backand app, or even update your Backand app's UI based upon what you know about your user in Salesforce.


## The Salesforce CRM Action Template
We have created an action template that will give you jump start with salesforceCRM. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following is the content of the ready action template call
"salesforce CRM" under the "CRM & ERP" section:

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
	var baseUrl = "https://eu11.salesforce.com/services/data/v37.0/";

  // This function manages authenticating with Salesforce, providing the access
  // token as a return value
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

    //get access token first. Check if access token exists in cookie session
    var accessToken = cookie.get('sf_access_token');
    if(accessToken == null){
        accessToken = loginSalesForce();
        cookie.put('sf_access_token', accessToken);
    }

    //get list of Opportunities
    var opps = $http({
        method: "GET",
        url: baseUrl + "sobjects/Opportunity",
        headers: {"Authorization": "Bearer " + accessToken}
    });

	return opps;
}
```
**Note:** The above code fetches the access token every time you call the action. You can improve the performance of this call by caching the token into a Backand Server-Side Cookie, and only performing the retrieval when the token has expired.

## Setup access and get client and secret keys
Follow these steps to obtain your Salesforce CRM authentication information:

1. Sign in to Salesforce CRM as a user with Admin rights
2. Open Setup, found under the gear icon
3. Open App Manager
4. Click on 'New Connected App'
5. Provide 'Connected App Name', 'API Name' and 'Contact Email'
6. Check 'Enable OAuth Settings'
  1. Check 'Enable for Device Flow'
  2. Under 'Selected OAuth Scopes' select 'Full Access,' or any other permissions you need
7. Click 'Save'
8. Copy 'Consumer Key' into the client variable in the code
9. Copy 'Consumer Secret' into the secret variable in the code

Once you've obtained the consumer key and the consumer secret, you'll need to enable server-side security in Salesforce. To do so, follow these steps:
1. From App Manager, select your new App and Click 'Manage'
2. Click 'Edit Polices'
3. Change 'IP Relaxation' to 'Relax IP Restriction,' or add Backand's IP to your organization's IP restrictions
4. Change 'Timeout Value' to 24 hours
5. Click 'Save'

With that completed, you should now have full access to your CRM objects using the Salesforce REST API.

##Setup client-side code:

Once you've configured the action to connect to Salesforce, you'll need to call the action from your client-side code. To do so, use the following JavaScript to construct a GET request and trigger your Salesforce action:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: '<your action name>'
    }
});

```
Simply replace 'your object name' with the object that contains your Salesforce custom action, and replace 'your action name' with the name of the action that you provided while creating the integration.

With these changes, you're now able to pull in any and all Salesforce accounts available via their API! You can use a similar pattern to construct additional calls to the Salesforce API - simply replace the URL and parameters in the custom SalesforceIQ action with the URL and parameters for the object you want to retrieve.
