## Push Notification Integration in Android with Netmera
Netmera is a cloud based service that can be used to send Push Notifications to various platforms, among other services such as Exception reporting. It offers a friendly site where campaigns ()push notifications) can be managed and customized and a REST API to send push notifications automatically. In this guide you can find out how to get started on Netmera and send push notifications with Backand.
## Get Started with Netmera
1. Register to Netmera
2. Download the Netmera SDK from [here](https://netmera.readme.io/docs/android-sdk-download) or their [starter app](https://cp.netmera.com/nm/admin/sdkDownload/overview/android/final?isNewProject=true).
3. Check out their guide to [set up your app/the starter app with the Netmera SDK](https://netmera.readme.io/docs/android-guide)
4. Follow this [Android guide](https://netmera.readme.io/docs/netmera-push-notification-android) and [iOS guid](https://netmera.readme.io/docs/netmera-push-notification-ios) to configure push notifications on the app and get the key from Google Cloud Messaging. If you use the starter app you only need the first 3 steps to get started (unless you want to trigger advanced features like tags).

## Integrating Netmera with Backand
Netmera has a rest API that can be used to remotely send push notifications. You can integrate Netmera with Backand by using Backand server-side actions. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code. The following example demonstrates the on-demand option. In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action. Learn more how to create actions [here](http://docs.backand.com/en/latest/apidocs/customactions/index.html). Name the action SendPushNotification, add notificationTitle and notificationContent to the input parameters and paste the following code:

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
    // Netmera REST API key. Get it from https://cp.netmera.com/backand-test/admin/overview when logged in
    var NETMERA_API_KEY = 'IERqDFg3tdgGDFZ8F0KF546FHg4564O7YAUhpzqMqlNCTNcQ';

    // Notification title for logging purposes (does not appear on the notification itself)
    var notificationTitle = parameters.notificationTitle;
    // Notification message
    var notificationContent = parameters.notificationContent;
    var basicUrl = 'https://api.netmera.com/push/1.1/';
    var action = 'notification';

    return $http({
        method: "POST",
        url: basicUrl + action,
        data: {
            "title": notificationTitle,
            "notificationMsg": notificationContent
        },
        headers: {
            "X-netmera-api-key": NETMERA_API_KEY,
            "Content-Type": "application/json"
        }
    });

    // Netmera has a more advanced API that supports filters by platforms or by tags
    // You can define tags on the Netmera CP
	// Learn more at: https://netmera.readme.io/docs/push-api-basics
    
    // var basicUrl = 'https://api.netmera.com/push/1.2/';

    // return $http({
    //     method: "POST",
    //     url: basicUrl + action,
    //     data: {
    //         target: {
    //             "platforms": ["ANDROID", "IOS"],
    //             "tags": ["tag1", "tag2"]
    //         },
    //         notification: {
    //             "title": "My First Push Notification",
    //             "notificationMsg": "My First Push Notification message"
    //         }
    //     },
    //     headers: {
    //         "X-netmera-api-key": NETMERA_API_KEY,
    //         "Content-Type": "application/json"
    //     }
    // });
}
```
You have just created your first push notification action! In order to call the function from your angular app, use the following code:

```javascript
return $http ({
  url: Backand.getApiUrl() + '/1/objects/action/<your object name>',
    params: {
      name: 'SendPushNotification',
      parameters: {
        notificationTitle: 'My First Push Notification',
		notificationContent: 'Hello! This is my first notification!'
      }
    }
});
```
Replace ‘your object name’ with the object associated with the action you created, set a title for the 'notificationTitle' field, write any message you want in ‘notificationContent’ field and you’re good to go. Now you can start sending push notifications dynamically using Backand and Netmera.
