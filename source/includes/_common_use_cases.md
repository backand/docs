# Common Use cases
Below are some common use cases for Backand applications

## Convert an older app to our serverless SDK
Follow the steps below to convert your existing Angular 1 Back& application to our new [Serverless SDK](https://github.com/backand/vanilla-sdk), and the associated [Angular 1 SDK](https://github.com/backand/angular1-sdk).

### Step 1: Adding the new SDK to your package manager
To install the Angular 1 SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/angular1-sdk |
| yarn | $ yarn add @backand/angular1-sdk |
| bower | $ bower install backand-angular1-sdk |
| clone/download via Git | $ git clone $ git clone https://github.com/backand/angular1-sdk.git |

As a part of this, you'll also want to remove any of the previous SDK includes from the *vendor* directory (or wherever your package manager stores JavaScript dependencies). Keeping these older versions around can pose problems when sending HTTP requests.

### Step 2: Include the SDK in your project

The Serverless SDK requires two includes for Angular 1 apps. The first is our Vanilla SDK, and contains all of our core SDK functionality. The second is our Angular 1 SDK, which wraps the Vanilla SDK and provides setup and access to its functionality. Start with removing the old SDK by commenting out - or deleting - the following line (or related lines, based on your project structure):

```html
     <script src="lib/angularbknd-sdk/dist/backand.min.js"></script>
```
Then, replace it with the new SDK links:

```html
<script src="lib/backand-vanilla-sdk/dist/backand.js"></script>
<script src="lib/backand-angular1-sdk/dist/backand.provider.js"></script>
```

You can also use our SDK directly via our Content Distribution Network. Include the CDN links as follows, instead of the local *\\lib* links specified above:

```html
<script src="//cdn.backand.net/vanilla-sdk/1.0.9/backand.js"></script>
<script src="//cdn.backand.net/angular1-sdk/1.9.5/backand.provider.js"></script>
```

### Step 3: Authentication

The authentication functionality in the SDK has not changed considerably with the new release. Here are a few notes on authentication elements that may be important to your application:

* If you have previously disabled anonymous auth in your app, you can re-enable it using *Backand.useAnonymousAuth()*. This value defaults to true, so if you do not modify the default value you do not need to make any changes.
* We’ve added new social media authentication functions:  *socialSignin*, and *socialSignup*. Please note that while some of the examples in our github repos - and some of our documentation - used *socialSignUp*  to refer to the specified function in the Backand SDK, this is no longer valid - the "up" will not be capitalized in the new SDK.

### Step 4: Update unauthorized user detection

The *getToken()* method has been expanded, to use a promise. Originally this method would return undefined when a user was unauthorized, but this functionality can now be managed via the promise method. In the new SDK, the *getToken()* method is not as prominent as it was in previous versions, and you are likely to not need it as you work on your app.

### Step 5: Use the new SDK methods in place of $http calls

The Serverless SDK features wrapper methods that can be used in place of the direct HTTP calls used in the previous method. Here's a quick comparison of the old SDK's HTTP communications, as compared to the new function-based Serverless SDK:


| **Old Method** | **New Method** |
| -------------- | -------------- |
| $http.get(getUrl()); | Backand.object.getList(objectName); |
| $http.get(getUrlForId(id)); | Backand.object.getOne(objectName, id); |
| $http.post(getUrl(), object); | Backand.object.create(objectName, object); |
| $http.put(getUrlForId(id), object); | Backand.object.update(objectName, object); |
| $http.delete(getUrlForId(id)); | Backand.object.remove(objectName, id); |
| URL built with Backand.getApiUrl() | No URL necessary |

### Step 6: Update handling of the response object

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

### Conclusion
In this post, we covered the new SDK. We touched briefly on the features the new SDK has to offer, and walked through the process necessary to convert an existing Backand-powered app to the new API. Most importantly, though, is that we built this SDK based on your feedback, and we want more! Connect with us via social media, or on StackOverflow, or even directly via comments on our Git repos and contacting customer support - we’d love to incorporate your thoughts and suggestions into the next version.

## Setting up and Configuring a database
Backand users typically fall into one of two categories. The first category works with the underlying data system as a series of objects, and doesn't spend a lot of time focused on the database underpinning the application. The second category, though, work with databases as a matter of course. Often they have existing databases that they want to build an interface on top of, with complex existing relationships that need to be modeled accurately by any framework put in front of it. Below we'll cover the database connectivity functionality offered by Backand, and how it can fit even the most complex of database needs.

### Bringing Your Own Database

Most back-end-as-a-service providers want you to work within their clearly-defined data roles. They often require you to port your data to their format, or provide only highly static interfaces to interact with and consume existing data. At Backand, we realize that the database is often the core of your business, and you need to be able to work with your data with minimal effort possible. That's why we have built a large suite of functionality around importing – and managing your existing SQL database. Backand can analyze your database structure and build a scaffold around your existing data quickly, allowing your application developers to very rapidly begin working with your existing database structure.

### Importing a Database

While you can create a new database from scratch using our JSON object modeling interface, the true power of Backand is shown when we integrate seamlessly with your existing database. When you connect your Backand application to an existing database on an external server, Backand quickly analyzes your database structure and constructs a meaningful interface on top of your data. This covers everything from basic table structure all the way up to complex foreign-key relationships and many-to-many mapping tables. Backand then uses this data to construct an API on top of your database, allowing you to interact with your data in a RESTful manner very quickly and easily.

### Staying In Sync

While an import process is beneficial, it quickly loses its ease if it does not accurately track changes. Many vendors expect your database schema to remain static, and want to work only with your data – requiring a re-sync whenever the database structure changes. With Backand, the need to re-synchronize is completely removed from the process. Backand automatically detects changes to your underlying database and updates its representation of your data, removing a needless tertiary step in the process to ensure that your application remains synchronized with your database's structure. Furthermore, data changes are synced back to the database server nearly instantaneously, ensuring that no matter how you fetch your data it will always be up-to-date.

### Summary

If you already have a database that you're looking to build into an application, your options for outsourcing the related back end can be limited. With Backand, however, you can bring your own database and import its structure directly. When you link your Backand application with your existing database you always have up-to-date access on both your database's schema and the data itself. By not having to perform awkward data transformations, or re-synchronize every time you make a change to the schema, Backand allows you to focus on the core of your application, keeping your database up-to-date no matter what.

## Using the Back& REST API
Once your web application's data has been imported and configured, the next step is working with your data. Backand, using your underlying database structure as a key, builds a RESTful API in front of your database, allowing for full programmatic access to each of the HTTP verbs that you would want to use to interact with your server objects. Below we'll cover the concepts behind REST, and how Backand makes creating and testing a RESTful interface simple.

### What is REST?

REST stands for Representational (RE) State (S) Transfer (T). It is a software architectural style that is designed for the creation of (and interaction with) scalable web services. Most RESTful web services – including Backand's implementation – use HTTP verbs such as GET, POST, PUT, and DELETE, to perform specific actions on the server. The end result is a stateless representation of object interaction, providing a clearly-defined interface to an underlying set of objects in an application. This allows for communication with the server to occur as a series of object changes, reducing the complex server logic needed with other, more stateful architectures.

### Automatically Building an API

When you import your database into your application, Backand analyzes your database's underlying schema. It uses the database schema itself, along with metadata associated with each of your database's tables and columns, to build a RESTful API that accurately represents the underlying data structure. Not only does it give you API endpoints for each of the common object actions (such as CREATE, RETRIEVE, UPDATE, and DELETE), but it also uses your database's metadata to fully represent the potential ranges of your application's objects, from representing foreign key relationships all the way down to allowing minimum and maximum values on a specific column in a table. It also keeps track of the nullability of each column, marking non-null fields as required parameters in all related API endpoints.

By automatically providing these endpoints for your database, Backand provides a scalable and understandable API based on web communication standards that would normally take months to develop, and does so within moments of attaching your database. The endpoints follow all defined security roles, and respond with well-formed JSON that pulls in as much – or as little – of your object as you need.

### The REST API Playground

While a RESTful API is useful, it can also be opaque while the application is being built around it. To get past this nebulous initial state, Backand offers the REST API Playground. The REST API Playground is a dynamic web page built using [Swagger 2.0](http://swagger.io/), an automated front-end for well-defined JSON web APIs. It provides information on each of the endpoints defined in your application, and gives you complete access to test these endpoints in an easy-to-use web-based interface. Through this interface you can see every parameter involved in each API endpoint in your application, and quickly test the endpoints using the supplied parameter prompts and descriptions. This tool can greatly increase development speed, as interested parties can simply use the REST API Playground to build out their JSON web requests prior to incorporating them into the application logic.

### Actions and Queries

Each API endpoint created by Backand also fully integrates with the Action system. You can associate actions with each of the API calls, which simply map to the underlying database actions. POST calls trigger database CREATE actions, PUT calls trigger UPDATE actions, DELETE calls trigger DELETE actions, and so on. Furthermore, the endpoints of each of your custom actions are provided in a helpful "Actions" section, allowing you to test each custom action independently, and with the same useful interface. Finally, endpoints for custom queries that you define in your application are also delineated in the API Playground, allowing for quick iteration on the underlying queries and greatly easing the troubleshooting process.

### Summary

Interacting with the data in your application is something that is crucial to the success of your product. Through Backand's automatic REST scaffolding, you can quickly and easily get up and running with a scalable web service that can provide developers with access to all of the potential modifications they might need to make to your data. With access to both custom actions and queries, as well as the predefined database triggers you create in the dashboard, Backand's RESTful API offers you the full functionality of a scalable web service – all with the click of a button.


## Configuring Role-based Security
One of the core concerns of any application that deals with sensitive user and business data is security. In this article we'll cover how Backand handles security for your application, covering the entire stack – from user registration to security templates. We'll also discuss anonymous access to your application, which can greatly enhance your application's adoption by the world at large. Backand takes your application's security seriously, and with the following we'll show you how we protect your data effectively.

### User Registration

The security process starts with user registration. Backand uses two-factor authentication by default – requiring you to first invite users to your application, then directing them to a registration page that you configure. Backand takes care of the communication, handling the verification (configurable) and registration processes from your customizable sign-up page. This entire process is known as "private" mode, which is the default for all new applications – it requires that users are invited to join the app.

Once your app is stable and your security is fully configured (or even before if you're confident in your data), you can open your application up to the public. This allows users to sign up to your application directly, without having to be invited. Once they register, Backand sends the user a verification email to ensure that the sign-up is legitimate, if the sign-up email verification setting is enabled.. Once the new user confirms the registration, Backand redirects them to a custom verified email link, representing their successful registration with the system. The user is assigned a default role (see below), and is free to use your app within the permissions framework that you constructed.

### Role-based Security

Backand uses role-based security to secure access to your application data. This is accomplished by assigning each user in your application a role. Roles have varying permission levels throughout your app, from object-specific restrictions on record modification all the way up to full access to your application's configuration. Each role's permission set is configurable at a macro level as well as at an object level, allowing you true flexibility in securing your application.

New users created from the admin dashboard are created in the "admin" role. One of your first steps should be adding an additional security role for new users – whether invited in private mode or registering organically in public mode – that will serve as the default for user sign-ups. Once this default role is set, all new users are assigned the new role automatically. You can then make any permission or access changes you need to from the administrative dashboard.

### Security Templates

Setting a security level for every single object and user class in your database has the potential to be a tedious, error-prone process. Luckily Backand provides a solution through Security Templates. Security Templates define a default set of security permissions for all of the roles in your application that can then be applied to each of the Create, Read, Update, and Delete actions in your database. Templates can be assigned at both the object and query level, allowing you flexibility in configuring user access to your data.

Sometimes, though, a blanket approach isn't sufficient for the final functionality of your application. In those cases, Backand allows you to override security templates at a per-role level. This ability gives you more granular control over your application's data, allowing you to grant or revoke access to sensitive components as necessary. Through use of Security Templates and Roles, you will be able to fully secure your application and ensure only authorized users can perform actions against your data.

### Anonymous Access

In many cases, particularly when working with your application as an API, it is beneficial to grant anonymous access to a specific user. This user can be someone arriving at your site organically, or can be another web service hitting your application's RESTful endpoints. To accommodate these cases, Backand provides anonymous access. This uses token-based authentication to ensure that the anonymous use case is valid and that the user in question is allowed to interact with your app. Furthermore, you can restrict anonymous access to your application based on any of the underlying user roles, ensuring that anonymous uses can only access the elements that you want them to.

### Summary

Securing your application is a priority task, and crucial to ensuring your application's success. Through Backand's use of two-factor authentication to drive registration, you can be sure that the users coming into your application are a low fraud risk, and that both their experience – and your data – are protected. Additionally, you can secure your app through the use of user roles and security templates, which allow you to quickly and easily manage the permissions in your application. Finally, you can grant some use cases anonymous access to your application, easing integration with third parties and handling organic traffic with ease. With Backand's security offerings, you can rest assured that your application is safe and protected.




## Integrating with Third Party APIs
Libraries have built the modern world. In a post originally on Google Plus, Steve Yegge detailed how it was [platform development](https://gist.github.com/chitchcock/1281611) that provided the basis for success of a lot of the major technology companies, using Amazon, Microsoft, and Facebook as examples. True platform-based development isn't possible without a mechanism to integrate with those platforms, though. Below we'll look at the options available to you when developing a Backand-backed web app, detailing which APIs work best at each stage of the application.

### Guidelines on Integrations
As a general rule, you'll see two overall ideas put forth in this article:

1. The client side of your application can integrate with any library that is available on the client side. Most commonly, this will consist of JavaScript libraries.
1. The server side of your application can integrate with any web service that bases its communication off of HTTP web requests.

Most modern libraries you are likely to consume will fit into one of these two categories, so integration usually shouldn't be a concern. However, some functionality will not be available to you if it relies upon complex server libraries or non-HTTP integrations, such as SOAP web services. As such, it is critically important that you plan your library usage thoroughly, and understand the requirements and auxiliary dependencies of each vendor you choose to integrate with. You can see a few examples of this in action on Backand's GitHub account:

* [This project](https://github.com/backand/stripe-example) shows how to use Backand perform a basic integration with Stripe.
* Backand's [Noterious app](https://github.com/backand/simple-noterious-app), a note board application backed by Backand's SDK, demonstrates using Backand to integrate with Amazon Web Services within a larger application.

### Client-side Integrations

As mentioned above, any library available to a client-side web app should be available to your AngularJS app's client-side code. Simply include the required library along with your other dependencies, and you should be able to use the library just as you would in any other application. One potential issue to be faced, though, is that of callbacks. Due to Backand providing the server-side integration for your web app, you will want to create On-Demand Custom Actions to respond to any callbacks sent from the third party you are consuming. Simply create the endpoint, specify the expected parameters, and implement the processing for the delayed notification within the JavaScript action, then provide the URL for that action as your application's callback URL. This approach allows your app to scale more quickly, as the processing load is handled primarily by Backand as opposed to your own servers.

### Server-side Integrations

Server-side integrations of third party libraries are restricted to those libraries that communicate via HTTP requests. This can mean a RESTful API, or just a series of simple HTTP web services that accept JSON parameters. Any library that can integrate natively with JavaScript code can be implemented in this manner. Take, for example, an integration with [Stripe's REST API](https://stripe.com/docs/api). While Stripe helpfully provides encapsulating libraries in a number of languages, all of their functionality is available via simple web requests which can be accessed via CURL. These endpoints are ideal for server-side actions, which can use JavaScript's built in AJAX communication functionality to hit the same endpoints. Simply wrap each endpoint in a custom server-side action, then modify your client-side code to call your action wrappers. In this way, you can integrate with any HTTP API.

While many of these libraries have code that can exist in either the client or the server, the approach above is ideal when security is a consideration. If you're dealing with a customer's sensitive personal info, a client side-based implementation may expose your users to unnecessary risk. By moving these calls to server-side custom actions, you can protect your users' information by encrypting the communications with HTTPS web requests (already provided by Backand), and put the sensitive code behind your application's administrative dashboard.









## Working with Geographic Data
Backand lets you store geography points as fields as part of any object. Geography point is an array of latitude and longitude which represents a location on a map.

With Backand geography query you can easily find distance between points or objects that match a distance to a specific location.

### Add a Point

To add new point field, open Backand Model, select add field with *point* type. To POST new data point for latitude 10 and longitude 20 just send the following array:

```json
  "geo": [10,20]
```

### Query Point

To query a distance within a geography point you can use any of the follow commands:
`$withinMiles`
`$withinFeet`
`$withinKilometers`
`$within (in meters)`

To get all the restaurants within 25 Miles of San Francisco Marina District [37.8019859, -122.4414805]:

```json
  {
    "object": "restaurants",
    "q": {
      "location" : { "$withinMiles" : [[37.8019859, -122.4414805], 25] }
    }
  }
```

The above noSql is translated into MySQL syntax using the new ST_Distance() function:

```SQL

  SELECT * FROM `restaurants` WHERE (ST_Distance ( `restaurants`.`location`, ST_GeomFromText('POINT( 37.8019859 -122.4414805 )') ) <= 25 /(69))

```

### Query Point with Parameters

In order to query get geography points dynamically, use [Query](http://docs.backand
.com/en/latest/getting_started/queries/index.html) with parameters:

1. Add this *Input Parameters*: lan, lon, dist
2. In the Query use tokens ("&#123;&#123;lan}}", "&#123;&#123;lon}}" and "&#123;&#123;dist}}") to represent the input parameters:
3.

  {
    "object": "restaurants",
    "q": {
      "location" : { "$withinMiles" : [["&#123;&#123;lan}}", "&#123;&#123;lon}}"], "&#123;&#123;dist}}"] }
    }
  }


### Filter Point

Using Backand noSql filter you can filter object based on a distance to a specific point using $within command.

Filter all the restaurants within 25 Miles of San Francisco Marina District [37.8019859, -122.4414805]:

```javascript

  return $http ({
    method: 'GET',
    url: Backand.getApiUrl() + '/1/objects/restaurants',
    params: {
      filter: {
        "q": {
          "location" : { "$withinMiles" : [[37.8019859, -122.4414805], 25]}
        }
      }
    }
  });

```

## Bulk Operations
```shell
curl -X POST -d "JSON String of bulk operations" https://api.backand.com/1/bulk
```

```javascript
backand.bulk.general(data)
  .then(function(data) {
    console.log(data);
  });
```
> Data is an array of operation objects, which are constructed with the general format below:

```json
{
  "method": "POST",
  "url": object_path,
  "data": {
    "fieldname": "value"
  },
  "parameters": {
    "param1": "value 1"
  },
  "headers": {
    "Authorization": "Auth header..."
  }
}
```


> The key element of this operation is the "headers" parameter. This represents the HTTP headers used to perform the request, and are used to uniquely identify your application as each request as made.

```json
  headers:{
    "Authorization": "Bearer **YOUR_ACCESS_TOKEN**",
    "AppName": "**YOUR_APP_NAME**"
  }
```
>The value for `YOUR_ACCESS_TOKEN` is obtained via a call to `https://api.backand.com/token` - simply replace `YOUR_ACCESS_TOKEN` with the bearer token returned by this endpoint, and `YOUR_APP_NAME` with your application's name that was specified during app creation. Each action hash can accept a different header parameter, allowing multiple operations to be undertaken by multiple users as a part of the same bulk request. If the 'headers' parameter is not included, the headers used for the call to the bulk operations endpoint are used for the relevant action instead.

Backand's bulk operations endpoint can be used to operate on multiple items in a single request. The request has the following general form:

The above represents an array of parameter hashes. Each parameter hash represents a different action to be performed as a part of the bulk operaation. The parameter hashes are broken down as follows:

| Field name | Possible Values | Required? | Description |
| ----- | ----------- |------|-----|
| method | POST/ PUT/ DELETE | yes | The HTTP verb to be used |
| url | url for the object | yes | This is the URL used to access the object being modified in bulk |
| data| JSON | yes (POST and PUT only) | This only applies to POST and PUT actions. It represents the data to be created (for POST requests), or the updates to existing data (for PUT requests). Provided as a series of key-value pairs |
| parameters | JSON | no | additional parameters, and values for those parameters, to send with the request |
| headers | JSON |no| This is used to set custom HTTP headers. You can use it to dynamically set the Authorization header for each REST call. If no value is provided, the generic bulk call authorization headers are used. |

Requests to perform bulk actions are sent as HTTP POST requests to the bulk operations URL: `Bulk operations URL: https://api.backand.com/1/bulk`. You can also use [the SDK](#bulk) to send your operations list to your app.

<aside class="notice">Backand's Bulk Endpoint accommodates up to 1,000 operations in a single request. Operations in excess of 1,000 will need to be sent in a separate request </aside>

### Constructing Bulk Requests
> Performing each of the standard non-retrieval CRUD operations in bulk is as simple as specifying the appropriate actions to take with each request. We start by constructing the URL we need in order to manipulate each object. This is done with the Backand SDK using the following code:

```javascript--persistent
// Backand is the Backand service from the SDK
var object_path = Backand.defaults.apiUrl
                  + Backand.constants.URLS.objects
                  + "/" + "YOUR_OBJECT_NAME";
```
> Simply replace "YOUR_OBJECT_NAME" with the name of the object you wish to manipulate. Once you have the object URL, you simply need to provide the appropriate command. To create a new record, for example, use the following hash as a starting point:

```json
{
    "method": "POST",
    "url": object_path,
    "data": {
      "fieldname": "value"
    }
  }
```

> For example, if you wanted to update two records in the same request (using the PUT method), you would send the following:

```json
[
  {
    "method": "PUT",
    "url": object_url,
    "data": {
      "heading": "Breaking news"
    }
  },
  {
    "method": "PUT",
    "url": object_url,
    "data": {
      "complete": true
    }
  }
]
```
> The following request will create two new records (one news article, one author), update an author record, then delete a news record:

```json
[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Breaking news",
      "author": 1,
      "complete": false
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/authors",
    "data": {
      "email": "ann@backand.com",
      "firstname": "ann"
    }
  },
  {
    "method": "PUT",
    "url": "https://api.backand.com/1/objects/authors/1",
    "data": {
      "firstname": "Brian"
    }
  },
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/news/3"
  }
]
```

Bulk requests are implemented as arrays of request object hashes.In this hash, we specify the HTTP verb to be used - POST, in this case, which will create a new record. We set the "url" parameter to the calculated URL from above, and then provide the new record's data as a JSON hash in the "data" parameter. Simply provide the list of key and value pairs that represent your object's data, and you're set.

Once you have a single request hash, you can add others by simply appending them to the request.

You can also mix and match operations, varying the objects updated, the authorization headers used, even the type of request for each record to be updated. There are no restrictions on types of operations that can be performed in a single bulk request, so feel free to mix and match to suit your application's needs.

### Authenticating the Request
> You can authenticate each request as a different user. Simply supply the following elements in the Authorization header:
> YOUR_ACCESS_TOKEN - an access token, obtained via a call to https://api.backand.com/token
> YOUR_APP_NAME - the app name to operate on

```json
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/news/3"
    {
      "Authorization": "Bearer <YOUR_ACCESS_TOKEN>",
      "AppName": "<YOUR_APP_NAME>"
    }
  }
```

Each request hash can authenticate as a different user within your application. Any valid token to connect to your application can be provided along with each bulk item being manipulated. You can also dynamically specify the app name for each bulk item. To do so, simply use the `headers` element of the bulk operations structure as seen on the right.

Refer to our documentation on user security (http://docs.backand.com/#the-user-object-and-security) for more information on how to construct the authentication headers.


### Bulk Request Examples
Below are several examples of the types of bulk actions that can be performed using this functionality.

### Example 1: Adding multiple rows
```json
[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME",
    "data": {
      "fieldname": "value"
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME",
    "data": {
      "fieldname": "value"
    }
  }
]
```
To add multiple objects to your application, simply provide multiple POST action hashes in your request's data array. The general form of the request is:


```json
[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Breaking news",
      "author": 1,
      "complete": false
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Politics",
      "author": 2,
      "complete": true
    }
  }
]

```
Simply change the 'url' parameter of each action hash to match your object's endpoint in your application. Let's look at a specific example that adds multiple 'news' objects to an application:

As you can see above, this bulk operation will add two new objects of type 'news' to your application. The first will have the heading of "Breaking News", while the second will have the heading of "Politics".

### Example 2: Deleting multiple rows
```json
[
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME/OBJECT_ID1"
  },
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME/OBJECT_ID2"
  }
]

```
To delete multiple objects from your application, simply provide multiple DELETE action hashes in your request's data array. The general form of the request will be:

```json
[
    {
        "method":"DELETE",
        "url":"https://api.backand.com/1/objects/news/2"
    },
    {
        "method":"DELETE",
        "url":"https://api.backand.com/1/objects/news/3"
    }
]

```
By changing the YOUR_OBJECT_NAME and OBJECT_ID# parameters, you can select which specific IDs will be deleted in your application. For example, if you wanted to delete 'news' objects with IDs of 2 and 3, you would use the following set of action hashes:


### Example 3: Multiple commands
```json
[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Breaking news",
      "author": 1,
      "complete": false
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/authors",
    "data": {
      "email": "ann@backand.com",
      "firstname": "ann"
    }
  },
  {
    "method": "PUT",
    "url": "https://api.backand.com/1/objects/authors/1",
    "data": {
      "firstname": "Brian"
    }
  },
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/news/3"
  }
]

```
You can mix and match operations to be performed to match whatever actions you need to take. For example, the following set of action hashes will create a new 'news' object, create a new 'authors' object, update an existing 'authors' object with an ID of 1, and delete a 'news' object with an ID of 3:

## Retrieving Data
A Complex object is an object that has relationships to other objects in the database. For example, the default model provided when you create a new Backand application has two objects: users and items. There is a one-to-many relationship between users and items, meaning that a specific user may have many different related items. In this example, both users and items are complex objects. The relationship between the two tables, represented as the User object having a collection of Item objects, can be retrieved from the database by loading them through the object's API.

There are two ways in which you can load complex objects: Deep Loading, or Lazy Loading. Deep loading pulls back both the object and all of its related objects. In the Users/Items example, this would pull back both the user, and all items related to that user. Lazy loading, by comparison, only pulls back the parent object, allowing you to more closely control when you pull in the rest of the data. This is important when the number of related objects is large, meaning a significant load time would be necessary.

An example of another complex object would be 'email':


```shell
curl https://api.backand.com/1/objects/users/5?deep=true
```
Seeing as most objects in a Backand application tend to  be complex objects, we added an easy way to enable deep
loading of objects. Simply append '?deep=true' to the object fetch URL, like so:

```json
{
  "email":"....",
  // other users fields...
  "items":[{"title":"....",...},...]
}
```
This will result in the following JSON:


```shell
curl https://api.backand.com/1/objects/users
#or
curl https://api.backand.com/1/objects/users/5
```
It takes two requests to perform a Lazy Load. The first request is a shallow request for users or a specific user.


```shell
curl https://api.backand.com/1/objects/users/5/items
```
The second request is for all the items of a specific user.


```shell
curl https://api.backand.com/1/objects/items
```
This request is similar to fetching the entire list of items, which would be done as follows:


You can also supply other query string parameters (such as paging, filtering, and sorting). See [the getList function](#getlist) for more details.

Regarding which approach to use - lazy or deep loading - there is no clear answer. It will be dependent upon the individual programming situation. If you need to present all of the relevant data at once, then deep loading is the right approach. However, if the object has a large number of relations to pull down, it might make more sense to do a lazy load, then selectively load related objects as needed.

### Filter and Predefined Filter
```json
[{"fieldName":"firstName","operator":"contains","value":"oh"}]
```

You can supply a query string parameter named 'filter' to filter your objects during fetch. This could be useful in
developm situations, or as a part of a UI with search functionality, allowing your users to see whatever they wish. You can either create a filter with NoSQL syntax (see [NoSQL Query Language](#nosql-query-language) for syntax), or with JSON containing a field name, an operator, and a value:

See [the getList function](#getlist) for full details on filters.

#### Predefined Filter

Sometimes, for security reasons you may want to enforce partial reading of object data, usually based upon the current user or their role. This can be accomplished on the server-side using a Predefined Filter. Predefined Filters can be found in the Security tab of each object. They act as a SQL 'where' condition. Filters are combined using the 'AND' operator. You can write the filter using either a SQL statement or the NoSQL Query Language reading of partial data, usually depending on the current user or current user role. The option to force it from the server side is called Predefined Filter and you can find it at the Security tab of each object. The predefined filter acts as a SQL where statement condition. If you add additional filter that were described above then they will be added with an "AND" logic. You can write the predefined filter either as a SQL statement or as a [NoSQL Query Language](#nosql-query-language) statement.

### Queries and On Demand Actions

So far we have discussed how to read objects that were defined in the database model. To create custom data structures, we can use Queries and On Demand Actions. Queries are identical to SQL database queries but you can write them using NoSQL syntax (see [NoSQL Query Language](#nosql-query-language) for more details). With On Demand Actions, you can orchestrate any structure that you want using javascript on the server side. You can request model objects, queries, or other on demand actions. See [On Demand Actions - Server-Side Javascript Code](#custom-actions) for more information.

#### Pagination
```SQL
            SET @records := 20;
            PREPARE stmt FROM
            " SELECT    *
              FROM      Users
              LIMIT     ?, ?";
            -- {{ "{{pageNum" }}}} is the client side parameter representing the requested page number
            SET @offset := @records * ({{ "{{pageNum" }}}} -1 );
            EXECUTE stmt USING @offset,@records;
```

The LIMIT clause can be used to constrain the number of rows returned by the SELECT statement. LIMIT takes one or two numeric arguments (number of records and offset), which must both be nonnegative integer constants. We can compute the constants either on [JavaScript action](#custom-actions) or by using a prepared statement:

## Continuous Deployment and Versioning
A common use case is separate development and production environments, often with several stages in between (testing, qa, staging), which allows phased deployment (rollout) and rollback in case of problems.The goal of Continuous Deployment is to enable a constant flow of changes from development to production.

The following explains the flow of deploying server-side and databases changes from one environment to the other.

### Deployment Steps
1. First, you need to sync model changes
    1. Go to your dev app and copy the JSON model ( Objects => Model => Model JSON) to the clipboard
    2. Go to your QA app and paste the JSON model in the Edit model pan.
    3. Click 'Validate & Update'

Now the schemas are synced.

 (If you get errors when applying the schema to the QA app, you'll need to fix them before you can upload the dev configuration)

2. Loading the dev configuration into the QA environment
    1. Go to Settings => Configuration
    2. Click 'Export Current Settings' button, which will download the app configuration zip file to your file system.
    3. In the QA app go to Settings => Configuration and click the 'Import External Settings' button to upload the configuration zip file you just downloaded from the dev app.
    4. In Settings => General click the 'Clear Cache' icon in the upper-right corner (swirly arrow refresh icon near the trashcan icon), and approve the popup message.

The QA app is now deployed with all the changes you made in the dev app.

Apply this process again when deploying QA to Production.

### Versions

On every change you make to your app, Backand saves a copy (version) of the app before the change.
You can use this feature to rollback, if needed.

To rollback to a previous version:

1. Go to Settings => Configuration
2. Click the 'Set' link which corresponds to the version you want to rollback to.

To save a backup of a version just click the 'Export' link, this will save a backup of the app configuration to your file system.


## Internationalization
Supporting multiple languages often requires supporting multi-byte character sets. In order to allow for wider international character sets, you will need to run the following MySQL command against your database:

For the following scripts you'll need your schema_name , you can find it here : https://www.backand.com/#/app/<YOUR_APP_NAME>/database

```sql
ALTER SCHEMA `<YOUR_SCHEMA_NAME>`  DEFAULT CHARACTER SET utf8;
```

On the table where you want to use multi-languages run:

```sql
ALTER TABLE `<YOUR_SCHEMA_NAME>`.`<YOUR_TABLE_NAME>` CONVERT TO CHARACTER SET utf8;
```

On each VARCHAR column on that table run:

```sql
ALTER TABLE `<YOUR_SCHEMA_NAME>`.`<YOUR_TABLE_NAME>` MODIFY COLUMN col VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci;
```

The Last step is to edit the connection details

 1. Go to Setting => Database
 2. Click Get Password and save it to the clipboard
 2. If you don't see the "Edit Connection" button then add "/edit" to the browser url
 3. On the username text box  add ;CharSet=utf8;
 4. Paste the password from the clipboard
 5. Click Save

## Integrating with Ionic Creator
[Ionic Creator](https://creator.ionic.io) is an online IDE for Ionic apps that can greatly speed the development of your cross-platform mobile and web application. However, like any IDE, it can pose problems when integrating with some external service providers like Backand. Below, we’ll look at the steps needed to get your Ionic Creator app up and running with the Backand SDK, allowing you to implement serverless apps in the Ionic framework with ease.

### Configuring the connection to Backand

To Enable Back with, you’ll need to update your Ionic Creator app’s **Other JS** section to include a new file name - bkndconfig.js. In this file, replace the entire contents with the following code:

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

* `BACKAND_APP_NAME` - This is your app's name in Backand

* `BACKAND_API_SIGNUP_TOKEN` - This is your app's signup token. It is available in the **Security & Auth -> Social & Keys** section.

* `BACKAND_ANONYMOUS_ACCESS_TOKEN` - This is your app's anonymous access token. It is available in **Security & Auth -> Configuration**.

Once these changes have been made, you'll need to update your application's code settings

### Code Settings

Next, we'll update the app's Code Settings to import the Backand SDK. In **Code Settings**, under the **External JS** tab, add these two script URLs:

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

Next, under **Angular Modules**, add `backand` and `app.config`. The end result will have the following content:


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

### Working with the Backand provider

Now that the external code has been configured, you can start working with the Backand provider in your service class. For example, you can use the following code to pull rows from an `items` object in a Backand application:


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

### Updating the Page Controller

To access this data, we'll update the page controller to call our `ItemsModel` and return the relevant rows. To do so, use the following JavaScript to define a `getAll()` function and store the results in `$scope`:

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

Now, we'll need to update the page's design to display the new information. To show a list of all the items you've obtained:

* Drag a **List Item** element onto your UI

* In the page list pane (upper left hand corner), click on **List**

* On the right side of the pane, click on **Angular Directives** and add a new directive with the following details:

    * Directive: ng-repeat
    * Value: object in data

* Finally, click on the **list-item** element and change the content to `{{object.name}}`

And with that, your Ionic Creator app is now connected to Backand!

### Learning more

At this point, you have the full power of the [Backand SDK](#vanilla-sdk) available in your Ionic Creator app. You can use the SDK to add more services to your app, providing CRUD functionality, real-time communications, server-side code execution, and more! Simply head over to [our documentation](http://docs.backand.com) to get started.

## Working with Custom Actions
Actions are a powerful tool that allow you to perform customized tasks when a number of different types of events occur within your application. They provide a great alternative to server-side custom code, and can add a lot of flexibility to how your application interacts with outside services. Below we're going to look at the types of actions that Backand offers application developers, and how they can easily be used.

### Server-Side Activity in a Front-End App

One of the challenges facing Angular developers focused on front-end development is what you need to do when you hit the limits of what the front-end has to offer. Many calls that you might like to make from your JavaScript might expose security issues, such as specific formats for requests to external vendors like PayPal, that leave you vulnerable to hackers who are looking for any way to hijack your application. The traditional solution to this has been to move the calls themselves server-side, which is a lot harder to gain access to than the JavaScript code running your website. However, having custom server actions requires building up the surrounding infrastructure to handle web requests, trigger the actions, handle the responses, and perform authentication – among a number of other actions you need to control for – all just to send a single server-side call.

With Backand's server-side actions, you completely sidestep the need for all of the setup, implementation, and deployment headaches you would have with a custom server-side framework like Rails or Django. It allows you to do things that would typically demand custom server code – such as authenticating with a third party vendor, or sending sensitive payment data – without having to build the server infrastructure. By providing simple hooks into Backand's API for your back end, you can quickly add new actions that can do anything from sending a simple email all the way up to running custom JavaScript that can perform any arbitrary action you like – all behind the server-side call to Backand!

### Triggers

Actions are activated via triggers in your application. Generally speaking, there are two types of action triggers in Backand – database-related triggers, and "On Demand" triggers. Database triggers fire under certain conditions when your application interacts with your database. You can add custom triggers to each of the standard database actions that update the database – record creation (Create), record updating (Update), and record deletion (Delete). Furthermore, you can assign your actions to occur either before the action takes place (such as "Before Update"), while the action is taking place as part of the same transaction (such as "During Create"), or after the action has completed (such as "After Deletion"). Through clever use of these actions, you can create a very complex database experience with a deceptively simple front-end.
On Demand triggers, on the other hand, happen whenever you like. These are associated with a specific object, and are performed in the context of a row via a simple call to an associated URL. These actions operate within the context of the provided row, allowing you to perform any custom actions you need to on that specific object. This allows you to build out a true API, providing the hooks for many custom server actions that would often need a framework surrounding them.

### Types of Actions

Once you've selected an appropriate trigger, you need to select a type of action to perform. Backand currently offers three types of actions – Send an Email, Execute Custom JavaScript, and Execute Custom SQL. Send an Email, as the name implies, simply sends an email whenever the trigger in question occurs. You can use the subject of the trigger as a local variable, inserting details into the message as you desire. Execute Custom JavaScript allows you to write a custom JavaScript function that will execute during the trigger, all within a server context. This allows you to perform sensitive communication without exposing vital details to potential attackers. Finally, Execute Custom SQL allows you to run a custom SQL script when the trigger is encountered, performing additional bookkeeping or updating related objects based upon the action performed.  With these three action types, you should be able to perform many highly complex interactions without needing to worry about maintaining a back-end server to manage them.

### Summary

One of the common concerns with outsourcing the back end of an application is "What happens when I need something more from the server?" With Backand's actions, you have the opportunity to perform a number of different types of actions at several highly-configurable trigger points, and all in the context of your application's server! This can allow you to implement sensitive web calls to third parties, maintain complex analytics back-ends, and protect your sensitive data from attackers – all with a few clicks in Backand's application dashboard. While actions are not ideal for all situations, they should suffice for the vast majority of server-side activity that web apps most commonly need.

## Connecting to Your App's Database
While the schema editor offered by Backand's app dashboard can be a powerful tool for managing your application's data schema, sometimes there are ideas that the schema editor can't express. For these situations, it's often best to revert to a SQL interface, either using command-line SQL or a dedicated database administration tool like MySQL Workbench. In this post, we'll look at how you can access your Backand application's database using any third-party MySQL compatible database tool you desire, and how you can sync the changes with your Backand application once you've made your changes.

###Connecting to Your Database
Backand hosts all application databases as schemas in Amazon RDS. The benefit of this approach is that it easily allows us to make your database available to any third-party tool you like. In the Backand App Dashboard, you can find your application's database server information in the **Settings -> Database** panel. This contains the host URL, username, password, and schema name for your application. Simply input these connection values into your database administration tool of choice, and you are free to make changes to your database schema.

###Synchronizing Changes
Once you've made the necessary changes in your schema, you'll need to synchronize the changes with your Backand application. This is accomplished in the Backand Application Dashboard as follows:

* Navigate to the **Model** panel, under **Objects**
* Select the **Model Database** tab
* After reviewing the text, click on the **Sync with Database** button

Once you've pressed the Sync button, Backand will use reflection to determine your database's structure, and rebuild your app's REST API according to the changes it sees.

###Moving Forward
With this approach, you can greatly increase your data model's flexibility.  By working with standard SQL queries, you can bypass the need for making complex changes to your application's data structure using only the provided editors. Anything that can be expressed in SQL is permitted, and Backand will automatically adapt to your database structure modifications.

## Working with Encrypted passwords
One common case encountered when storing passwords is in how to manage password encryption - particularly for working with third parties. In this section, we'll explore encrypting a user's password, and comparing encrypted passwords with their plaintext equivalents to check for authorization.

### Encrypting Passwords
To work with encrypted passwords in Backand, follow these steps:

1. Create a `password` field in your `users` object, of type `text`
1. Modify or create a "before create" action for the `users` object, and set the type to Custom Server-Side JavaScript.
1. Add `userInput.password = security.hash(userInput.password);` to encrypt the password

With that, your encrypted password is now stored along with your `users` object.

### Comparing Encrypted passwords

Once you've encrypted your password, you'll want to use it for password validation. To do so, you can use the `security.compare` function: `return security,compare(password, hashedPassword);`.

This method returns `true` if the provided password is valid. It utilizes one-way encryption, so that it is not possible to decrypt the password hash

## Using UUID Primary Keys

While integer IDs can provide a quick and easy way to get up and running with a primary key, they face a number of different challenges when it comes to both scaling and security:

* They're limited in scope, meaning you're only given the maximum range of the integer object as unique IDs
* They're easy to understand, meaning they're also easy to hack (in other words, "What happens if I change this ID of 77 to 78?")
* In general, they're hard to make thread-safe. This is an issue when a data source is operating at internet scale, or when working with multiple concurrent databases.

Universally-Unique IDentifiers, or UUIDs, solve many of these problems. They obfuscate the ID so that it is not brute-force guessable, they have an extremely wide range of potential values, and the [risk of collisions in UUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier#Collisions) is miniscule. In this section, we'll look at configuring a Backand object to use a UUID as its primary key.

### Setting up the Database
To create a UUID primary key column, you'll need to operate within your database directly. Download a tool to connect to your database (like [MySQL Workbench](https://www.mysql.com/products/workbench/)), and configure it with the connection settings from [your application's dashboard](#connecting-to-your-apps-database). Once you've connected your database, you'll next want to create a new field in the object you are changing, using the datatype `char(36)`. Once this is done, sync the changes back to your application using the Object -> Model -> Model Database tab in the app dashboard.

<aside class="notice">Backand uses auto-incrementing integers as the default for IDs in your objects, meaning in the default case values provided in the ID field will be overwritten with the next sequential ID. You'll need to remove this constraint in order to successfully populate the ID field with other data. You can also use an alternative field in the object as the primary key - simply make the designation in SQL, and Backand will update once you sync the database changes.</aside>

### Setting up the Before Create action
```javascript--persistent
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
  userInput.id = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
      var r = Math.random()*16|0, v = c == 'x' ? r : (r&0x3|0x8);
      return v.toString(16);
  });
  return {};
}
```
We'll override the default integer-based behavior in a Custom Server-Side JavaScript action that we will create expressly for this purpose. The code to the right uses `Math.random()` to generate a UUID Version 4-compliant value. This value is then stored in userInput.id, which in turn populates the column in the database.

<aside class="warning">It is important to note that this value is not truly random, and should not be used in situations where security is paramount - in those cases, we recommend you either implement your own algorithm or integrate with a third party library that can do so on your behalf.</aside>

## Integrating Third-Party Security
While Backand provides a set of robust and dependable authentication and user management tools, sometimes you need a deeper integration with a provider that is not directly supported via integration with the Backand platform. In this post, we'll look at three mechanisms you can use to implement robust user security integrations with arbitrary third-party platforms. By using the `backandAuthOverride`, `sociaAuthOverride`, and `accessFilter` security actions, you'll be able to achieve all of your user security goals.

###A Quick Review of User Security
There are two questions that need to be resolved in any conversation - user authentication, in which we determine whether or not the user is who they claim to be, and user authorization, in which we determine whether or not the authenticated user has access to the resources they are requesting. Backand uses OAuth2 to manage user authentication for plain username/password logins, and social media providers to manage user authentication when configured. To manage authorization, Backand provides a flexible and powerful set of security templates and security roles, which can be used to control the access a user has to any given field in a Backand application.

###Overriding Backand Authentication
While we offer a number of ways to integrate with third-party security providers, some applications need to work within systems that we either do not offer integrations with, or whose integrations are not feature-rich enough to support all of the needed functionality. To work around this, we provide the `backandAuthOverride` security action. By modifying this action, you can use it to authenticate the user with any third party authentication platform you desire. You're given the user's email address and password, and can respond with one of three statuses:

* **allow** - Allow the user access to your application
* **deny** - Deny the user access to your application
* **ignore** - Ignore this override, and use Backand User Authentication (default)

If this method returns "ignore", the standard Backand user authentication is used. Otherwise, the results of this function are adopted by Backand. If you wish, you can use the provided `additionalTokenInfo` object to pass back supplementary information for the authentication result, such as system user ID, system role, authentication lifetime, and so on - these will be available with the return value from the `signin` method, or via the SDK method `getUserDetails`.

###Overriding Social Media Authentication
While you can use `backandAuthOverride` to integrate with most third-party authentication providers, if you're working with our social media authentication you'll need to use a different function - `socialAuthOverride`. This function, similar to `backandAuthOverride`, overrides the social media authentication experience, returning one of the following three values:

* **allow** - Allow the user access to your application
* **deny** - Deny the user access to your application
* **ignore** - Ignore this override, and use Backand Social Media Authentication (default)

Once again, if this method returns `ignore`, the default built-in social media authentication provided by Backand is used. Otherwise, the return value of this function - either `allow` or `deny` - coupled with the contents of `additionalTokenInfo` are returned to the caller, and the user is either authenticated or prevented access appropriately. You can use this pattern to, for example, obtain a user's Facebook profile picture upon completion of authentication, and pass the results back to the application for display or manipulation.

<aside class="notice">
While this function works similarly to backandAuthOverride, there are a couple minor differences that are important to note.
  <ol>
    <li> With socialAuthOverride, the only things you can do are deny access, or provide additional information. The backandAuthOverride function gives you more power over the result. </li>
    <li> The socialAuthOverride function is called for both user authentication <strong>and</strong> for social media registration. The backandAuthOverride function is only called during authentication.</li>
  </ol>
</aside>

###Overriding Authorization
Once you've authenticated the user, you'll need to determine whether they are authorized to access the specified resource. While our security roles and templates can cover a lot of functionality, they are by nature restricted to a single app. If you organization's security policy restricts app access to users with a specific flag in your authentication mechanism, you'll want to override basic Backand authorization to instead leverage your external solution. To do this, you can use the `accessFilter` method to dynamically approve or deny any users that authenticate with your system using an Oauth2 token.

This method works like any other custom JavaScript action, allowing you to easily contact third party services for additional user information. Once you've used this feature to determine whether or not a user is authorized, you simply return `allow` or `deny` to permit or restrict access appropriately. You can also provide an additional message explaining the denial, or providing additional information on the approval.

###Conclusion
The security landscape for internet apps is wide and varied. While we'd love to be able to support every possible method of authentication available to developers, after a while it is far more beneficial and flexible to provide the developer with tools to manage their own authentication integrations. Using our provided override functions - `backandAuthOverride`, `socialAuthOverride`, and `accessFilter` - you can integrate with any authentication or authorization provider that can communicate over the web.


The "Objects" section of the dashboard allows you to modify the specific objects in your application's database. A menu entry is created for each table in your database, and allows you to configure your application at the object level. For each object in your system, you are given access to the following areas to modify and work with.

## Managing the Application Model with JSON
> Below is an example model

```json
[
  {
    "name": "items",
    "fields": {
      "name": {
        "type": "string"
      },
      "description": {
        "type": "text"
      },
      "user": {
        "object": "users"
      }
    }
  },
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "firstName": {
        "type": "string"
      },
      "lastName": {
        "type": "string"
      },
      "items": {
        "collection": "items",
        "via": "user"
      }
    }
  }
]
```

The Model JSON tab in the Object menu allows you to modify the JSON schema of your database's model structure, adding new objects and establishing the relationship between them. Below is a brief overview of how to use this interface:

<aside class="notice"> In addition to the fields supplied by the user, Backand defines an 'id' field, of type integer, which will be used as a primary key for the table.</aside>
<aside class="warning"> Renaming fields can only be done from the Fields tab in the application dashboard. Renaming a field or an object in a model will delete the data associated with that field or object.</aside>

###Definitions
> The model represents a database schema that is defined as a JSON array of one or more object (Table) definitions.

```json
<model> = [  <object>, <object>, ... ]
```
> An object definition is a JSON object with a name entry and a fields entry:

```json
<object> = { "name":  <string>, "fields" : <fields> }
```
> The fields definition is a JSON list of field attributes:

```json
<fields> =  { "field1" : <field>, "field2": <field>, ... }
```
>A field is defined by its type and a set of optional properties. The field definition is a JSON object:

```json
<field> = { "type": <type>, <optional properties>}
```


A JSON schema consist of three types of objects:

* model - This is the overall definition for your database schema. It consists of a list of `objects`
* object - This represents a single object type in your application. It is converted into a database table.
* fields - This represents an array of field entries, representing the possible attributes of your object. These become columns in the database table.

A field may have one of the following types:

* **string** - string column with length up to 255 characters
* **text** - text column with length up to 21,844 characters
* **float**
* **datetime**
* **boolean**


###One-to-Many Relationship
> As an example, consider a model describing pet ownership. It has two objects, 'users' and 'pets'. Each user can own
several pets, but a pet has a single owner (user). Thus, the users-pets relationship is a one to many relationship between users and pets, The 'users' object will have a 'pets' relationship field, which establishes the 'many' side of the relationship by
creating a collection of pets objects for each user in the model.
> In 'users', add this field:

```json
"pets": { "collection": "pets", "via": "owner" }
```
> The 'pets' object will have an 'owner' relationship field, which establishes the 'one' side of the relationship by
linking each pet back to an individual owner.
> In 'pets', add the following new field to complete the linkage:

```json
"owner": { "object": "users" }
```

> Here is the full sample model for this relationship:

```json
[
  {
    "name": "pets",
    "fields": {
      "name": {
        "type": "string"
      },
      "owner": {
        "object": "users"
      }
    }
  },
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "firstName": {
        "type": "string"
      },
      "lastName": {
        "type": "string"
      },
      "pets": {
        "collection": "pets",
        "via": "owner"
      }
    }
  }
]
```


Many databases rely on one-to-many relationships between objects. These happen when an object can exert "ownership" over another, and is typically represented as a foreign key constraint in the "owned" object. The sidebar demonstrates how to create a one-to-many relationship in the database using the JSON Schema.

Say, for example, that we have a one to many relationship between objects R and S. This means that for each row in R, there are many potentially corresponding rows in S. In the 'many' side of the relationship (object S), we specify that each row relates to one row in the other object R by providing an object field link:

`"myR" : { "object" : "R" }`

In the 'one' side of the relationship (object R), we specify that each row relates to several rows in S by providing a collection field link, using a 'via' attribute to denote which field on the 'many' side (object S) fulfills the relationship:

`"Rs" : { "collection": "S", "via" : "myR" }`

In the database, a foreign-key constraint will be added between tables S and R (pointing in the direction of R), represented by a foreign key field 'myR' being created in the object S's data table. This field will hold the primary key of the corresponding row in R for each row in S. One-to-many relationship between objects are specified using relationship fields. A relationship field will generate the appropriate foreign-key relationship fields in the corresponding relation objects.



###Many-to-Many Relationship
> As an example, consider a different model describing pet ownership. It has two objects, 'users' and 'pets'. Each user
 can own several pets, and each pet can have several owners. Thus, the users-pets relationship is many to many relationship between users and pets:
> First we need to add a new object - 'users-pets' - with one-to-many relationships to both 'pets' and 'users' objects.
> In 'users-pets':

```json
"pet": { "object": "pets" }
"owner": { "object": "users" }
```
> In the corresponding objects 'users' and 'pets', we need to add a reference to 'users-pets' to complete the
one-to-many relationship:
>In 'pets':

```json
"owners": {"collection": "users-pets", "via": "pet"}
```
>In 'users':

```json
"pets": {"collection": "users-pets", "via": "owner"}
```
> Stated another way: To build a many-to-many model of many-to-many Model:
> * Start with a Model that contains 2 objects that have no relations between them:

```json
[
  {
    "name": "pets",
    "fields": {
      "name": {
        "type": "string"
      }
    }
  },
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "firstName": {
        "type": "string"
      },
      "lastName": {
        "type": "string"
      }
    }
  }
]
```
> Add the new object and the relationship fields:

```json
[
  {
    "name": "pets",
    "fields": {
      "name": {
        "type": "string"
      },
      "owners": {
        "collection": "users-pets",
        "via": "pet"
      }
    }
  },
  {
    "name": "users-pets",
    "fields": {
      "pet": {
        "object": "pets"
      },
      "owner": {
        "object": "users"
      }
    }
  },
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "firstName": {
        "type": "string"
      },
      "lastName": {
        "type": "string"
      },
      "pets": {
        "collection": "users-pets",
        "via": "owner"
      }
    }
  }
]
```

Many-to-Many relationships between objects are added by adding a new object that has a one-to-many relationship with each object participating in the many-to-many relation. Please review [One-to-many relationships](one-to-many-relationship) before continuing with this section.

Say, for example, that we have a many to many relationship between objects R and S. This means that for many rows in R, there are many potentially corresponding rows in S. Where we could handle a one-to-many relationship between R and S using a single column in the underlying database, we can't do so to represent a many-to-many relationship. If we simply add another foreign key to the other side of the relation, we are stuck having to duplicate items that are shared among multiple owners. Instead, we use a mapping table to link the two objects together.

`{"name": "R_TO_S_MAPPING", "fields": { "r": {"object":"R"}}, "s": {"object":"S"} }`

 The mapping table will have two columns - r_id, and s_id - that are then linked to by the source tables via separate one-to-many relationships between the objects themselves and the mapping table. More specifically: S will have a one-to-many relationship with R_TO_S_MAPPING:

`{"name": "R", "fields": {...., "S": { "collection": "R_TO_S_MAPPING", "via":"r"}}}`

 ... and R will also have a one-to-many relationship with R_TO_S_MAPPING


`{"name": "S", "fields": {...., "R": { "collection": "R_TO_S_MAPPING", "via":"s"}}}`

...  resulting in a many-to-many relationship between R and S with the relationship managed via table R_TO_S_MAPPING.

## Enabling SMS-based Authentication

Using Backand's security actions and a service like Twilio, you can implement a token-based authentication system that uses SMS as the delivery method, giving your users the capability to authenticate with a code received via a SMS message on a mobile device.

### Process overview

The process we'll follow to authenticate the user in our SMS-based system is as follows:

1. Call an on-demand action that creates a Token object. The Token object contains a token number, and a session ID. The token number is sent to the provided phone number via SMS, while the session ID is sent back to the caller.
2. Call backand's `/token` API with a username (any username is acceptable), the SMS code, and the session ID sent back as a result of step 1. WE'll use a security action to verify the provided token data, and flag the token as "used" to remove it from active status.
3. Restrict the token object to only be accessible by Admin users, to prevent malicious third-party intrusions.


For multiple device authentication, you can use the same process - even keeping the same username. See an example of the code at work in CodePen: [http://codepen.io/backand/pen/GZdYgy](http://codepen.io/backand/pen/GZdYgy)

### Creating the Token object

```JSON
{
  "name": "tokens",
  "fields": {
    "token": {
      "type": "string",
      "required": true
    },
    "sessionId": {
      "type": "string",
      "required": true,
      "unique": true
    },
    "used": {
      "type": "boolean",
      "defaultValue": false
    }
  }
}
```

The JSON to create a token object is provided to the right. This consists of three columns:

* `token` - the token value sent to the user via SMS
* `sessionId` - the session ID to be used by the client in authenticating the token
* `used` - a boolean flag indicating whether or not the token has been used

### Adding the sendToken action
```javascript-persistent
//sendToken action
// Required parameters (to be stored in the 'parameters' object): phone
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {
    // When in debug mode, don't send SMS messages
    var debug = false;

    // Creates a random number to serve as the Token value
  	var randomNums = function(length) {
        return Math.random().toString().slice(2,length+2);
    }
    // Creates a random string to serve as the session ID.
    // Replace with a GUID function if you wish to avoid collisions.
    var randomText = function (length) {
	    if (!length) length = 10;
	    return Math.random().toString(36).slice(2,length);
	  }

    // These are used to construct basic authentication. The first value is
    // the master token for your app. The second value is a user's user token.
    // See the docs at http://docs.backand.com/#basic-authentication for more
    // information on this method of authentication
    var master_key = 'f8929838-2a2e-4818-a47e-2080ad62fc00';
    var user_key = 'f9d89228-0336-11e6-b112-0ed7053426cb';

    // Get a session ID and a token
    var sessionId = randomText(32);
    var token = randomNums(6);

    // Store the new token in the database using an API call
    var response = $http({
        method: "POST",
        url:CONSTS.apiUrl + "/1/objects/tokens",
        data: {
            token: token,
            sessionId: sesstionId
        },
        headers:{'Authorization':'basic ' + btoa (master_key + ':'+ user_key) }
    });

    // From this point onward, we construct and send an SMS message to the user
    var ACCOUNT_SID = 'YOUR TWILIO ACCOUNT SID';
    var AUTH_TOKEN = 'YOUR TWILIO AUTH TOKEN';

    var FROM_PHONE_NUM = 'YOUR TWILIO NUMBER';

    // Construct the endpoint in Twilio's API
    var basicUrl = 'https://api.twilio.com/2010-04-01/Accounts/' + ACCOUNT_SID + '/';
    var action = 'Messages.json' // twilio have many services

    // Only send SMS messages when we are not debugging
    if(!debug){
        // Send the request to Twilio for processing.
        var t1 = $http({
            method: "POST",
            url: basicUrl + action,
            data:
                'Body=' + 'Your verification code is: ' + token +
                '&To='  + '%2B' + parameters.phone +
                '&From='+ FROM_PHONE_NUM,
            headers: {
                "Accept":"Accept:application/json",
                "Content-Type": "application/x-www-form-urlencoded",
                "Authorization": 'basic ' + btoa(ACCOUNT_SID + ':' + AUTH_TOKEN)
            }
        });
    }

    //Return the Guid
	  return {"sessionId": sessionId};
}
```

To generate the token data for each authentication attempt, we'll use an on-demand server side JavaScript action. As all actions require an owning object, we'll attach this action to the `items` object in the database. While it would make more sense to link this action to the `token` object, the `token` object will have other security restrictions that will interfere with calling this action prior to authentication. So while we have chosen the `items` object, it can apply to any object in your system accessible to anonymous users.

Use the JavaScript to the right as the body of the action code.

<aside class="warning">Ensure that you update the values stored in master_key and user_key for your application. More information is in the documentation at <a href="#basic-authentication">/#basic-authentication</a>. You'll also need to create a Twilio account by following the instructions provided <a href="#setup-a-free-account-in-twilio">here</a> - update the ACCOUNT_SID, AUTH_TOKEN, and FROM_PHONE_NUM with the correct values for your account.</aside>

### Updating the security action to allow for authentication
```Javascript-persistent
// backandAuthOverride action
// Required parameters (in the parameters object): token, sessionId
'use strict';
function backandCallback(userInput, dbRow, parameters, userProfile) {

    //Example for SSO in OAuth 2.0 standard
    // Get the token and session ID from the parameters
    var token = parameters.token;
    var sessionId = parameters.sessionId;

    // If you're debugging this on the command line, you'll want to throw a new
    // Error with the session ID
    //throw new Error(sessionId);

    //Check if token exists and not in use
    var master_key = 'f8929838-2a2e-4818-a47e-2080ad62fc00';
    var user_key = 'f9d89228-0336-11e6-b112-0ed7053426cb';

    var res = $http({
        method: "GET",
        url: CONSTS.apiUrl + "/1/objects/tokens",
        params: {
            filter: [
            {
                fieldName: "sessionId",
                operator: "equals",
                value: sessionId
            },
            {
                fieldName: "token",
                operator: "equals",
                value: token
            },
            {
                fieldName: "used",
                operator: "equals",
                value: false
            },
            ]
        },
        headers:{'Authorization':'basic ' + btoa (master_key + ':'+ user_key) }
    });

    //Generate a temporary password and username to serve as user input.
    var result = "deny";
    if(res.data.length == 1){

      result = "allow";

      //set the username and password
      var randomText = function (length) {
        if (!length) length = 10;
        return Math.random().toString(36).slice(2,length);
	    }
	    userInput.username = randomText(10) + '@domain.com';
	    userInput.passsword = randomText(18);

	    //Update the token to mark it used.
      var response = $http({
        method: "PUT",
        url: CONSTS.apiUrl + "/1/objects/tokens/" + res.data[0].id,
        data: {
            used: true
        },
        headers:{'Authorization':'basic ' + btoa (master_key + ':'+ user_key) }
      });
    }
   //Return results of "allow" or "deny" to override the Backand auth and provide a denied message
   //Return ignore to ignore this function and use Backand default authentication
   //Return values in additionalTokenInfo that will be added to Backand auth result.
   //You may access this later by using the getUserDetails function of the Backand SDK.
   return {"result": result, "message":"Incorrect information", "additionalTokenInfo":{}};
}
```

Next, we need to use the `backandAuthOverride` security action to override the standard Backand authentication process. See more info on how this works in the documentation on [using third-party security](#integrating-third-party-security).

Replace the code of the action `backandAuthOverride` with the JavaScript on the right.

<aside class="warning">Once again, ensure that you update the values stored in master_key and user_key for your application. More information is in the documentation at <a href="#basic-authentication">/#basic-authentication</a>.</aside>

### Restricting access to the token object
As the token object gives users a lot of power within your application, it is important that you restrict access to the `token` object in your app's API. Follow these steps to update the `token` object's security, restricting access to administrators only:

1. Open the Security tab of `token` object

2. Click on 'Override the Security Template' and set it to `true`

3. Uncheck all boxes until only the checkboxes for the Admin role remain checked.

###Disable token expiration
Finally, you'll want to update your authentication tokens to never expire. This allows your SMS-based authentication to be persistent, meaning the user won't need to periodically respond to a new text message requesting re-authorization unless you explicitly choose so. To modify the token expiration length:

    a. Open the `Security & Auth --> Social & Keys` menu
    b. Change Session Length to `Never - use refresh token`

### Building from here
This approach opens a number of options for authentication, supporting use cases more complex than "user is sitting at a computer with a web browser open". You can easily use this approach to implement a two-factor authentication model, for example - by requiring the session token in addition to the user's username and password, you can get more granular control over the user's access to your data. You can also use this to implement persistent authentication in a more private application environment, such as when running an app on a user's mobile device.

## Reducing Backand Compute usage

One common cost center for developers using the Backand platform is the Backand Compute Unit. These Units accrue as a result of API calls and other requests to the Backand platform, and reflect the computing effort expended by Backand to perform the requested operations. Below, we'll look at some strategies you can use to mitigate the accrual of Backand Compute Units, letting you more easily mitigate costs and resource usage within your Backand application.

### Strategy 1 - Reduce the number of calls
Backand Compute Units are directly proportional to the number of API calls you make. As such, the most obvious strategy is to reduce the number of calls you make to the API. Some of this can be managed via standard caching and code optimization techniques, simply storing data that changes less frequently instead of fetching it from the server every time it is called. But you can also make use of Backand to reduce the amount of API calls necessary to perform common tasks. The Bulk endpoint, for example, allows you to combine up to 1,000 API requests into a single API call, reducing the cost of executing these API requests by that same 1,000 factor. See more information on making bulk requests [here](#bulk-operations).

### Strategy 2 - Combine multiple calls into an Action
A unique quirk of the Backand platform is that performing complex code in a custom on-demand action only counts as a single Backand compute unit. So if you are able, consolidate related Backand API requests into calls that take place within a custom action. There's no limit on the number of calls you can make, and through the use of parameters you can achieve most of the tasks you'd need separate API requests to accomplish.

### Strategy 3 - Make use of a plan-included test app
If your plan allows for it, you may receive a test app allowance with your Backand subscription. These test apps are not billed, and are ideal for a development platform. Once you've finished developing your application, you can port the changes to your production instance, to ensure you are ready to serve your content to your users. Review our [pricing page](http://backand.com/pricing) to see what types of options are included with your current package, and whether it makes sense to upgrade your subscription plan.

### Strategy 4 - Move database updates into queries
Similar to moving multiple API requests into a single action, you can also consolidate multiple table updates into a single Query. Simply modify the query to perform multiple reads or updates, and return the results you want back to your calling code. Like custom actions, you'll only incur a usage charge for a single call, even if the query is making dozens of updates each time it is called.

### Keeping an eye to resource utilization
Backand Compute Units can be troublesome to manage, but if you treat them like any other computing resource they are capable of being optimized along a number of vectors. If none of the strategies above help your specific use case, [contact us](#contact-us) to discuss other alternative solutions - we are always willing to help, and want to help you get the most out of your usage of the Backand platform.

## Contact us
We'd love to hear how you use Backand, and if you have any development or usage patterns that you have found useful! Send us a message with feedback, bug reports, or anything else at [support@backand.com](mailto:support@backand.com) for more info.
