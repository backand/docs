#Sending Analitics data

There are plenty of web analytics tools out there which helps your marketing team understand where and how your product is going, segment.io allows you to implement the analytics API once and sends it to almost any tool out there depending on you choice

usually you would implement it on you client side , but in cases where you want data from your server side to be send to the analytic tool you would do it on a server side action on backand
# Server-side Action

We'll begin the application by adding a custom server-side action. This action will execute JavaScript code that send user identification data to segment and from there to Woopra or Intercom,


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

The above code accepts 2 parameters userId and activeApp where the userId is the user email.
you can also send Segment.io, data collected from Backand query detailing users information
# Client-side Integration

there is no client side codding
