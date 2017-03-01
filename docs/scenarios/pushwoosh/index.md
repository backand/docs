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

3. To integrate in an existing app use the following [Android guide](http://docs.pushwoosh.com/docs/native-android-sdk) – include the SDK.jar and add the relevant code to your application.
  [Make the relevant changes to your AndroidManifest.xml file](http://docs.pushwoosh.com/docs/androidmanifestxml-modifications) – if you're using the starter app just change the App ID and Project ID.
4. To integrate in an existing app use the following [iOS guide](http://docs.pushwoosh.com/docs/apns-configuration)
5. For Ionic you would need to implement the SDK for [Cordova / PhoneGap](http://docs.pushwoosh.com/docs/cordova-phonegap) and check this example for [PhoneGap Build](http://docs.pushwoosh.com/docs/phonegap-build)

## Integrating PushWoosh with Backand 
PushWoosh has an API that can be used to send Push Notifications. You can integrate PushWoosh with Backand by using Backand server-side actions. You can either trigger this action with an object's CRUD event handler, or call it on-demand from your client code.

In the Backand dashboard, open the Actions tab for one of your application's objects, and create a new on-demand server-side JavaScript action by clicking on 'PushWoosh' under 'Push Notifications'.

**You have just created your first push notification action!**

In order to call the function from your angular app, use the following code:

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
