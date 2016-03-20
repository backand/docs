## Push Notification Integration in Android with PushWoosh
Pushwoosh is a cloud based service that can be used to send Push Notifications to various platforms. It offers a friendly site where push notifications can be sent and customized and an API to send push notifications automatically. In this guide you can find out how to get started on PushWoosh with an example app for Android and send push notifications with Backand.
## Get Started with PushWoosh

1. Register an account in PushWoosh and proceed to create an application. Using the PushWoosh control panel, configure your application to support Android including [configuring GCM (Google Cloud Messaging](http://docs.pushwoosh.com/docs/gcm-configuration))
2. You can either get the starter app or to integrate in an existing app. 
To get a starter app: clone the PushWoosh Android SDK:
 ```
  git clone https://github.com/Pushwoosh/pushwoosh-android-sdk.git 
 ```
 open /Samples/Android-Simple directory in Android Studio and build the app.

3. To integrate in an existing app [use the following guide](http://docs.pushwoosh.com/docs/native-android-sdk) – include the SDK.jar and add the relevant code to your application.
  [Make the relevant changes to your AndroidManifest.xml file](http://docs.pushwoosh.com/docs/androidmanifestxml-modifications) – if you're using the starter app just change the App ID and Project ID.

## Integrating PushWoosh with Backand 
PushWoosh has an API that can be used to send Push Notifications. You can integrate PushWoosh with Backand by using Backand server-side actions. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action SendPushNotification, add notificationContent to the input parameters and paste the following code:
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
    // Get the code from the application section in the PushWoosh control panel - https://cp.pushwoosh.com/v2/applications
    var APPLICATION_CODE = '3F0N8-E375C';
    // Get the auth token from the API Access tab in the PushWoosh control panel - https://cp.pushwoosh.com/v2/api_access
    var AUTH_TOKEN = 'ANfYkeDdjt4H3TGRkdsgdAomDSDNsolHbUXI8tnkDidIc4SBkz9ASDwQXNnJb3HGJyR2TqlpHDiOIhFntKqq';

    var notificationContent = parameters.notificationContent;
    var basicUrl = 'https://cp.pushwoosh.com/json/1.3/';
    var action = 'createMessage';

    return $http({
        method: "POST",
        url: basicUrl + action,
        data: {
            "request": {
                "application": APPLICATION_CODE,
                "auth": AUTH_TOKEN,
                "notifications": [
                    {
                        "send_date": "now",
                        "ignore_user_timezone": true,
                        "content": notificationMessage
                    }
                ]
            }
        },
        headers: {
            "Accept": "application/json",
            "Content-Type": "application/json"
        }
    });

    // It's also possible to send a targeted message using a filter
    // In the following example we're sending a targeted message only to Android and iOS devices
    // See more filter possibilities at http://docs.pushwoosh.com/docs/createtargetedmessage

    //var action = 'createTargetedMessage';
    //
    //return $http({
    //    method: "POST",
    //    url: basicUrl + action,
    //    data: {
    //        "request": {
    //            "auth": AUTH_TOKEN,
    //            "send_date": "now",
    //            "content": notificationMessage,
    //            "devices_filter": "A(\"" + APPLICATION_CODE + "\", [\"Android\", \"iOS\"])"
    //        }
    //    },
    //    headers: {
    //        "Accept": "application/json",
    //        "Content-Type": "application/json"
    //    }
    //});

}
```
You have just created your first push notification action! In order to call the function from your angular app, use the following code:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: 'SendPushNotification',
      parameters: {
        notificationContent: 'Hello! This is a push notification!',
      }
    }
});
```
Replace ‘your object name’ with the object associated with the action you created and write any message you want in ‘notificationContent’ field and you’re good to go. Now you can start sending push notifications dynamically using Backand.
