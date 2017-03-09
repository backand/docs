Building and maintaining customer relationships is crucial for driving sales to your platform. However, it can also be a complex process requiring integrating data from multiple sources and, more importantly, ensuring that data is accessible when it is needed. Customer Relationship Management (CRM) software is designed to make this data management and integration process much easier, providing you with the tools you need to drive customers through your sales funnel. In this article, we'll look at integrating a Backand application with Salesforce IQ, providing you with all of the tools you need to effectively leverage your customer data in your Backand application.

## What is SalesforceIQ?
[SalesforceIQ](https://www.salesforceiq.com/) is an out-of-the-box CRM solution that quickly gives you access to dynamic information tied into a full CRM solution. With Automatic Data Capture and enterprise-level intelligence under the hood, SalesforceIQ acts like your own personal assistant so you can focus on what matters most: selling. SalesforceIQ, in addition to providing easy integrations with tools like Google and Microsoft Exchange, also gives you the capability to dynamically access and manage your data through a series of robust APIs.

## Connecting a Backand application with SalesforceIQ
Integrating Backand and SalesforceIQ is as simple as leveraging the full-featured [API](https://api.salesforceiq.com/) provided by Salesforce from within your Backand application. While traditionally you'd need to make these calls to the API from a server, you can achieve the same functionaltiy in Backand by using our custom template action. This action provides an easy-to-use code template for making calls to the SalesforceIQ API, letting you update your user tracking data based upon user activity in your Backand app, or even update your Backand app's UI based upon what you know about your user in SalesforceIQ.

Working with the [SalesforceIQ API](https://api.salesforceiq.com/) provides you with full capabilities to create, retrieve, and update data on your users, as well as manage their infromation in SalesforceIQ. Communicating with their API is as simple as translating the cURL commands provided by Salesforce in their documentation into the appropriate $http calls that you can make from JavaScript. Simply provide the required authentication and identification headers, construct the URL, make the call, and handle the response when it arrives, dispatching it either directly to your application via a synchronous function call return, or emitting the data as an event using our Socket-based real-time communications functionality.

## The SalesforceIQ Action Template
We have created an action template that will give you jump start with salesforceIQ. You can either trigger this action from an object's database transaction event actions, or create a new on-demand action that you can call from your app's client code. The following JavaScript is provided by the template action, which is available as "SalesforceIQ" in the "CRM & ERP" section of action templates.:

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


This code provides you with all of the basic tools you need to get connected to the SalesforceIQ API. It takes in your SalesforceIQ API Key and API Secret, and performs a call to the "/accounts" endpoint to fetch accounts.

## Connecting this action to your SalesforceIQ account
To connect to SalesforceIQ, you'll first need to register for an account if you haven't done so. Once you've signed up, follow these steps to obtain your API Key and API Secret:

1. Open Settings under the gear icon
2. Open the 'Integration' tab under 'My Account Settings'
3. Under 'Create New Custom Integration,' click 'Custom'
4. Set the name to 'Backand API,' and provide a description
5. Copy the 'API Key' and 'API Secret' from the integration page into the JavaScript Action code
6. Click 'Save'

When this is completed, you should now be able to access all of your SalesforceIQ accounts from the custom JavaScript action.


##Setup client-side code:

Once you've configured the action to connect to SalesforceIQ, you'll need to call the action from your client-side code. To do so, use the following JavaScript to construct a GET request and trigger your SalesforceIQ action:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: '<your action name>'
    }
});

```
Simply replace 'your object name' with the object that contains your SalesforceIQ custom action, and replace 'your action name' with the name of the action that you provided while creating the integration.

With these changes, you're now able to pull in any and all SalesforceIQ accounts available via their API! You can use a similar pattern to construct additional calls to the SalesforceIQ API - simply replace the URL and parameters in the custom SalesforceIQ action with the URL and parameters for the object you want to retrieve.
