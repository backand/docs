## Upgrading Your Angular 1 application
Follow the steps below to convert your existing Angular 1 Back& application to our new [Serverless SDK](https://github.com/backand/vanilla-sdk), and the associated [Angular 1 SDK](https://github.com/backand/angular1-sdk).

## Step 1: Adding the new SDK to your package manager
To install the Angular 1 SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/angular1-sdk |
| yarn | $ yarn add @backand/angular1-sdk |
| bower | $ bower install backand-angular1-sdk |
| clone/download via Git | $ git clone $ git clone https://github.com/backand/angular1-sdk.git |

As a part of this, you'll also want to remove any of the previous SDK includes from the *vendor* directory (or wherever your package manager stores JavaScript dependencies). Keeping these older versions around can pose problems when sending HTTP requests.

## Step 2: Include the SDK in your project

The Serverless SDK requires two includes for Angular 1 apps. The first is our Vanilla SDK, and contains all of our core SDK functionality. The second is our Angular 1 SDK, which wraps the Vanilla SDK and provides setup and access to its functionality. Start with removing the old SDK by commenting out - or deleting - the following line (or related lines, based on your project structure):

```html
     <script src="lib/angularbknd-sdk/dist/backand.min.js"></script>
```
Then, replace it with the new SDK links:

```html
<script src="lib/backand-vanilla-sdk/dist/backand.js"></script>
<script src="lib/backand-angular1-sdk/dist/backand.provider.js"></script>
```

You can also use our SDK directly via our Content Distribution Network. Include the CDN links as follows, instead of the local *\lib* links specified above:

```html
<script src="//cdn.backand.net/vanilla-sdk/1.0.9/backand.js"></script>
<script src="//cdn.backand.net/angular1-sdk/1.9.5/backand.provider.js"></script>
```

## Step 3: Authentication

The authentication functionality in the SDK has not changed considerably with the new release. Here are a few notes on authentication elements that may be important to your application:

* If you have previously disabled anonymous auth in your app, you can re-enable it using *Backand.useAnonymousAuth()*. This value defaults to true, so if you do not modify the default value you do not need to make any changes.
* We’ve added new social media authentication functions:  *socialSignin*, and *socialSignup*. Please note that while some of the examples in our github repos - and some of our documentation - used *socialSignUp*  to refer to the specified function in the Backand SDK, this is no longer valid - the "up" will not be capitalized in the new SDK.

## Step 4: Update unauthorized user detection

The *getToken()* method has been expanded, to use a promise. Originally this method would return undefined when a user was unauthorized, but this functionality can now be managed via the promise method. In the new SDK, the *getToken()* method is not as prominent as it was in previous versions, and you are likely to not need it as you work on your app.

## Step 5: Use the new SDK methods in place of $http calls

The Serverless SDK features wrapper methods that can be used in place of the direct HTTP calls used in the previous method. Here's a quick comparison of the old SDK's HTTP communications, as compared to the new function-based Serverless SDK:


| **Old Method** | **New Method** |
| -------------- | -------------- |
| $http.get(getUrl()); | Backand.object.getList(objectName); |
| $http.get(getUrlForId(id)); | Backand.object.getOne(objectName, id); |
| $http.post(getUrl(), object); | Backand.object.create(objectName, object); |
| $http.put(getUrlForId(id), object); | Backand.object.update(objectName, object); |
| $http.delete(getUrlForId(id)); | Backand.object.remove(objectName, id); |
| URL built with Backand.getApiUrl() | No URL necessary |

## Step 6: Update handling of the response object

The old SDK would use code similar to the following when handling responses from Back& API calls:

```javascript
function getAll() {
 ItemsModel.all()
   .then(function (result) {
     vm.data = result.data.data;
   });
}
```

The new sdk does not store the results in a nested data member, but rather in a root data element. The old SDK stored the response contents in a root property, meaning that the actual data was a subset of this *data* property - hence the use of *result.data.data* in the old SDK. With the new SDK, you no longer need the extra level of disambiguation, and can store the data in your application's object with the following code:

```javascript
     vm.data = result.data;
```

## Conclusion
In this post, we covered the new SDK. We touched briefly on the features the new SDK has to offer, and walked through the process necessary to convert an existing Backand-powered app to the new API. Most importantly, though, is that we built this SDK based on your feedback, and we want more! Connect with us via social media, or on StackOverflow, or even directly via comments on our Git repos and contacting customer support - we’d love to incorporate your thoughts and suggestions into the next version.
