## Backand Hosting & Deployment
High-performance, reliable and secure hosting for AngularJS applications.

Backand hosting built on AWS and it’s super-easy, effortlessly deploy your local Angular project files with the
hosted server.

Your project files will be available on https://hosting.backand.io/Your-App-Name

The Backand CLI (command-line interface) require Node.js and npm, which can both be installed by following the instructions on [https://nodejs.org](https://nodejs.org). Installing Node.js also installs npm.

#### First Time Installation

```bash

  $ npm install -g backand

  # or use sudo (with caution) if required by your system permissions
  # sudo npm install -g backand
```

#### Update previous Backand CLI

```bash

  $ npm update -g backand

  # or use sudo (with caution) if required by your system permissions
  # sudo npm update -g backand
```

#### Deploy the Angular Project

To deploy and sync your local Angular project folder, run this command:

```bash
  backand sync --app {{appName}} --master {{master-token}} --user {{user-token}} --folder /path/to/project/folder
```

  **--app**: The current app name
  
  **--master**: The master token of the app (get it from Social & Keys)
  
  **--user**: The token of the current user (get it from Team and click on key icon)
  
  **--folder**: The path of the local Angular folder to sync and deploy
  
  
## Configure Sync in Gulp

Sync and deploy a local folder to Backand hosting as part of (gulpjs)[http://gulpjs.com/].

To make the deployment work as Gulp task, follow these steps:

1. Install Backand hosting gulp plugin

```
  npm install backand-hosting-s3
```

2. At the top of the gulpfile.js add the `require`

```
  var backandSync = require('../sync-module');
```  

3. Set credentials. Credentials will be stored in file `.backand-credentials.json`:

```
  gulp.task('sts', function(){
      var masterToken = "your master backand token";
      var userToken = "your user backand token"; 
      return backandSync.sts(masterToken, userToken);
  });
```

4. Sync folder `./src`

```
  gulp.task('dist',['sts'], function() {   
      var folder = "./src";
      return backandSync.dist(folder, appName); //appName - if you have only one app you don't need this parameter
  });
```

4. Syncing is done via local cache file `.awspublish-<bucketname>`. Repeated add/delete of the same file may confuse it. To clean the cache do:

```
  gulp.task('clean', function() {
      return backandSync.clean();
  });
```