## Overview

This is the documentation for Back&'s Redux SDK. This SDK enables you to communicate comfortably and quickly with your Backand app.
It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Redux.

## Installation

To install the Redux SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | $ npm i -S @backand/redux-sdk |
| yarn | $ yarn add @backand/redux-sdk |

## Import
Use the following import statement to include the Angular2 SDK in your project:

```javascript
import { BackandService } from '@backand/angular2-sdk'
```

## Quick start
To get started, first use *bkdnredux* to generate the necessary *Types*, *Actions*, and *Reducers* for your Backand objects from the command line:

```bash
$ "./node_modules/.bin/bkndredux" --help
$ "./node_modules/.bin/bkndredux" user obj1 obj2 obj3... -m (thunk/saga)
```  
***Note:*** *user* is a unique object. It has a different *Reducer* and *Type*, and it reveals most of the authentication *Actions* (getUserDetails, signin, signout, etc.).

Next, Include the new *Reducers* in [combineReducers()](http://redux.js.org/docs/api/combineReducers.html):
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

For the next step, pick the section that applies to your chosen middleware stack - either [redux-thunk](https://github.com/gaearon/redux-thunk) or [redux-saga](https://github.com/redux-saga/redux-saga) - then follow the corresponding instructions:

### [redux-thunk](https://github.com/gaearon/redux-thunk)
Download [redux-thunk](https://github.com/gaearon/redux-thunk) and include it in [createStore()](http://redux.js.org/docs/api/createStore.html):
```javascript
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'

createStore(rootReducer, initialState, applyMiddleware(thunk))
```

### [redux-saga](https://github.com/redux-saga/redux-saga)
Download [redux-saga](https://github.com/redux-saga/redux-saga) and include it in [createStore()](http://redux.js.org/docs/api/createStore.html):
```javascript
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'
import rootSaga from './sagas.js'

const sagaMiddleware = createSagaMiddleware()
createStore(rootReducer, initialState, applyMiddleware(sagaMiddleware))
sagaMiddleware.run(rootSaga)
```

### Working with the SDK
Once the above is completed, import `Actions` and dispatch them easily!

```javascript
import { getUserDetails, signin, useAnonymousAuth, signout } from './user/userActions'

store.dispatch(signin(username, password))
store.dispatch(getUserDetails())
```

Review the full API reference at our [vanilla-sdk's github](https://github.com/backand/vanilla-sdk) to get started with your back end!

## Examples
Below are a few examples of Redux code that uses the Back& SDK:
- [codepen example (thunk)](http://codepen.io/backand/pen/VmRajE)
- [codepen example (saga)](http://codepen.io/backand/pen/pRgqyx)
- [react-native-example](https://github.com/backand/react-native-example/tree/sdk)
