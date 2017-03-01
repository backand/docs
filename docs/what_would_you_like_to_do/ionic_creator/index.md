[Ionic Creator](https://creator.ionic.io) is an online IDE for Ionic apps that can greatly speed the development of your cross-platform mobile and web application. However, like any IDE, it can pose problems when integrating with some external service providers like Backand. Below, we’ll look at the steps needed to get your Ionic Creator app up and running with the Backand SDK, allowing you to implement serverless apps in the Ionic framework with ease.

# Configuring the connection to Backand

To Enable Back with, you’ll need to update your Ionic Creator app’s 'Other JS' section to include a new file name - bkndconfig.js. In this file, replace the entire contents with the following code:

```javascript
angular.module('app.config', [])
// remember to add 'app.config' to your angular modules in Code Settings

.config(function(BackandProvider) {

    BackandProvider.setAppName('BACKAND_APP_NAME');
    BackandProvider.setSignUpToken('BACKAND_API_SIGNUP_TOKEN');
    BackandProvider.setAnonymousToken('BACKAND_ANONYMOUS_ACCESS_TOKEN');

})
```

Once the code has been modified, replace the values above with the appropriate values from your Backand application:

* BACKAND_APP_NAME - This is your app's name in Backand

* BACKAND_API_SIGNUP_TOKEN - This is your app's signup token. It is available in the 'Security & Auth -> Social & Keys' section.

* BACKAND_ANONYMOUS_ACCESS_TOKEN - This is your app's anonymous access token. It is available in 'Security & Auth -> Configuration'.

Once these changes have been made, you'll need to update your application's code settings

# Code Settings

Next, we'll update the app's Code Settings to import the Backand SDK. In 'Code Settings', under the 'External JS' tab, add these two script URLs:

```html
https://cdn.backand.net/vanilla-sdk/1.1.0/backand.js
https://cdn.backand.net/angular1-sdk/1.9.6/backand.provider.js
```

Once you've finished, the  External JS tab will have the following content:


```html
<script src='https://cdn.backand.net/vanilla-sdk/1.1.0/backand.js'></script>
<script src='https://cdn.backand.net/angular1-sdk/1.9.6/backand.provider.js'></script>
<script src='js/directives.js'></script>
<script src='js/services.js'></script>
<script src='js/bkndconfig.js'></script>
```

Next, under 'Angular Modules', add 'backand' and 'app.config'. The end result will have the following content:


```javascript
angular.module('app', [
'ionic',
'app.controllers',
'ionicUIRouter',
'backand',
'app.config',
'app.services',
'app.directives'
])
```

# Working with the Backand provider

Now that the external code has been configured, you can start working with the Backand provider in your service class. For example, you can use the following code to pull rows from an 'items' object in a Backand application:


```javascript
.service('ItemsModel', function(Backand){

    var service = this;
    var objectName = 'items';

    service.all = function () {
        return Backand.object.getList(objectName);
    };

});
```

Next, we'll add this function call into a page for display.

# Updating the Page Controller

To access this data, we'll update the page controller to call our ItemsModel and return the relevant rows. To do so, use the following JavaScript to define a getAll() function and store the results in $scope:

```javascript
$scope.getAll = function() {
    ItemsModel.all()
        .then(function (result) {
            console.log(result.data);
            $scope.data = result.data;
        });
}

$scope.getAll();
```

Now, we'll need to update the page's design to display the new information. To  show a list of all the items you've obtained:

* Drag a 'List Item' element onto your UI

* In the page list pane (upper left hand corner), click on 'List'

* On the right side of the pane, click on 'Angular Directives' and add a new directive with the following details:

    * Directive: ng-repeat

    * Value: object in data

* Finally, click on the 'list-item' element and change the content to object.name (including the curly-brace specializers to get the object's name value, instead of the flat text object.name)

And with that, your Ionic Creator app is now connected to Backand!

# Learning more

At this point, you have the full power of the [Backand SDK](http://docs.backand.com/en/latest/getting_started/vanilla_sdk/index.html) available in your Ionic Creator app. You can use the SDK to add more services to your app, providing CRUD functionality, real-time communications, server-side code execution, and more! Simply head over to [our documentation](http://docs.backand.com) to get started.
