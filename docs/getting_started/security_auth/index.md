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

1. Check the 'Use your credentials for signing in with GitHub' toggle
1. Register your application on GitHub's <a href="https://github.com/settings/applications/new" target="_blank">Developer Applications Tab</a>. Include the following details, which are mandatory for new applications:
    * **Application name**: A name for your app. Something users will recognize and trust
    * **Homepage URL**: Your application's url (you can use http://localhost for testing purposes)
    * **Authorization callback URL**: https://api.backand.com/1/user/github/auth
1. Record the Client ID and the Client Secret from GitHub
1. Finally, on the **Social & Keys** section of the Backand app management dashboard, copy the Client ID and Client Secret in the correct fields in the GitHub section..

More details on GitHub integrations are available in <a href="https://developer.github.com/v3/oauth/#redirect-urls/" target="_blank">developer documentation</a>.


#### Google App Configuration

In order to add Google app sign-in, follow these steps:

1. Check the 'Use your credentials for signing in with Google' toggle.
1. Open the <a href="https://console.developers.google.com/iam-admin/projects" target="_blank">Google Developer's Console</a>.
    1. In the Google APIs console, click the Project menu and select **Create Project**.
    1. Enter a name and click **Create**.
    1. After the project has been created,
    1. Click **Library** on the left sidebar to view the API Library, which lists all available APIs. The APIs are grouped by product family and popularity. Click on Google+ API under Social APIs
    1. Click **Enable API**.
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

For more details on integrating with Google, review their <a href="https://developers.google.com/console/help/new/" target="_blank">developer documentation</a>.

#### Facebook App Configuration

In order to add Facebook login integration, follow these steps:

1. Check the 'Use your credentials for signing in with Facebook' toggle.
1. Open the <a href="https://developers.facebook.com/" target="_blank">Facebook Developers console</a>.
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
    {{appName}} public?** to **yes** and confirm the popup window
    1. In the sidebar on the left, select **Dashboard**    
    1. Record the **App ID**
    1. Click **Show** (near **App Secret**) and enter your Facebook password.
    1. Record the **App Secret**
1.  Finally, on the **Social & Keys** section of the Backand app management dashboard , copy the App ID and App Secret you recorded into the appropriate fields in the Facebook integration section.


#### Twitter App Configuration

In order to add Twitter login integration, follow these steps:

1. Un-check the 'Use Back& app for signing in with Twitter' toggle.
1. If you haven't added your phone number to Twitter you'll need to add it before creating Twitter app, please follow <a href="https://support.twitter.com/articles/110250">these instructions</a>
1. Open the <a href="https://apps.twitter.com/" target="_blank">Twitter Apps console</a>.
1. Click **Create New App** button
    1. Enter the App details - Name, Description, Website
    1. In the Callback URL enter: **https://api.backand.com/1/user/twitter/auth**
    1. Check **Yes, I agree**  in the developer agreement
    1. Click **Create your Twitter application** button
    1. In the **Detail** tab of your newly created app scroll down to the Application Setting area and click the **manage keys and access tokens** link
    1. Record the **Consumer Key (API Key)** and the **Consumer Secret (API Secret)**
1. In the Backand dashboard, copy the Consumer Key and API Secret you recorded into the appropriate fields in the Twitter integration section.

#### Local ADFS App Configuration

In order to add ADFS login integration, follow these steps:

1. Open windows command shell on the ADFS server with Admin permissions
2. Run Add-AdfsRelyingPartyTrust command with the following parameters (you may change the defaults as needed):

```bash
Add-AdfsRelyingPartyTrust -Name "Backand QA" -Identifier "https://api.backand.com" -TokenLifetime 10 -IssueOAuthRefreshTokensTo AllDevices -EnableJWT $True -IssuanceTransformRules '@RuleTemplate = "PassThroughClaims" @RuleName = "Windows account name" c:[Type=="http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(claim = c); @RuleTemplate = "PassThroughClaims" @RuleName = "UPN" c:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"] => issue(claim = c);' -IssuanceAuthorizationRules '=> issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");'
```

3. Run Add-ADFSClient to add Backand as a client application to your ADFS:

```bash
Add-ADFSClient -Name "Backand" -ClientId "188c552d-bf43-409b-9355-a0fb0eb227e3" -RedirectUri "https://api.backand.com/1/user/adfs/auth"
```

  Update the ClientId with any GUID value (Get <a href="https://www.guidgenerator.com/">new GUID</a>)

4. Run this command to configure persistent Single Sign-On and show the "keep me signed in" checkbox:

```bash
Set-AdfsProperties -EnableKmsi $True –KmsiLifetimeMins 43,200
```

  43,200 are numbers of minutes in 30 days - you may change it to meet your organization requirements

5. Finally, on the **Social & Keys** section of the Backand app management dashboard, copy the Client Id and Redirect Uri you entered in the Add-ADFSClient command.

#### Azure AD App Configuration

In order to add Azure AD application integration, follow these steps:

1. Open the <a href="https://portal.azure.com/" target="_blank">Azure new portal</a>.
1. Select **Azure Active Directory**
1. Select **App Registrations**
1. Click **+Add** and use these parameters:
    1. Name: **Backand**
    1. Application Type: **Native**
    1. Redirect URI: **https://api.backand.com/1/user/azuread/auth**
1. Click **Create**
1. Click on the Backand App and copy the Application Id / Client Id (e.g. 7c799275-1102-4aa6-b36a-fac7aa7fee60)
1. Click on “Endpoints” menu and copy “OAUTH 2.0 AUTHORIZATION ENDPOINT”. In backand just use up to /oauth2 in the OAUTH 2.0 Endpoint filed(e.g. "https://login.windows.net/a652911c-7a2c-4a9c-d1b2-d149256f461b/oauth2")

1. Finally, on the **Social & Keys** section of the Backand app management dashboard, copy the Application Id into the Client Id field and OAUTH 2.0 Endpoint you copied down in the last step.

## Security Templates

Security templates allow you to create a template that is used to set permissions on objects. You can create new templates, update existing templates, and rename security templates as you see fit. Each of the checkboxes corresponds to the REST API action indicated by the column header. When you check "Create" for a user role, for example, and then assign that security template to an object, all users with the specified role will be granted "Create" access to the object.

## Registered Users

The registered users page lists all users currently registered for your app. You can add new user, edit existing user details, delete a user, or invite a new user to your application via email. You can also manually enter - and edit - user passwords from this interface.

## Team

The Team section allows you to add new administrative users to your application. Admin users have full access to the application dashboard, and your application's configuration. You can perform all of the same actions available on the Users page on the Team page as well.

