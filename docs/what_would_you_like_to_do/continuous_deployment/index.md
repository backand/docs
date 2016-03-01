## Continuous Deployment (CD) and Versions


### Continuous Deployment
A common use case is where development environment and production environment are separated, often with several stages in between (testing, qa, staging), in order to allow phased deployment (rollout), testing, and rollback in case of problems.
The goal of Continuous Deployment is to enable a constant flow of changes from development into production .
Now days web apps usually contain 3 major components - client-side , server-side and a database.
The following steps will demonstrate the flow of deploying server-side and databases changes from one environment to the other.

1. First you need to sync the model changes
    1. Go to your dev app and copy the jsom model (Objects => model => Json model) to the clipboard
    2. Now go to the QA app and paste the json model
    3. Click "Validate & Update

Now the schemas are synced ( if you get errors when applying the schema to the qa app , you need to fix them before you can upload the dev configuration)

2. Now to load the dev configuration
    1. Go to Settings => configuration
    2. Click "Export Current Settings" button, this will download the app configuration zipped file to your file system. This file basically contains all your backand app ( actions, security ,queries...) so ,you can save it for backup also.
    3. On the QA app go to Settings => configuration and  click the "Import External Settings" button to upload the configuration zip file you just downloaded from the dev app.

The QA app is now deployed with all the changes you maid in the dev app.

The above steps should be applied the same way when you are deploying QA to Production.

### Versions

On every change you make to backand app we save a copy(version) of the app before the change.
You can use this feature to rollback when ever you need or where changes you've done cause errors.
To rollback to a previous version :
1. Go to Settings => Configuration
2. Click the Set link which corresponds to the version you wont to rollback to.

