This section covers [Backand's Angular 2.x SDK 2.3.x](https://github.com/backand/angular2bknd-sdk)

### Getting Started

Backand's Angular 2 SDK is compatible with AngularJS 2.0.x, and provides methods for easily communicating with Backand's servers, working with objects in a Backand application, and handling users of your  Backand app. To start using the SDK, follow these steps:

## Installation

First, install the SDK using NPM:

```

    npm install angular2bknd-sdk --save

```

## Dependencies

Once the SDK is installed, install the dependencies for the SDK:

```

    npm install @types/node --save-dev
    npm install @types/socket.io-client --save-dev

```

## Adding Backand to your App

Now that the SDK is installed, it needs to be included in your project. Add the following line to 'src/app/app.module.ts' to provide access to the SDK. You will also need to do this in every component that requires Backand:

```

    import { BackandService } from 'angular2bknd-sdk';

```

Once you've included the BackandService object, add it to the `providers` array. Then, you are ready to start working with the SDK. Add a parameter to the constructor to define the service and provide access:

```

    constructor(private backandService:BackandService){}

```

Now you can refer to the service via the `this` keyword, as `this.backandService`.

## Configuring Your App's Details

Now, configure the SDK to connect to your Backand application. This configuration is done using three methods: `setAppName`, `setSignUpToken`, and `setAnonymousToken`. To do this, add the following to your 'src/app/app.component.ts' file:

```

    this.backandService.setAppName('your app name');
    this.backandService.setSignUpToken('your backand signup token');
    this.backandService.setAnonymousToken('your backand anonymous token');

```

## Configuring Mobile Apps

In order to use the SDK with a mobile app, you need to set a flag in the `backandService` object using the `setIsMobile` function. To do this, add the following line to your 'src/app/app.component.ts' file:

```

    this.backandService.setIsMobile(true);

```

## Authentication

Backand has methods to support all of the ways a user might choose to authenticate with your app. The SDK puts all of these behind easy-to-use functions. Below, we'll explore each of the authentication and registration methods available.

_Note: If your app operates without users, this section does not apply - you only need to configure your app's `anonymousToken` value in order to authenticate with your back-end application._

### signin

#### Description
Use the `signin` function to authenticate your users when they opt to sign in to your app. This call returns an access token that is used in the SDK to authenticate within the context of the signed-in user. This token is stored in the API, and is not something you will need to work with directly.

#### Usage
Add the `signin` function to your app using the following code:

```

    var $obs = this.backandService.signin(username, password);
        $obs.subscribe(
            data => {
                console.log('Sign in succeeded with user:' + username);
            },
            err => {
                this.backandService.logError(err)
            },
            () => console.log('Finish Auth'));
```

#### Parameters

* username - the username to authenticate
* password - the user's password

### signup

#### Description

The `signup` method adds a new user to your application. This user is a standard app user, and will be available in Backand's users table. If you have a custom `Users` object, Backand will also attempt to add a new record to this object as well.

_Note: if your app's `Users` object has additional fields that are marked as 'required', or custom actions whose failure prevents records from being created, the automated user insertion process may not complete successfully._

#### Usage

To use the `signup` method, copy the following code into your app:

```
    var $obs = this.backandService.signup(email, signUpPassword, confirmPassword, firstName, lastName);
        $obs.subscribe(
            data => {
                console.log('Sign up succeeded with user email:' + email);
            },
            err => {
                this.backandService.logError(err)
            },
            () => console.log('Finish Auth'));

```
#### Parameters

* email - the new user's email
* signUpPassword - the user's desired password
* confirmPassword - the user's password confirmation (should match `signUpPassword` exactly)
* firstName - the user's first name
* lastName - the user's last name

### signout

#### Description

The `signout` method clears the locally-stored access token obtained from the `signin` method, signing your user out of your application.

_Note: After `signout` is called, you will need to re-authenticate your user, or restrict the user to only functionality available for use with the anonymous access token for your application_

#### Usage

To sign a user out of your app, copy the following code into the appropriate location:

```

    this.backandService.signout();

```

#### Parameters

None

### changePassword

#### Description

The `changePassword` function changes a user's  password in your Backand application.

_Note: A best practice for user password modification is to prompt them to confirm their password in a separate field on your change password form. It is highly recommended that you follow this pattern, and prevent the changePassword function from being called if the new password and its confirmation entry do not match. This code does not account for this functionality_

#### Usage

To use the `changePassword` function, copy the following code into the appropriate location in your app:

```

    var $obs = this.backandService.changePassword(oldPassword, newPassword);
        $obs.subscribe(
            data => {
                console.log('Password changed);
            },
            err => {
                this.backandService.logError(err)
            },
            () => console.log('Finish change password'));

```

#### Parameters

* oldPassword - the user's old password
* newPassword - the user's desired new password

## Social Media Authentication

In addition to basic user authentication, Backand also provides you with the option to allow your users to authenticate with a social media account. To add this functionality to your Backand application, use the methods below.

### socialSignin (also for signing up)

#### Description

The `socialSignin` function redirects your user to a social media provider in order to authenticate. This call can also be used to let the user register a new account with a social media provider - if the user needs to sign up before signing in, that will be handled on the provider's website.

#### Usage

To use `socialSignin`, add the following code to the appropriate location in your app:

```

    var $obs = this.backandService.socialSignin(provider, spec);
    $obs.subscribe(
      data => {
          console.log('Sign up succeeded with:' + provider);
      },
      err => {
          this.backandService.logError(err)
      },
      () => console.log('Finish Auth'));

```

#### Parameters

* provider - the string name of the social media provider to authenticate with
* spec - ????

## CRUD - START HERE NEXT

To fetch, create, and filter rows, from an object, say 'items', the CRUD functions in BackandService, should receive 'items' as their first argument:

* Read one row

```

    this.backandService.getOne('items')
        .subscribe(
                data => {
                },
                err => this.backandService.logError(err),
                () => console.log('OK')
            );

```

* Create

```

    //data - input JSON with all fields need for create
    this.backandService.create('items', data)
        .subscribe(
                data => {
                },
                err => this.backandService.logError(err),
                () => console.log('OK')
            );

```

* Update

```

    //data - input JSON with all fields need to be updated
    this.backandService.update('items', id, data)
        .subscribe(
                data => {
                },
                err => this.backandService.logError(err),
                () => console.log('OK')
            );

```

* Query

When 'q' is set to your search pattern, define a filter:

```

    let filter = [{
        fieldName: "name",
        operator: "contains",
        value: q
    }];

```

Or use NoSQL syntax:

```

    let filter = {
        "q":{
            "name" : {
                "$like" :  q
            }
        }
    }

```

Query with filter:

```

    this.backandService.getList('items', null, null, filter)
        .subscribe(
            data => {
                this.items = data;
            },
            err => this.backandService.logError(err),
            () => console.log('OK')
        );

```

* 'provider': facebook, twitter, google, github
* 'spec': optionally defines the look of the social network sign in window, like: left=1, top=1, width=600, height=600

## Socket Service

* Socket login and logout are done automatically as part of the login and logout calls, respectively.

* To subscribe to event 'items_updated' from server side via sockets, call 'this.backandService.on' and in your controller, subscribe with:

```

    this.backandService.on('items_updated')
      .subscribe(
            data => {
                console.log("items_updated", data);
            },
            err => {
                console.log(err);
            },
            () => console.log('received update from socket')
        );

```

## Get User Details

Fetch:

```

    this.backandService.getUserDetails(true).subscribe(
       data=> {
           console.log(data);
       },
       err=> this.backandService.logError(err),
       () => console.log('Got Details')
    );

```

Caches user details in the app. The 'force' parameter can cause it to fetch from it from Backand as in the call above.

## Backand Storage

### Create Backand Action

Create a server side action in Backand by going into the 'items' object actions tab and clicking on the Back& Files icon. Name your action 'files'

### File Upload

```

    backand.uploadFile('items', 'files', fileName, base64Data).subscribe(
        data => {
            console.log(data);
            //data.url is the url of the uploaded file
          },
          err => backand.logError(err),
          () => console.log('OK')
    );

```

### File Delete

```

    backand.deleteFile('items', 'files', fileName).subscribe(
        data => {
        },
        err => backand.logError(err),
        () => console.log('OK')
    );

```
