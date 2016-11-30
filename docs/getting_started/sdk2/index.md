This section covers [Backand's Angular 2.x SDK 2.3.x](https://github.com/backand/angular2bknd-sdk)

### Getting Started

The Backand Angular 2 SDK compatible with AngularJS 2.0.x and provides methods for easily communicating with the Backand server and performing common tasks, such as managing users. To use the SDK, follow these steps:

## install

```

    npm install angular2bknd-sdk --save

```

## Dependencies

```

    npm install @types/node --save-dev
    npm install @types/socket.io-client --save-dev

```

## Import

In 'src/app/app.module.ts':

```

    import { BackandService } from 'angular2bknd-sdk';

```

Add 'BackandService' to the 'providers' array. In each component where you use Backand, import it:

```

    import { BackandService } from 'angular2bknd-sdk';

```

In the constructor use it as 'this.backandService':

```

    constructor(private backandService:BackandService){}

```


## Set Your App Details

In 'src/app/app.component.ts':

```

    this.backandService.setAppName('your app name');
    this.backandService.setSignUpToken('your backand signup token');
    this.backandService.setAnonymousToken('your backand anonymous token');

```

## Mobile

In 'src/app/app.component.ts':

```

    this.backandService.setIsMobile(true);

```

## Sign-in / Sign-up

Sign-in with username and password in order to get access_token to be used in all other calls. If you don't have users
 you can use anonymous access. Use this code for signin:

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

In sign-up you must provide the basic details of username email, first name, last name and password:

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

Sign-out will clear the local storage and will invalidated the access_token:

```
    this.backandService.signout();

```

## Social Sign-in / Sign-up

For social, just call sign-in and by default the user will be signed up if needed. The app opens a dialog supplied by
the social network.

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

## CRUD

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

and call 'filterItem'

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
