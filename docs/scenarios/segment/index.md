# Sending Analytics data

There are a large number of web analytics tools available that can help your marketing team understand both how many users are using your product, and what they are doing while they use it. Implementing these services in your Backand application is as simple as any other third-party API integration. Segment.io allows you to implement an analytics API and send it to almost any notification tool available, depending on your infrastructure needs. Normally this type of integration would be done on the client side, but there are some instances where a server-side integration is useful. In this example, we will look at implementing a Segment.io integration with your Backand application using a custom server-side action.

# Server-side Action

We'll start by adding a new custom server-side action.  This action will execute JavaScript code that sends user identification data to segment.io (and, from there, to Woopra, or Intercom, or any other interested service):

```javascript
* globals
 $http - Service for AJAX calls
 CONSTS - CONSTS.apiUrl for Backands API URL
 */
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
  // write your code here

  var writeKey = 'YOUR_SEGMENT_IO_WRITE_KEY';
  var authorization = 'Basic ' + btoa(writeKey);
  var segmentUrl = 'https://api.segment.io/v1/identify';

  var segmontPostResponse = $http(
    {
      method: 'POST',
      url: 'https://api.segment.io/v1/identify',
      data: {
        "userId": parameters.userId,
        "traits": {
          "email": parameters.userId,
          "ActiveApps": parameters.activeApp
        },


        "integrations": {
          "All": false,
          "Woopra": true,
          "Intercome":true
        }
      },
      headers: {'Authorization': authorization, 'Accept': 'application/json', 'Content-Type': 'application/json'}

    });

  return segmontPostResponse;
}
```

The above code takes two parameters - userId and activeApp, with the userId being the user's email address. You can also send Segment.io data collected from Backand using a query to obtain the data you have stored on a given user.

# Client-side Integration

This code has no client-side component.
