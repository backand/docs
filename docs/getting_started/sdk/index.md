## Overview

This is the documentation for Back&'s Angular 1 SDK. This SDK enables you to communicate comfortably and quickly with your Backand app.
It wraps the [vanilla-sdk](https://github.com/backand/vanilla-sdk) to allow you to work with Back& more easily when working on projects based on Angular.js 1.

## Installation

To install the Angular 1 SDK, use the correct command for your dependency management platform:

| Provider | Command |
| -------- | ------- |
| npm | `$ npm i -S @backand/angular1-sdk` |
| yarn | `$ yarn add @backand/angular1-sdk` |
| bower | `$ bower install backand-angular1-sdk` |
| clone/download via Git | `$ git clone $ git clone https://github.com/backand/angular1-sdk.git` |

## Import

Include the following tags in your `index.html` file to start working with the SDK:

``` html
<script src="node_modules/@backand/angular1-sdk/backand.provider.min.js"></script>
<script src="backand.provider.min.js"></script>
```

## Quick start

Getting started with the SDK is as simple as configuring access to a Back& application, then calling `getList` on a relevant object:

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

Review the full API reference at our [vanilla-sdk's github](https://github.com/backand/vanilla-sdk) to get started with your back end!

## Examples
***To view a demo of the SDK in action, just run npm start - [example page](https://github.com/backand/angular1-sdk/blob/master/example/).***
