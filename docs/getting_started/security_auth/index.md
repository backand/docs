The Security & Auth section allows you to modify the security settings for your application, setting both application-level and user-level security policies.

## Security configuration

The Security Configuration section allows you to set application-wide settings for access to your project.

### Anonymous Access

When enabled, this allows any user with a link to access your application. You can set a role for anonymous users, and obtain an anonymous access token to use in your application's code.

### Public App

The Public App settings determines whether users can sign up for your app, or if they must be invited. You can also assign a role to each invited user. PLEASE NOTE - email verification is disabled by default, so verifying users requires an extra setting change for your application.

### Custom pages

These custom pages are pages within your application code that are used for user registration and login. The Custom Registration Page URL is supplied in the invitation email sent to new users. The Custom Verified Email Page URL is sent to the user after they have verified their email address. Note that these are pages on your own servers, and not a part of Backand. These pages are only sent when Sign-up Email Verification is enabled.

### Sign-up Email Verification

This enables or disables automated user email verification. When enabled, users are sent a verification email by Backand which they must use to verify their account. When disabled, this action is not performed and users are immediately able to use their account.

### Security Actions

The actions on this page, much like those on the other pages of the application, allow you to define custom actions to occur at each point in the transactional CRUD conversation against an object, or when called by accessing a specific URL. These actions, though, are tied specifically to the internal Backand Users object, which manages your application''s security. All of the standard custom action options are available for use in this section, and you can easily create new actions, edit existing actions, and test the actions you have created.

There are several actions provided by default by Backand. The User Invitation Email and Admin Invitation Email actions allow you to modify the emails sent to a user when they are invited to the application. The User Approval action allows you to modify the approval email received by the user after email verification. The New User Verification action allows you to modify the generic verification email sent when sign-up verification is enabled. Finally, the Request Reset Password action allows you to modify the email sent when a user requests a password reset token.

## Social & Keys

The Social & Keys page consists of two parts - the tokens, and the social media configurations. These settings are used in your app to enable integration with social media providers, allowing your users to sign into your app using their social media accounts.

### Master Token

The master Token is a token that you can use to access your Backand app without a username or password. **NOTE**: Use this token carefully, as it gives the user full access to your application.. This token mast be sent with the user's GUID key. The master key is used with [basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication), simply enter the master key as the username, and the user's GUID key as the password.

### API Signup Token

The API Signup Token is used whenever you contact the Backand API to register a new user with your application.

### Social Configuration

In this section you can configure the social app for each company (GitHub, Google and Facebook). By default you can use Backand as the social app, unless you add your own client and secret ids.

#### GitHub App Configuration

In order to enable signing in with GitHub credentials, follow these steps:

1. Un-check the 'Use Back& app for signing in with GitHub' toggle
1. Register your application on GitHub's <a href="https://github.com/settings/applications/new" target="_blank">Developer Applications Tab</a>. Include the following details, which are mandatory for new applications:
    * **Application name**: A name for your app. Something users will recognize and trust
    * **Homepage URL**: Your application's url (you can use http://localhost for testing purposes)
    * **Authorization callback URL**: https://api.backand.com/1/user/github/auth
1. Record the Client ID and the Client Secret from GitHub
1. In Backand's Social Configuration section, enter the Client ID and Client Secret in the correct fields in the GitHub section..

More details on GitHub integrations are available in <a href="https://developer.github.com/v3/oauth/#redirect-urls/" target="_blank">developer documentation</a>.


#### Google App Configuration

In order to add Google app sign-in, follow these steps:

1. Un-check the 'Use Back& app for signing in with Google' toggle/
1. Open the <a href="https://console.developers.google.com/project" target="_blank">Google Developer's Console</a>. 
    1. In the Google Developer's Console, Click **Create Project**.
    1. Enter a name and a project ID, or accept the defaults, and click **Create**. 
    1. After the project has been created, expand the **APIs & auth** section on the left sidebar.
    1. Click **APIs** to view the API Library, which lists all available APIs. The APIs are grouped by product family and popularity. Click on **Google+ API** under **Social APIs**
    1. Click **Enable API** (You should see this under the Google+ API button).
    1. In the sidebar on the left, select **Consent screen** and enter your product name (this can be your Backand app name).
    1. In the sidebar on the left, select **Credentials**.
    1. Click **Create new Client ID** and enter the following details:
        * **Application type**: Web application (default)
        * **Authorized JavaScript origins**: Delete the default example URL and leave this field empty
        * **Authorized redirect URIs**: Replace the default example URL with `https://api.backand.com/1/user/google/auth`
    1. Click **Create Client ID**
    1. Record your Client ID and Client Secret from the dashboard. They can be found in the **Client ID for web application** section.
1. Finally, on the **Social & Keys** section of the Backand app management dashboard, fill out the Client ID and Client Secret you copied down in the last step.

For more details on integrating with Google, review their <a href="https://developers.google.com/console/help/new/" target="_blank">developer documentation</a>.

#### Facebook App Configuration

In order to add Facebook login integration, follow these steps:

1. Un-check the 'Use Back& app for signing in with Facebook' toggle. 
1. Go to the <a href="Open https://developers.facebook.com/" target="_blank">Facebook Developers console</a>.
1. Optional: Click MyApps and register as developer
    1. Under **My Apps**, select **Add a new app**
    1. Select **WWW (web site)**
    1. In the wizard that follows, provide your app's name and click **Create New Facebook App ID**
    1. Choose a Category and a Sub-Category, then click **Create App ID**
    1. Click on **Skip Quick Start** (In the upper right side of the window)
    1. In the sidebar on the left, select **Settings**
    1. Click on the **Advanced** Tab
    1. In **Valid OAuth redirect URIs** enter: `https://api.backand.com/1/user/facebook/auth`
    1. Click **Save Changes**
    1. In the sidebar on the left, select **Dashboard**    
    1. Record the **App ID**
    1. Click **Show** (near **App Secret**) and enter your Facebook password.
    1. Record the **App Secret**
1. In the Backand dashboard, copy the App ID and App Secret you recorded into the appropriate fields in the Facebook integration section.


## Security Templates

Security templates allow you to create a template that is used to set permissions on objects. You can create new templates, update existing templates, and rename security templates as you see fit. Each of the checkboxes corresponds to the REST API action indicated by the column header. When you check "Create" for a user role, for example, and then assign that security template to an object, all users with the specified role will be granted "Create" access to the object.

## Registered Users

The registered users page lists all users currently registered for your app. You can add new user, edit existing user details, delete a user, or invite a new user to your application via email. You can also manually enter - and edit - user passwords from this interface.

## Team

The Team section allows you to add new administrative users to your application. Admin users have full access to the application dashboard, and your application's configuration. You can perform all of the same actions available on the Users page on the Team page as well.

