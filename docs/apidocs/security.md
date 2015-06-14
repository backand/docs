# Authentication

The default authentication setup for Backand applications relies on [OAuth2](http://oauth.net/2/) to provide tokenize authentication. By logging in with your username (your email address), your password, and your app name, you receive an authentication token that is valid for 24 hours. This token is required for all communication with Backand, and as such we highly recommend that you use Backand's angular infrastructure to help you save the access token. Using the [ngCookies](https://docs.angularjs.org/api/ngCookies) wrapper, you can adjust the default request header to include your current access token, greatly easing the process of securing your API communications. Backand also offers Anonymous Access, which allows you to access your application without the need to authenticate via username and password.

# Sign Up
Registering with Backand, and creating an application, automatically sets you as a user with an "Admin" role in your new project (see [roles](security.md#roles) for more info). By default your application is marked private, which means that only users that you have personally invited can sign up for your app. This can be changed in the dashboard by setting your application to Public in the Security & Auth --> Configuration menu. Setting your application to public allows any user to register for - and use - your application. These users are assigned a default role, which needs to be configured when you enable public usage of your app (see [roles](security.md#roles) for more details). For security reasons you cannot change the role of a user through API calls - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -> Users page, or by creating a custom server-side action that performs this task. For a private application the registration steps are as follow:

* First, set your application's registration page on the Security & Auth --> Configuration page.
* Next, if you wish to use automated email verification, enable Sign-up Email Verification on the Security & Auth --> Configuration page
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page. 
* Once you have entered the emails you wish to invite, click the "Invite User(s)" button. This will send an email to each new user with a link to the registration page that you created.
**The following steps are the same for both Public and Private apps:**
* The user enters his/her email, name and password on your registration page
* After the user submits their registration, Backand sends a verification email to authenticate the user's identity if Sign-up Email Verification is enabled
* If Sign-up Email Verification is not enabled, at this point registration is complete.
* If Sign-up Email Verification is enabled, after the user clicks on the link in the verification email, Backand completes the registration process and redirects to the "Custom Verified Email Page" URL for your application. Configure this on the Security & Auth --> Configuration page

# Remove user from the app
There are two ways to remove a user from the application. You can permanently remove a user from the application by deleting a user from the Users grid in your application's admin section. This requires the user to register again if they wish to continue using your app. Alternatively, you can un-check the "approved" checkbox in the user's row on the Security & Auth --> Users page. This allows you to reinstate the user simply by re-checking the "approved" column.

# Anonymous Access
If you wish to enable anonymous access to your application, you need to perform a couple steps. First, you need to turn on the Anonymous Access option in you application's settings. Once that's done, you are given the option to create an AnonymousToken value. By passing this value in a request header (e.g. AnonymousToken=<your token here>) you can perform api actions for your application. finally, you need to set the default role of an anonymous user within your application's settings (see [roles](security.md#roles) for more info).

# Link your app's users with Backand's users
We offer full capability to link your application's users with Backand users. However, to ensure that you can restrict access to your application without impacting the Backand user's experiece, we recommend that you create another table for your app's users with a unique email address column. This allows you to build rules specific to your app, but still link to Backand's users without affecting their overall experience.

# <a name="roles"></a>Roles & Security Templates

Each user has a role. When you created your app, you were automatically assigned with an "Admin" role. The "Admin" role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.

