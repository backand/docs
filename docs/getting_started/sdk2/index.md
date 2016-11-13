This section covers [Backand's Angular 2.x SDK 2.3.x](https://github.com/backand/angular2bknd-sdk)

### Getting Started

The Backand Angular 2 SDK compatible with AngularJS 2.0.x and provides methods for easily communicating with the Backand server and performing common tasks, such as managing users. To use the SDK, follow these steps:

## install

    npm install angular2bknd-sdk --save

## Dependencies

    npm install @types/node --save-dev
    npm install @types/socket.io-client --save-dev

## Import

In 'src/app/app.module.ts',

```
    import { BackandService } from 'angular2bknd-sdk';
```

add 'BackandService' to the 'providers' array.

In each component where you use Backand, import it:

```
    import { BackandService } from 'angular2bknd-sdk';
```

and then in the constructor:

```
    constructor(private backandService:BackandService){}
```

Use it as 'this.backandService'

## Set Your App Details

1. In 'src/app/app.component.ts':

```
    this.backandService.setAppName('your app name');
    this.backandService.setSignUpToken('your backand signup token');
    this.backandService.setAnonymousToken('your backand anonymous token');
```

2. Do we call signup if we tried to sign in via a social network, and the user is not signed up for the app? (true by
 default)

```
    this.backandService.setRunSignupAfterErrorInSigninSocial(true);
```

## Mobile

In 'src/app/app.component.ts':

```
    this.backandService.setIsMobile(true);
```

## CRUD

To fetch, create, and filter rows, from an object, say 'stuff', the CRUD functions in BackandService, should receive 'stuff' as their first argument

    getItems
    filterItems
    postItem

## Social Sign-up

The app opens a dialog supplied by the social network.

## Socket Service

* Socket login and logout are done automatically as part of the login and logout calls, respectively.

* To subscribe to event 'items_updated' from server side via sockets, call 'this.backandService.subscribeSocket' and in your controller, subscribe with:

```
    this.backandService.subscribeSocket('items_updated')
      .subscribe(
            data => {

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

Caches user details in the app. The 'force' parameter can cause it to fetch from it from Backand as in the call above.

## Backand Storage

### Create Backand Action

Create a server side action in Backand by going into the 'items' object actions tab and clicking on the Back& Files icon. Name your action 'files'

### File Upload

```
    backand.upload('items', 'files', fileName, base64Data).subscribe(
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
    backand.delete('items', 'files', fileName).subscribe(
        data => {
        },
        err => backand.logError(err),
        () => console.log('OK')
    );
```
