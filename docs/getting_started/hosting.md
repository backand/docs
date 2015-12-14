## Backand Hosting & Deployment
High-performance, reliable and secure hosting for AngularJS applications.

Backand hosting built on AWS and it’s super-easy, effortlessly deploy your local Angular project files with the
hosted server.

Your project files will be available on https://hosting.backand.io/Your-App-Name

The Backand CLI (command-line interface) require Node.js and npm, which can both be installed by following the instructions on [https://nodejs.org](https://nodejs.org). Installing Node.js also installs npm.

### First Time Installation

```bash

  $ npm install -g backand

  # or use sudo (with caution) if required by your system permissions
  # sudo npm install -g backand
```

### Update previous Backand CLI

```bash

  $ npm update -g backand

  # or use sudo (with caution) if required by your system permissions
  # sudo npm update -g backand
```

### Deploy the Angular Project

To deploy your local Angular project run this command:

```bash
  backand sync --app {{appName}} --master {{master-token}} --user {{user-token}} --folder /path/to/project/folder
```

  **--app**: The current app name
  **--master**: The master token of the app (get it from Social & Keys)
  **--user**: The token of the current user (get it from Team and click on key icon)
  **--folder**: The path of the local Angular folder to sync and deploy