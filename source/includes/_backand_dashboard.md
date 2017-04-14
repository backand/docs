# Backand Dashboard

Backand's REST API allows you to fully access and manipulate the backend data for your system, giving you full access to the underlying database and related functionality from any program that can send an HTTP request. While we start by wrapping your database in a true RESTful API Interface, we also give you a lot of extras that you can use to replace the server in your app:

* You can define and execute custom queries that are triggered by a call to a URL
* You can define and execute custom actions that execute whenever you hit a specific endpoint. These can be as simple as sending an email, or as complex as a custom server-side javascript function.
* You can manage user security and access to your application automatically through Backand's security and authorization system
* You can test every single endpoint in your system using Backand's API Playground
* You can get immediate and up-to-date looks at your database's underlying tables and stored data, and even perform real-time manipulations of that data
* You can create, retrieve, update, and delete every object in your system through a simple web call, and return as much - or as little - detail as you need

Let's take a quick look at each available action in the Backand dashboard, and get started with focusing on the front-end of your application!

## Docs & API
The Docs & API tab contains links to kickstart tutorials, tutorials on converting existing applications to use Backand, and other useful documentation. let's take a look at each menu category below.
### Examples
This links to our [examples page](https://www.backand.com/apps/), which has useful sample applications for many different platforms. These sample applications work to show how best to integrate Backand with an application for each mentioned platform, and can be a good starting point for integrating Backand with an existing (or brand new) application.

### Kickstart Tutorials
Our kickstart tutorials provide a list of kickstart apps that you can use to build an app for the platform specified. These kickstart apps ship with Backand already built-in and integrated, and provide you with tools to explore the SDK's functionality. They all work with the default data model, so getting the kickstart app wired up to a new Backand application is a simple matter of changing the appropriate keys in each example.

<aside class='notice'>
We provide a platform selector, which allows you to only view the kickstarts, existing apps, and code samples throughout the dashboard that are relevant to your needs. Simply choose a different platform, and the dashboard will present all examples in the relevant version of JavaScript that matches your selection.
</aside>

### Existing App Tutorials
Our existing app tutorials provide a list of tutorials on connecting Backand into your existing application, with a number of different frameworks supported. These focus on including the Backand SDK, connecting to your app, and working with the data model. If you'd like to see another platform included, please [contact us](https://www.backand.com/contact/)!

<aside class='notice'>
We provide a platform selector, which allows you to only view the kickstarts, existing apps, and code samples throughout the dashboard that are relevant to your needs. Simply choose a different platform, and the dashboard will present all examples in the relevant version of JavaScript that matches your selection.
</aside>
### Realtime Database
The Realtime Database tab provides documentation on configuring your Backand application to work with our Socket-based real-time communications capabilities. It outlines all of the steps necessary on both the client and server sides to get everything working in your app.

<aside class="notice">Backand's realtime functionality relies upon Socket.io. We choose not to include this library by default as the JavaScript library itself is quite large, and can greatly increase the size of your application's assets. Make sure that when you enable socket functionality, that you have also included the Socket.io javascript in your project. </aside>

### Documentation
The documentation item has a link to this documentation.

### REST API Playground
The REST API playground is a Swagger-based page from which you can exercise all of the SDK functionality. Use this to gain familiarity with the Backand REST API.

## Objects
The "Objects" section of the dashboard allows you to modify the specific objects in your application's database. A menu entry is created for each table in your database, and allows you to configure your application at the object level. For each object in your system, you are given access to the following areas to modify and work with.

### Model
> This sample model JSON creates two objects - Items and Users

```json
[
  {
    "name": "items",
    "fields": {
      "name": {
        "type": "string"
      },
      "description": {
        "type": "text"
      },
      "user": {
        "object": "users"
      }
    }
  },
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "firstName": {
        "type": "string"
      },
      "lastName": {
        "type": "string"
      },
      "items": {
        "collection": "items",
        "via": "user"
      }
    }
  }
]
```

The Model parent tab allows you to modify your database's model structure, adding new objects and establishing the relationship between them. This can be done using either our GUI model editor, or the JSON schema editor. We also provide a tab demonstrating how to make - and sync - changes to your database using an external database tool (like MySQL Workbench). Below is a brief overview of how to use this interface.

<aside class="notice">Backand offers support for automated population of created and updated timestamps. Simply add the `createdAt` or `updatedAt` fields to your model, and if they are present Backand will automatically populate them with the timestamp of the most recent POST or PUT request to that object.</aside>

<aside class="warning">In addition to the fields supplied by the user, Backand defines an 'id' field, of type integer, which will be used as a primary key for the table. Backand uses auto-incrementing integers as the default for IDs in your objects, meaning in the default case values provided in the ID field by the SDK or via another API call will be overwritten with the next sequential ID. You'll need to remove this constraint in order to successfully populate the ID field with other data. You can also use an alternative field in the object as the primary key - simply make the designation in SQL, and Backand will update once you sync the database changes.</aside>

#### Model Diagram
The Model Diagram tab provides a GUI editor for working with your app's database schema. It works with the model JSON in the background to orchestrate changes to your database. Using the GUI you can easily add new objects, update existing objects, create relations between objects, and all other related database modification tasks.

#### Model JSON
The Model JSON tab provides a straightforward method of editing your database schema using well-formed JSON. You're given a pane to perform your edits, and a comparison pane so that you can easily see what has changed from the current schema. Once your changes are finalized, click "Validate & Update" to finalize your changes.

#### Database (pending)
This tab describes how to make the changes you desire in your model directly in the database. Once you've made your changes via whichever RDBMS tool you prefer, you can click the "Sync your database" button to pull the changes into Backand, updating your schema and object list.

### Object List
For each object in your schema, you are provided a dedicated management area for that object. These are accessed via a list beneath the "New object" entry.

Each object has a set of tabs provided that allow you to modify either the object's data, or the object itself. These tabs are Data, Actions, Security, Settings, and REST API

<aside class="notice">You can also use the "New object" to quickly create a new object to add to your system.</aside>
#### Data tab
The data tab provides you with a quick method by which you can add new records, update existing records, or remove records from your database. For every action you take in this pane, we provide the code needed to reproduce the action using the SDK - simply select the appropriate language/JavaScript framework for your project, and copy the code into your application.

We also give you access to the Filter Data functionality, which can be used to quickly construct request filters that can be applied to GET requests. Simply click on the "Filter Data" link to expose a UI for constructing your filter, and the resulting parameter code will appear in the "Request Code" textbox on the right side of the screen.

#### Actions tab
The Actions tab allows you to work with custom actions in your object. Custom actions allow you to perform automated tasks in your Backand application. You can create actions that execute at any point during the database transaction context for an object, or create actions that execute whenever a specific URL is hit. See [our documentation on custom actions](#custom-actions) for more information.

#### Security tab
The security tab allows you to create custom filters that apply restrictions to loading data associated with the user session. You can also use this tab to override your app's global security configuration, allowing custom access to your object based on user role, and also restrict access to specific fields in the object as necessary.

#### Settings tab
The Settings tab allows you to modify properties of your object directly, and work with each field in the object. At the object level, you're given the capability to enable change tracking, or to set a descriptive column. Change tracking keeps a record of all changes to an object as they occur, storing these results in App Dashboard -> Log -> Data History. This can result in performance impacts, so you should use this feature sparingly. A descriptive column represents the object when it is retrieves as a part of a list, providing the user with an easy-to-understand indicator for the current object record they are manipulating.

The field properties provide you with the capability to make a field searchable (available via the "Search" filter when making API calls), mark a field as "required", or provide a default value for a field. These options are presented for each field in your object, but depending on usage may not be modifiable. If a field represents a link to another object in your system, you are provided with a hyperlink in the settings section that takes you directly to that related object's datagrid view.

#### REST API tab
The REST API tab offers you the capability to test all of the standard database actions with a custom Swagger interface. Once you execute the call, the request that was made - and the response received - are both provided, allowing you to quickly adopt the action into your app.

## Queries
The queries tab provides you with the capability to generate custom queries in your system. Simply click "New Query" to start building out a custom query. When building a new query you can provide a name, input parameters, and the query body using either NoSQL or SQL syntax. You can also set the security template of the query, restricting the query access based on the user's role. On the right side of this screen, you can also perform a quick test of your query by hitting either "Test Query" or "Save & Test". The test process will present the query's URL endpoint, the SDK code needed to call the query, and a sample response.

### Query List
As you create queries, you're provided with a list of the queries present in your system. Simply click on the name of a query to begin editing or testing the query's contents.

### Create a query
You can add a new query by clicking on "+New Query" in the menu bar. This allows you to name the query, provide any parameters necessary, and then write the query. It also allows you to set a specific security template for the query to use as it executes, and allows you to override the security template settings when running the query.

### Update an existing query
To update an existing query, select the query from the navigation menu and press the "Edit" button.

### Test a query
You can test both new and existing queries using the Test query parameters panel on the right hand side of the page. This executes the query, returning both the data of the query and the URL used to execute the query.

## Background Jobs
Background jobs (also known as cron jobs) allow you to run an action, query, or external URL request on a schedule that you define. Simply set the time and frequency, and the desired task will be executed automatically by Backand's servers. With this mechanism, you can run periodic queries for things like reports, recurring actions to control your application's behavior, or regularly post to external services.

### Create a job
To create a new Background job, click on "+ New Job" in the "Background jobs" section of the navigation bar. Simply add a name, a description, a start time or frequency, and set the type of job (Action, Query, External URL) to execute.

#### Advanced Options
In the advanced options, you can configure the job to operate via a POST or GET request. The GET request allows you to configure the query string used in the request, as well as modify the headers sent - allowing you to send additional parameters to the job when it executes. For POST requests, you can modify both the headers and the query string, but you are also given the Request Data field which you can use to send additional parameter formats, such as long text.

This can be very useful in distinguishing between two jobs that run the same action. For each job, you can send a "type" parameter to indicate the specific job being run. You can accomplish this by setting the "Query String" field to the appropriate value: `parameters={type:1}` for job 1, and `parameters={type:2}` for job 2.

### Update an existing job
You can update existing job by selecting the background job from the navigation menu and pressing the "Edit" button. This allows you to change all aspects of the job.

### Test a job
You can test both new and existing jobs using the Test buttons on the right hand side of the page. These buttons execute the job on Backand's servers, displaying the request sent and the response received. In addition to simple verification, testing can be used to verify that advanced parameters function properly, and that the job responds as expected.

### Run background job using REST API
```shell
  curl https://api.backand.com/1/jobs/run/{id}
```

```javascript
   var response = $http({
       method: "GET",
       url: CONSTS.apiUrl + "/1/jobs/run/" + cronId
   });
```

Background Jobs are useful for long running tasks such as integrating with external sites (where the response time could be slow), or sending out batched push notifications. If you commonly encounter timeout errors while running on-demand actions, then you should consider using a Background Job.

All messages and errors are logged into the console. These messages are available in the dashboard under Log --> Console.


### Test background job
```bash
curl https://api.backand.com/1/jobs/run/{id}/test
```

In order to test the job and get a response immediately (either valid results or an error message), use the following REST endpoint.

<aside class="notice">this call runs synchronously, so it may take a long time to finish</aside>

## Analytics
The Analytics section provides several reports exploring the various facets of you app's performance. You are able to provide a date range to examine, using preconfigured date ranges our your own custom range. You can use these to gain insight on user activity within your app, as well as on the resources being used by your application.

### Reports
Backand provides the following reports for your app:

* Activity-based reports
  * Devices by Country
  * Daily Active Devices
  * Weekly Active Devices
  * Montly Active Devices
  * Registered Users by Country
  * Daily Active Registered Users
  * Weekly Active Registered Users
  * Monthly Active Registered Users
  * Top Active Registered Users
* Performance-based reports
  * Requests per Objects
  * Slow Requests
* Usage-based reports
  * Backand Compute
  * Cache Memory
  * Requests per Second
  * Data Hosting
  * Database Size

## Security & Auth
The Security & Auth section allows you to modify the security settings for your application, setting both application-level and user-level security policies.

### Security configuration
The Security Configuration section allows you to set application-wide settings for access to your project.

#### Anonymous Access

When enabled, this allows any user with a link to access your application. You can set a role for anonymous users, and obtain an anonymous access token to use in your application's code.

#### Public App
The Public App settings determines whether users can sign up for your app, or if they must be invited. You can also assign a role to each invited user.

<aside class="warning">Email verification is disabled by default, so verifying users requires an extra setting change for your application.</aside>

#### Custom pages
These custom pages are pages within your application code that are used for user registration and login. The Custom Registration Page URL is supplied in the invitation email sent to new users. The Custom Verified Email Page URL is sent to the user after they have verified their email address. Note that these are pages on your own servers, and not a part of Backand. These pages are only sent when Sign-up Email Verification is enabled.

#### Sign-up Email Verification
This enables or disables automated user email verification. When enabled, users are sent a verification email by Backand which they must use to verify their account. When disabled, this action is not performed and users are immediately able to use their account.

### Social & Keys
The Social & Keys page consists of two parts - the tokens, and the social media configurations. These settings are used in your app to enable integration with social media providers, allowing your users to sign into your app using their social media accounts.

#### Master Token

The master Token is a token that you can use to access your Backand app without a username or password.
<aside class="warning">Use this token carefully, as it gives the user full access to your application.. This token mast be sent with the user's GUID key. The master key is used with <a href="https://en.wikipedia.org/wiki/Basic_access_authentication" target="\_blank">basic authentication</a>, simply enter the master key as the username, and the user's GUID key as the password.</aside>

#### API Signup Token
The API Signup Token is used whenever you contact the Backand API to register a new user with your application.

#### Social Configuration
In this section you can configure the social app for each company (GitHub, Google and Facebook). By default you can use Backand as the social app, unless you add your own client and secret ids.

##### Social Redirect URIs
Following sign-in with social media, your user will be redirected to a Redirect URI that is then tasked with obtaining the full details of the authentication that occurred. This URI is called automatically by Backand, and is specified as the redirectUrl parameter to the socialMediaSignup call in the API. In order to support this more secure pattern of working with social media authentication, all URLs that can be redirected to must be present in this Social Redirect URI list. If a URL is not present in this list, then the redirect request will not be honored.

<aside class="notice">This field accepts a comma-separated list of URIs, to allow for multiple endpoints in your application. Simply add each new URI by adding a comma to the previous value, then pasting the new URI into the text box.</aside>

##### GitHub App Configuration
In order to enable signing in with GitHub credentials, follow these steps:

1. Check the 'Use your credentials for signing in with GitHub' toggle
1. Register your application on GitHub's <a href="https://github.com/settings/applications/new" target="\_blank">Developer Applications Tab</a>. Include the following details, which are mandatory for new applications:
    * `Application name`: A name for your app. Something users will recognize and trust
    * `Homepage URL`: Your application's url (you can use http://localhost for testing purposes)
    * `Authorization callback URL`: https://api.backand.com/1/user/github/auth
1. Record the Client ID and the Client Secret from GitHub
1. Finally, on the `Social & Keys` section of the Backand app management dashboard, copy the Client ID and Client Secret in the correct fields in the GitHub section..

More details on GitHub integrations are available in the <a href="https://developer.github.com/v3/oauth/#redirect-urls/" target="\_blank">developer documentation</a>.


##### Google App Configuration
In order to add Google app sign-in, follow these steps:

1. Check the 'Use your credentials for signing in with Google' toggle/
1. Open the <a href="https://console.developers.google.com/project" target="\_blank">Google Developer's Console</a>.
    1. In the Google Developer's Console, Click **Create Project**.
    1. Enter a name and a project ID, or accept the defaults, and click **Create**.
    1. After the project has been created, expand the **APIs & auth** section on the left sidebar.
    1. Click **APIs** to view the API Library, which lists all available APIs. The APIs are grouped by product family and popularity. Click on **Google+ API** under **Social APIs**
    1. Click **Enable API** (You should see this under the Google+ API button).
    1. In the sidebar on the left, select **Consent screen** and enter your product name (this can be your Backand app name).
    1. In the sidebar on the left, select **Credentials**.
    1. Click **Create credentials** and select  **OAuth client ID**
    1. Click  **Configure consents screen** tab on the right panel and enter the Product name, Privacy policy URL (https://www.backand.com)
    1. Click **Save**
    1. Enter the following details:
            * **Application type**: Web application
            * **Name** (this can be your Backand app name)
            * **Authorized JavaScript origins**:  leave this field empty
            * **Authorized redirect URIs**: Replace the default example URL with 'https://api.backand.com/1/user/google/auth'
        1. Click **Create** and **Create** again
        1. Record the **Client ID** and **Client secret** from the popup window and click **OK**
    1. On the left side bar click **Dasboard** make sure Google+ API is listed under the **API** list.
1. Finally, on the **Social & Keys** section of the Backand app management dashboard, copy the Client ID and Client Secret you copied down in the last step.

For more details on integrating with Google, review their <a href="https://developers.google.com/console/help/new/" target="\_blank">developer documentation</a>.

##### Facebook App Configuration

In order to add Facebook login integration, follow these steps:

1. Check the 'Use your credentials for signing in with Facebook' toggle.
1. Open the <a href="https://developers.facebook.com/" target="\_blank">Facebook Developers console</a>.
1. Optional: Click MyApps and register as developer
    1. Under **My Apps**, select **Add a new app**
    1. In the wizard that follows, provide your app's name and **Category**
    1. Click **Create app ID**
    1. Complete the **Security Check** step
    1. In the sidebar on the left, select **Settings**
    1. In Basic tab add your **Contact Email**
    1. In the sidebar click **+ Add Product** under Products
    1. Click Get Started in the **Facebook Login** product
    1. In **Valid OAuth redirect URIs** enter: 'https://api.backand.com/1/user/facebook/auth'
    1. Click **Save Changes**
    1. To start using your app, you need to make it public, open App Preview from the left menu and change **Make
    `{{appName}}` public?** to **yes** and confirm the popup window
    1. In the sidebar on the left, select **Dashboard**    
    1. Record the **App ID**
    1. Click **Show** (near **App Secret**) and enter your Facebook password.
    1. Record the **App Secret**
1.  Finally, on the **Social & Keys** section of the Backand app management dashboard , copy the App ID and App Secret you recorded into the appropriate fields in the Facebook integration section.


##### Twitter App Configuration

In order to add Twitter login integration, follow these steps:

1. Un-check the 'Use Back& app for signing in with Twitter' toggle.
1. If you haven't added your phone number to Twitter you'll need to add it before creating Twitter app, please follow <a href="https://support.twitter.com/articles/110250">these instructions</a>
1. Open the <a href="https://apps.twitter.com/" target="\_blank">Twitter Apps console</a>.
1. Click **Create New App** button
    1. Enter the App details - Name, Description, Website
    1. In the Callback URL enter: **https://api.backand.com/1/user/twitter/auth**
    1. Check **Yes, I agree**  in the developer agreement
    1. Click **Create your Twitter application** button
    1. In the **Detail** tab of your newly created app scroll down to the Application Setting area and click the **manage keys and access tokens** link
    1. Record the **Consumer Key (API Key)** and the **Consumer Secret (API Secret)**
1. In the Backand dashboard, copy the Consumer Key and API Secret you recorded into the appropriate fields in the Twitter integration section.

##### Local ADFS App Configuration
In order to add ADFS login integration, follow these steps:

```bash
# Step 2
Add-AdfsRelyingPartyTrust -Name "Backand QA" -Identifier "https://api.backand.com" -TokenLifetime 10 -IssueOAuthRefreshTokensTo AllDevices -EnableJWT $True -IssuanceTransformRules '@RuleTemplate = "PassThroughClaims" @RuleName = "Windows account name" c:[Type=="http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(claim = c); @RuleTemplate = "PassThroughClaims" @RuleName = "UPN" c:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"] => issue(claim = c);' -IssuanceAuthorizationRules '=> issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");'

# Step 3
# Update the ClientId with any GUID value (Get a <a href="https://www.guidgenerator.com/">new GUID</a>)
Add-ADFSClient -Name "Backand" -ClientId "188c552d-bf43-409b-9355-a0fb0eb227e3" -RedirectUri "https://api.backand.com/1/user/adfs/auth"

# Step 4
# 43,200 is the number of minutes in 30 days - you may change it to meet your organization requirements
Set-AdfsProperties -EnableKmsi $True –KmsiLifetimeMins 43,200
```

1. Open windows command shell on the ADFS server with Admin permissions
2. Run Add-AdfsRelyingPartyTrust command with the following parameters (you may change the defaults as needed):
3. Run Add-ADFSClient to add Backand as a client application to your ADFS:
4. Run this command to configure persistent Single Sign-On and show the "keep me signed in" checkbox:
5. Finally, on the **Social & Keys** section of the Backand app management dashboard, copy the Client Id and Redirect Uri you entered in the Add-ADFSClient command.

##### Azure AD App Configuration
In order to add Azure AD application integration, follow these steps:

1. Open the <a href="https://portal.azure.com/" target="\_blank">Azure new portal</a>.
1. Select **Azure Active Directory**
1. Select **App Registrations**
1. Click **+Add** and use these parameters:
    1. Name: **Backand**
    1. Application Type: **Native**
    1. Redirect URI: **https://api.backand.com/1/user/azuread/auth**
1. Click **Create**
1. Click on the Backand App and copy the Application Id / Client Id (e.g. 7c799275-1102-4aa6-b36a-fac7aa7fee60)
1. Click on "Endpoints" menu and copy "OAUTH 2.0 AUTHORIZATION ENDPOINT". In Backand just connect to /oauth2 in the OAUTH 2.0 Endpoint filed(e.g. "https://login.windows.net/a652911c-7a2c-4a9c-d1b2-d149256f461b/oauth2")

1. Finally, on the **Social & Keys** section of the Backand app management dashboard, copy the Application Id into the Client Id field and OAUTH 2.0 Endpoint you copied down in the last step.

### Registered Users
The registered users page lists all users currently registered for your app. You can add new user, edit existing user details, delete a user, or invite a new user to your application via email. You can also manually enter - and edit - user passwords from this interface.

### Team
The Team section allows you to add new administrative users to your application. Admin users have full access to the application dashboard, and your application's configuration. You can perform all of the same actions available on the Users page on the Team page as well.

### Security Actions (Backand Dashboard)
The actions on this page, much like those on the other pages of the application, allow you to define custom actions to occur at each point in the transactional CRUD conversation against an object, or when called by accessing a specific URL. These actions, though, are tied specifically to the internal Backand Users object, which manages your application''s security. All of the standard custom action options are available for use in this section, and you can easily create new actions, edit existing actions, and test the actions you have created.

The following actions are provided by default for a Backand application. You can edit these actions as you desire, or add new actions that occur during the database transaction that modifies your app's `registered_users` table:

| Action Name | When it Runs | What it Does |
| ----------- | ------------ | ------------ |
| accessFilter | Automatic, after successful authentication | See [our post on third-party security](#integrating-third-party-security) |
| backandAuthOverride | Automatic, prior to user authentication | See [our post on third-party security](#integrating-third-party-security) |
| beforeSocialSignup | Automatic, prior to social media registration | This allows you to run additional code prior to signing up a user through social media-based authentication |
| ChangePasswordOverride | Automatic, during password change | **not currently used** |
| newUserVerification | Automatic, during user registration | This verification message is sent to new users if email address verification is enabled |
| requestResetPassword | Automatic, after a call to `requestResetPassword` | This email is sent in response to initiating a password reset |
| socialAuthOverride | Automatic, after social media authentication *and* registration | See [our post on third-party security](#integrating-third-party-security) |
| userApproval | Automatic, after user registration | This email is sent to users to inform them that they have been successfully added to an app |
| Admin Invitation Email | Automatic, during user creation | This email is used to invite administrators to your application |
| Create My App User | Automatic, during user creation | This function takes the user input from the sdk `signup` call, and uses it to populate a custom `users` object in your application. |
| User Invitation Email | Automatic, during user creation | This email is used to invite standard users to your application |
| Update My App User | Automatic, during user update | This action occurs whenever a registered user is modified. It attempts to make the appropriate modifications in your app's custom `users` object |
| Delete My App User | Automatic, during user deletion | This action occurs whenever a registered user is deleted. It attempts to remove the associated `users` record for the user being deleted. |

<aside class="warning">It is important to note that security actions, like <code>requestResetPassword</code>, require that emails provided be in a valid email format, such as recipient@example.com. If the addresses are not in the correct format, then calls to the security action will fail, resulting in the emails not being sent in a timely fashion.</aside>

Here's a general informational graphic demonstrating how authentication-related actions integrate with the system:

![image](images/security_diagram.png)
### Security Templates
Security templates allow you to create a template that is used to set permissions on objects. You can create new templates, update existing templates, and rename security templates as you see fit. Each of the checkboxes corresponds to the REST API action indicated by the column header. When you check "Create" for a user role, for example, and then assign that security template to an object, all users with the specified role will be granted "Create" access to the object.

## Hosting
The Hosting menu item provides all the tools you need to work with [Backand Hosting](#backand-hosting).

### Configuration
This page provides dynamic information designed to walk you through the relevant steps for the current stage of your hosting effort. Follow the provided guide to initiate, update, and deploy your project's hosting.

### Hosting files
The Hosting Files section lists the files hosted by Backand on behalf of your app. It provides a simple UI for listing and downloading the hosted files.

### Storage files
The Storage Files section lists the files stored in S3 on behalf of your application. You can view the list of files, upload a new file, download a file, or delete a file from your app's s3 storage.

## Log
The Log menu item provides access to Backand's built in logging functionality. We provide a number of different logs with different targets, allowing you to easily see all activity in your application in one place.

### API Requests
The API Requests log logs all requests made to your application over the web, either via cURL or using our SDK. You can filter the request logs based on any of the provided fields.

### Console
The Console log logs all console messages created by your application's actions. You can use this when troubleshooting server-side actions to ensure that all messages are being received correctly, and you can filter the log messages to match your current debugging scenario.

### Server Side Exceptions
The Server Side Exceptions log provides access to any server-side exceptions generated during your app's execution. Details on the exception are provided, and you are given the capability to filter the logs based on arbitrary criteria.

### Configuration
The Configuration log tracks all configuration changes made to your application. This includes all actions taken in your application's Backand dashboard, recording the action taken and the user responsible. You can also filter the log by any of the provided fields.

### Data History
The Data History log contains entries for all modifications made to objects that have data tracking enabled. This log will remain empty until the data tracking feature is enabled for an object.

## Settings
The Settings configuration section contains the application-level configurations for your Backand app.

### General
The General section contains global configuration settings for your application. You can change the app name and title, enable debug mode, adjust the date format, adjust default pagination and object retrieval parameters, and provide global configuration values available throughout your application.

<aside class="warning">You can also delete your Backand application from this screen using the "trash can" icon in the upper-right-hand corner of the page. Be careful - this action is irreversible. </aside>

### Database
The Database section contains information on your app's Backand database. If you have not yet entered any data or modified the default model, you are also given the capability to connect to an existing database platform on this tab. You can use the connection data provided to connect any third-party DB management tool to your database, allowing you to modify or migrate your database as you desire.

### Configuration
The Configuration tab allows you to version your app's configuration. You can export and import settings, or work with previous application configurations.

### Billing Portal
The Billing Portal provides you with a UI that gives you easy access to all the details of your billing agreement with Backand. From this section, you can view your current subscription, your contact information, a list of changes made, and other billing details. You can also add and modify credit card information, add or change your billing address, or review your payment history.

### Upgrade Plan
The Upgrade Plan section provides you with the capability to change your plan to something that meets your needs. Simply select the plan you wish to use, ensure your payment method is correct, then confirm the change.

### Payment Method
The Payment Method section allows you to add and edit your payment methods. Provide all of the required information for your payment method to update how your account is charged.
