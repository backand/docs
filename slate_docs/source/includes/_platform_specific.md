# Platform-specific SDKs
This section covers our platform-specific SDKs. These SDKs are optimized for the named platform, and wrap the [Vanilla SDK](#sdk-documentation) to provide access to Backand.

## Angular - Ionic 1 SDK
### Overview

This is the documentation for Back&'s Angular 1 SDK. This SDK enables you to communicate comfortably and quickly with your Backand app.
It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Angular.js 1.

### Installation

To install the Angular 1 SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/angular1-sdk |
| yarn | $ yarn add @backand/angular1-sdk |
| bower | $ bower install backand-angular1-sdk |
| clone/download via Git | $ git clone https://github.com/backand/angular1-sdk.git |

### Import
``` html
<script src="node_modules/@backand/angular1-sdk/backand.provider.min.js"></script>
<script src="backand.provider.min.js"></script>
```
Include the following tags in your *index.html* file to start working with the SDK:


### Quick start
```javascript
angular
  .module('myApp', ['backand'])
  .config(function (BackandProvider) {
    BackandProvider.init({
      appName: 'APP_NAME',
      anonymousToken: 'ANONYMOUS_TOKEN'
    });
  })
  .controller('myAppCtrl', ['$scope', '$http', 'Backand', function myAppCtrl() {

  }]);
```

Getting started with the SDK is as simple as configuring access to a Back& application, then calling *getList* on a relevant object.

Review the full API reference for our [Vanilla SDK](#vanilla-sdk) to get started with your back end!

### Examples
***To view a demo of the SDK in action, just run npm start - [example page](https://github.com/backand/angular1-sdk/blob/master/example/).***

## Angular - Ionic 2 SDK
### Overview

This is the documentation for Back&'s Angular 1 SDK. This SDK enables you to communicate comfortably and quickly with your Backand app.
It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Angular.js 1.

### Installation

To install the Angular 2 SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/angular2-sdk |
| yarn | $ yarn add @backand/angular2-sdk |

### Import
Use the following import statement to include the Angular2 SDK in your project:

```javascript
import { BackandService } from '@backand/angular2-sdk'
```

### Quick start

Using the Back& Angular2 SDK requires two steps - configuring access to the BackandService provider, and then actually calling the provider using the [vanilla-sdk](https://github.com/backand/vanilla-sdk) methods.

#### app.module.ts:
```javascript
@NgModule({
  imports: [ ... ],
  declarations: [ ... ],
  providers: [ BackandService ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

Update *app.module.ts* to include the BackandService as a provider

#### app.component.ts:
```javascript
@Component({
  selector: 'my-app',
  template: `<h1>Hello angular2-sdk</h1>
    <h3>{{res}}</h3>`
})
export class AppComponent implements OnInit {
  res: string;
  constructor(private backand: BackandService) { }
  getList(): void {
    this.res = 'fetching objects...';
    this.backand.object.getList('users').then((res: any) => {
      this.res = `${res.data.length} objects fetched`;
      console.log(res);
    })
  }
  ngOnInit(): void {
    this.backand.init({
      appName: 'APP_NAME',
      anonymousToken: 'ANONYMOUS_TOKEN'
    });
    this.getList();
 }
}
```

Now, call the SDK from *app.component.ts*. The SDK is initialized during *ngOnInit()*, and *getList* is called as a property on *AppComponent*

Review the full API reference for our [Vanilla SDK](#vanilla-sdk) to get started with your back end!

### Examples
***To view a demo of the SDK in action, just run npm start - [example page](https://github.com/backand/angular1-sdk/blob/master/example/).***

## Redux - React SDK
### Overview

This is the documentation for Back&'s Redux SDK. This SDK enables you to communicate comfortably and quickly with your Backand app.
It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Redux.

### Installation

To install the Redux SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/redux-sdk |
| yarn | $ yarn add @backand/redux-sdk |

### Import
```javascript
import { BackandService } from '@backand/angular2-sdk'
```

Use the following import statement to include the Angular2 SDK in your project:


### Quick start
```bash
$ "./node_modules/.bin/bkndredux" --help
$ "./node_modules/.bin/bkndredux" user obj1 obj2 obj3... -m (thunk/saga)
```  

To get started, first use *bkdnredux* to generate the necessary *Types*, *Actions*, and *Reducers* for your Backand objects from the command line:

***Note:*** *user* is a unique object. It has a different *Reducer* and *Type*, and it reveals most of the authentication *Actions* (getUserDetails, signin, signout, etc.).

```javascript
import { combineReducers } from 'redux'
import user from './user/userReducer'
import obj1 from './obj1/obj1Reducer'
import obj2 from './obj2/obj2Reducer'

combineReducers({
  user,
  obj1,
  obj2
})
```

Next, Include the new *Reducers* in [combineReducers()](http://redux.js.org/docs/api/combineReducers.html) (see JavaScript tab)


Next, we nede to configure the middleware stack to work with the SDK. Pick the section that applies to your chosen middleware stack - either [redux-thunk](https://github.com/gaearon/redux-thunk) or [redux-saga](https://github.com/redux-saga/redux-saga) - then follow the corresponding instructions:

#### [redux-thunk](https://github.com/gaearon/redux-thunk)
Download [redux-thunk](https://github.com/gaearon/redux-thunk) and include it in [createStore()](http://redux.js.org/docs/api/createStore.html):
```javascript
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'

createStore(rootReducer, initialState, applyMiddleware(thunk))
```

#### [redux-saga](https://github.com/redux-saga/redux-saga)
Download [redux-saga](https://github.com/redux-saga/redux-saga) and include it in [createStore()](http://redux.js.org/docs/api/createStore.html):
```javascript
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'
import rootSaga from './sagas.js'

const sagaMiddleware = createSagaMiddleware()
createStore(rootReducer, initialState, applyMiddleware(sagaMiddleware))
sagaMiddleware.run(rootSaga)
```

#### Working with the SDK
```javascript
import { getUserDetails, signin, useAnonymousAuth, signout } from './user/userActions'

store.dispatch(signin(username, password))
store.dispatch(getUserDetails())
```

Once the above is completed, import `Actions` and dispatch them easily!

Review the full API reference for our [Vanilla SDK](#vanilla-sdk) to get started with your back end!

### Examples
Below are a few examples of Redux code that uses the Back& SDK:
- [codepen example (thunk)](http://codepen.io/backand/pen/VmRajE)
- [codepen example (saga)](http://codepen.io/backand/pen/pRgqyx)
- [react-native-example](https://github.com/backand/react-native-example/tree/sdk)
