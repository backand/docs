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

## CRUD

To fetch, create, and filter rows, from an object, say 'items', the CRUD functions in BackandService, should receive 'items' as their first argument:

```

  //1. Read
  this.backandService.getItems('items')
      .subscribe(
          data => {
          },
          err => this.backandService.logError(err),
          () => console.log('OK')
      );

  //2. Create
  this.backandService.postItem('items', this.name, this.description)
      .subscribe(
          data => {
          },
          err => this.backandService.logError(err),
          () => console.log('OK')
      );

  //3. Query

  //When `q` is set to your search pattern,
  this.backandService.filterItems('items', q)
      .subscribe(
          data => {
              console.log('subscribe', data);
              this.items = data;
          },
          err => this.backandService.logError(err),
          () => console.log('OK')
      );

```

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

```

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
