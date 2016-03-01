## Introduction
A common use case is separate development and production environments, often with several stages in between (testing, qa, staging), which allows phased deployment (rollout) and rollback in case of problems.The goal of Continuous Deployment is to enable a constant flow of changes from development to production.

The following explains the flow of deploying server-side and databases changes from one environment to the other.

## Deployment Steps
1. First, you need to sync model changes
    1. Go to your dev app and copy the JSON model ( Objects => Model => Model JSON) to the clipboard
    2. Go to your QA app and paste the JSON model in the Edit model pan.
    3. Click 'Validate & Update'

Now the schemas are synced.

 (If you get errors when applying the schema to the QA app, you'll need to fix them before you can upload the dev configuration)

2. Loading the dev configuration into the QA environment
    1. Go to Settings => Configuration
    2. Click 'Export Current Settings' button, which will download the app configuration zip file to your file system.
    3. In the QA app go to Settings => Configuration and click the 'Import External Settings' button to upload the configuration zip file you just downloaded from the dev app.
    4. In Settings => General click the 'Clear Cache' icon in the upper-right corner (swirly arrow refresh icon near the trashcan icon), and approve the popup message.

The QA app is now deployed with all the changes you made in the dev app.

Apply this process again when deploying QA to Production.

## Versions

On every change you make to your app, Backand saves a copy (version) of the app before the change.
You can use this feature to rollback, if needed.

To rollback to a previous version:

1. Go to Settings => Configuration
2. Click the 'Set' link which corresponds to the version you want to rollback to.

To save a backup of a version just click the 'Export' link, this will save a backup of the app configuration to your file system.

