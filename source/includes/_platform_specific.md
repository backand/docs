# Platform-specific SDKs
This section covers our platform-specific SDKs. These SDKs are optimized for the named platform, and wrap the [Vanilla SDK](#vanilla-sdk) to provide access to Backand.

## Angular - Ionic 1 SDK
### Overview

This SDK enables you to communicate comfortably and quickly with your Backand app using AngularJS and Ionic Framework. It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Angular.js 1.

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

### Examples for Angular/Ionic 1
***To view a demo of the SDK in action, just run npm start - [example page](https://github.com/backand/angular1-sdk/blob/master/example/).***

## Angular - Ionic 2 SDK
### Overview

This SDK enables you to communicate comfortably and quickly with your Backand app using Angular 2 and the Ionic Framework version 2. It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Angular.js.

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

### Examples for Angular/Ionic 2
***To view a demo of the SDK in action, just run npm start - [example page](https://github.com/backand/angular2-sdk/blob/master/example/).***

***We also offer an example app for Angular 2 - just follow the instructions in our [sample project](https://github.com/backand/angular2-example)***

## Redux - React SDK
### Overview

This SDK enables you to communicate comfortably and quickly with your Backand app when building on top of React or Redux. It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Redux.

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


After this, we need to configure the middleware stack to work with the SDK. Pick the section that applies to your chosen middleware stack - either [redux-thunk](https://github.com/gaearon/redux-thunk) or [redux-saga](https://github.com/redux-saga/redux-saga) - then follow the corresponding instructions:

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

### Examples for React/Redux
Below are a few examples of Redux code that uses the Back& SDK:
- [codepen example (thunk)](http://codepen.io/backand/pen/VmRajE)
- [codepen example (saga)](http://codepen.io/backand/pen/pRgqyx)
- [react-native-example](https://github.com/backand/react-native-example/tree/sdk)

## Vue.js
Vue.js is a progressive framework designed to provide an intuitive means by which a front-end developer can build their application. It is designed to be incrementally adaptable, allowing you to use it as often as you like, and provides a lightweight set of tools for adding dynamic functionality to a web app. By providing dynamic backing to the view model, you can easily make a Vue.js-based application into a large and feature-rich web app. In this section, we'll look at how we can use Backand's Vanilla SDK in a Vue.js-based application to provide the backing for that data service.

Note: This example is also available on [Codepen](http://codepen.io/backand/pen/ZLwwZg/?editors=1010)

###Including and Initializing the SDK
```html
<!-- Include the Back& SDK -->
<script type="text/javascript" src="https://cdn.backand.net/vanilla-sdk/1.1.2/backand.min.js"></script>
```

The first step in connecting your Vue.js app to Backand is to import the JavaScript file containing the Backand SDK. You can either do this through a package manager, or via our CDN.

```javascript--persistent
var myApp = new Vue({
  // Other constructor code here
  beforeMount: () => {
    backand.init && backand.init({
      appName: 'reactnativetodoexample',
      signUpToken: '4c128c04-7193-4eb1-8f19-2b742a2a7bba',
      anonymousToken: '2214c4be-d1b1-4023-bdfd-0d83adab8235'
    });
  },
  // Other constructor code here
});
```
Once you've included the SDK, you'll have access to the functionality via the global object `backand`. You'll next use this to initialize our connection to Backand in the constructor for your app's Vue instance. We recommend putting the initialization in the `beforeMount` lifecycle hook, but any location or hook that initializes the SDK before it is used should be sufficient:

With that, the Backand SDK is initialized. This post uses a pre-built demo app, `reactnativetodoexample`, and its associated keys. If you wish to connect this demo to your own Backand application, simply replace `appName`, `signUpToken`, and `anonymousToken` with the correct values from your Backand application's dashboard. Aside from updating the data model to include the `ToDo` object, the only thing you will need to do to ensure your app operates in the same way is to add the relevant custom actions when editing a `ToDo` item (see below).

###Loading the object data
>Next, you need to define a global method that knows how to load the object data from your Backand application. To do this, first define an empty data member to store the future results from the API:

```javascript--persistent
var myApp = new Vue({
  // Other constructor code here
  data: {
    todos: []
  },
  // beforeMount: () => { ...
});

```
>This data member will hold the list of ToDo items obtained from the server. Next, we'll define a global method to populate this data member:

```javascript--persistent
var myApp = new Vue({
  // Other constructor code here
  methods: {
    fetchTodos: function() {
      this.todos = [];
      let params = {
        sort: [backand.helpers.sort.create('creationDate', backand.helpers.sort.orders.desc)],
        exclude: backand.helpers.exclude.options.all,
        pageSize: 1000000,
        pageNumber: 1,
      }
      backand.user.getUserDetails().then(res=> {
        if (res.data != null && res.data.userId) {
          params.filter = [backand.helpers.filter.create('user', backand.helpers.filter.operators.relation.in, user.data.userId)];
        }
        return backand.object.getList('todos', params);
      }).then(res=> { this.todos = res.data }).catch(error);
    }
  }
  // Other constructor code here
});
```

This function first clears out the `todos` data member, then defines a set of filter params using helper methods in the SDK (refer to our documentation for more information). Then, it calls `backand.user.getUserDetails()` to determine if the list should be restricted to the currently logged-in user. Finally, the method calls `backand.object.getList()` to fetch the list of objects in the SDK, and then updates the todos data container in the promise resolution block.
> Now, you simply need to call `fetchTodos()` in the appropriate location in the Vue lifecycle. To load the data when mounting is complete, you can add it to the `mounted` handler like so:

```javascript--persistent
var myApp = new Vue({
  // Other constructor code here
  mounted: function() {
    this.fetchTodos();
  },
  // Other constructor code here
});
```



###Creating, Editing, and Deleting Objects
>The following code is a handler for an x-template component titled `todo-form`, which is used to create a new `ToDo` entry in the application:

```javascript--persistent
Vue.component('todo-form', {
  template: '#todo-form',
  data: () => {
    return {
      input: ''
    }
  },
  methods: {
    create: function() {
      // console.log(this.input)
      if (this.input) {
        backand.user.getUserDetails().then(res=> {
          return backand.object.create(objectName, {
            text: this.input,
            creationDate: (new Date()).toISOString(),
            completionDate: null,
            user: res.data.userId
          });
        }).then(res=> this.input = '').catch(error);
      }
    }
  }
})

```
Once you've got the basic display up and running, you'll want to add methods to create, update, and delete the ToDo items based on user actions.

Following a similar pattern to the initial load, this code first checks for a user context. If a user is not logged in, this call will have a value of `null`. Otherwise, we can use the `userId` of the active user to assign ownership of the new `ToDo` item. The code then calls `backand.object.create()` to create the record in your Backand application.
> You can follow a similar pattern when updating an object:

```javascript--persistent
const todoItem = {
  props: {
    'todo': Object
  },
  template: '#todo-item',
  methods: {
    update: function() {
      backand.object.update(objectName, this.todo.id, Object.assign({}, this.todo, {
        completionDate: this.todo.completionDate ? null : (new Date()).toISOString()
      })).catch(error);
    },
    remove: function() {
      backand.object.remove(objectName, this.todo.id).catch(error);
    }
  }
};
```

This function demonstrates how to update a `ToDo` item's completion date using `backand.object.update()` - simply provide the object name, the object ID, and the collection of changes to be made. The above method also provides deletion functionality with the remove method. Simply provide the object name and the ID of the `ToDo` item to delete the entry from your database.

###Keeping the app up-to-date
>First, create the appropriate Custom JavaScript actions in your Backand app's dashboard, and have them emit an event titled "update_todos" as follows:

```javascript--persistent
socket.emitAll("update_todos", {});
```

While you now have a fully-functional CRUD interface for the `todo` object, you have not yet built a way for your application to receive - and respond to - updates. You can accomplish this using Backand's Realtime Communications capability.

This will send a socket message to all connected clients. At the moment we do not provide any parameters to this call (just an empty object - `{}`), but you can replace this with any of the variables available to your custom action, such as the `dbRow` that was just modified.
>First, include the Socket.io library in the appropriate location in your code:

```html
  <script type="text/javascript" src="https://cdn.backand.net/socket.io/1.4.5/socket.io.js"></script>
```
Now that you have a set of actions designed to announce changes to your `ToDo` database object, we need to configure the client side to receive socket communications.

> Once that's done, we need to set the SDK to enable socket mode:

```javascript--persistent
var myApp = new Vue({
  // Other constructor code here
  beforeMount: () => {
    backand.init && backand.init({
      appName: 'reactnativetodoexample',
      signUpToken: '4c128c04-7193-4eb1-8f19-2b742a2a7bba',
      anonymousToken: '2214c4be-d1b1-4023-bdfd-0d83adab8235',
      runSocket: true
    });
  },
  // Other constructor code here
});
```
>Finally, you need to add an event handler. Update your mounted handler to include the event handler function as follows:

```javascript--persistent
var myApp = new Vue({
  // Other constructor code here

  mounted: function() {
    this.fetchTodos();
    backand.on('update_todos', (data) => {
      this.fetchTodos();
    });
  },
  // Other constructor code here
});
```

This function handles all update_todos events the same way - by reloading the entire list from the server. You can use a similar technique to write different handlers for each database operation, or to perform other types of logic based on your application's needs.

###Conclusion
With the above code, you now have a simple data service that you can use to update your Vue.js app's user interface with results from a Backand application. You can create, update, and delete records at will in a responsive manner, no server code required. With Backand, you can accelerate your Vue.js development, focusing on what makes your app unique, and leave the server side to us.

## jQuery
JQuery is the most widely-deployed JavaScript library. It provides a comprehensive set of responsive tools for dynamically working with an HTML page's DOM, and is responsible for establishing and guiding a lot of the patterns that are the standard in web development. In this section, we'll look at how you can integrate the Back& Vanilla SDK with an app built with JQuery.

Note: The code for this section is also available on [CodePen](http://codepen.io/backand/pen/VPRpBN?editors=1010)
### Including and Initializing the SDK
```html
<!-- Include the Back& SDK -->
<script type="text/javascript" src="https://cdn.backand.net/vanilla-sdk/1.1.2/backand.min.js"></script>
```
The first step in connecting your JQuery-based web app to Backand is to include the JavaScript file containing the Backand SDK. You can either do this through a package manager, or via our CDN.

###Creating the Service
```javascript--persistent
var dataService = {
  init: function(){
    backand.init(
      {appName: 'reactnativetodoexample',
       signUpToken: '4c128c04-7193-4eb1-8f19-2b742a2a7bba',
       anonymousToken: '2214c4be-d1b1-4023-bdfd-0d83adab8235',
       runSocket: false
      }
    );   
  },
  // Other code here
};
```
Once you've included the SDK, you'll have access to the functionality via the global object `backand`. To integrate Back& with a JQuery-based web app, we'll make use of a global service variable that will wrap the SDK. This service will handle initialization of the sdk, and provide all of the tools necessary to work with a Backand application. We'll start by declaring the service and initializing Backand with the following code:

This defines an object - `dataService` - that can be used to interact with the Backand SDK. In the data service, we define a method - `init` - that is used to initialize the SDK's connection to a Backand application. Simply call `dataService.init()` at the appropriate location in your app's initialization code, and the SDK is ready to use (in the example, we do this in the $(document).ready handler, but this is not required).

This content uses a pre-built demo app, `reactnativetodoexample`, and its associated keys. If you wish to connect this demo to your own Backand application, simply replace `appName`, `signUpToken`, and `anonymousToken` with the correct values from your Backand application's dashboard. Aside from updating the data model to include the `ToDo` object, the only thing you will need to do to ensure your app operates in the same way is to add the relevant custom actions when editing a `ToDo` item (see below).
### Fetching and Manipulating the Data
```javascript--persistent
var dataService = {
  //other code here
  getList: function(){
        var params =  {
          sort: backand.helpers.sort.create('creationDate', backand.helpers.sort.orders.desc),
          exclude: backand.helpers.exclude.options.all,
          pageSize: 10,
          pageNumber: 1,
          filter: backand.helpers.filter.create('completionDate', backand.helpers.filter.operators.date.empty, '')
        };
        return backand.object.getList('todos',params);
  },
  // Other code here
};
```
Now that our app is wired up to Backand, we'll want to start writing code to work with our Backand application's objects. We can retrieve a list of `todo` objects from the server by adding a property to the `dataService` object, `getList`, that calls the appropriate SDK method

This function first defines a set of filter params using helper methods in the SDK (refer to our documentation for more information). It then calls `backand.object.getList()` to fetch the list of objects in the SDK. The result of this call - a promise - is then passed back to the calling code. The calling code can then create its own success and failure handlers, using `.then` and `.catch`, and update internal state appropriately.
>You can use this technique to wrap every call in the Backand SDK, providing your JQuery code with full access to the server.

```javascript--persistent
var dataService = {
  //other code here
  create:function(text){
    return backand.object.create('todos',
          {"text":text,"creationDate":new Date()});
  },
  // Other code here
};
```
From this point onward, working with the SDK is a matter of writing wrapper methods in the `dataService` object for each SDK method with which you wish to interact. Backand provides interfaces for all of the common database manipulation tasks. For example, the call in the sidebar defines a method - `create` - that creates a new entry in the database.


###Handling Responses
```javascript--persistent
var loadList = function(){
  dataService.getList().then(function(response){
// call a helper function to populate results in our UI
      $.each(response.data, function(i, todo){
        appendTodo(todo.text,todo.id);
      })
   })
}
```

All Backand SDK methods return a promise, performing the tasks requested asynchronously. To handle the responses, we simply need to write the appropriate handlers for the `.then` and `.catch` calls. In the code to the side, for example, we write a simple set of handlers for the `getList` call.

As JQuery is a very flexible framework, it is hard to provide concrete guidance on exactly where SDK should take place in terms of the program's execution. The CodePen example demonstrates one method of creating this component, but you do not need to mimic the code structure there exactly. The key component is to ensure that `backand.init()` is called prior to any SDK calls taking place, otherwise the calls from the SDK will fail.

###Conclusion
With the above code, you now have a simple data service that you can use to update your JQuery-based app's user interface with results from a Backand application. You can make full use of the SDK, with the capability to create, update, and delete records at will in a responsive manner, no server code required. With Backand, you can accelerate your JQuery front-end development, focusing on what makes your app unique, and leave the server side to us.
