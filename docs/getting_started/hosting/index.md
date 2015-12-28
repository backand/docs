## Hosting & Deployment
Backand offers high-performance, reliable, and secure hosting for AngularJS applications through the hosting and deployment tool. Built off of AWS, Backand Hosting gives you an effortless way to deploy your Angular project to a hosted server.

Your project files will be available on **https://hosting.backand.io/Your-App-Name**, providing you an easy way to see what gets deployed for your project.


*Note*: The Backand CLI (command-line interface) requires Node.js and Node Package Manager (NPM). To install Node and NPM, follow the instructions at [https://nodejs.org](https://nodejs.org). This one process handles both NodeJS and NPM.

#### First Time Installation

The hosting and deployment functionality is provided with the Backand node package. Simply install the package from the command line as follows:

```bash

  $ npm install -g backand

  # or use sudo (with caution) if required by your system permissions
  # sudo npm install -g backand
```

#### Updating a Prior Version

If you've installed the Backand node package before, you will need to perform an update to get the new hosting and deployment functionality. Follow these steps to update your locally-installed version of Backand:

```bash

  $ npm update -g backand

  # or use sudo (with caution) if required by your system permissions
  # sudo npm update -g backand
```

#### Deploy the Angular Project

The Backand CLI, which we installed using NPM, provides the capability to deploy your project, as well as sync your local project folder. To deploy and sync, use the following command from the command line:

```bash
  backand sync --app {{appName}} --master {{master-token}} --user {{user-token}} --folder /path/to/project/folder
```

The parameters for this call are:

  **--app**: The current app name
  
  **--master**: The master token of the app (get it from Social & Keys)
  
  **--user**: The token of the current user (get it from Team and click on key icon)
  
  **--folder**: The path of the local Angular folder to sync and deploy
  
#### Browse to your app
Your app is now on air, just browse to **https://hosting.backand.com/YourAppName** and see it live.

## Configure Sync in Gulp

Backand can integrate with your existing [gulpjs](http://gulpjs.com) configuration, allowing you to easily control deployment as you would the rest of your project. Below, we'll create a Gulp task to perform the deployment. This can then be used to deploy your application as a part of your standard build process.

To create the new Gulp deployment task:

1. Install the Backand hosting gulp plugin using NPM:

```
  npm install backand-hosting-s3
```

2. At the top of gulpfile.js add the `require` call below:

```
  var backandSync = require('../sync-module');
```  

3. Set your Backand credentials for the task. Credentials will be stored in file `.backand-credentials.json`:

```
  gulp.task('sts', function(){
      var masterToken = "your master backand token";
      var userToken = "your user backand token"; 
      return backandSync.sts(masterToken, userToken);
  });
```

4. Configure the task to Sync folder `./src`:

```
  gulp.task('dist',['sts'], function() {   
      var folder = "./src";
      return backandSync.dist(folder, appName); //appName - if you have only one app you don't need this parameter
  });
```

4. Syncing is performed via a local cache file named `.awspublish-<bucketname>`. This file cache can become corrupted after multiple uses, so we need to create one more task to perform cleanup after the deployment has completed:

```
  gulp.task('clean', function() {
      return backandSync.clean();
  });
```
