#Sign Up
Registering with Backand, and creating an application, automatically sets you as a user with an "Admin" role in your new project (see [roles](../security/roles.md) for more info). By default your application is marked private, which means that only users that you have personally invited can sign up for your app. This can be changed in the dashboard by setting your application to Public in the Security & Auth --> Configuration menu. Setting your application to public allows any user to register for - and use - your application. These users are assigned a default role, which needs to be configured when you enable public usage of your app (see [roles](../security/roles.md) for more details). For security reasons you cannot change the role of a user through API calls - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -> Users page, or by creating a custom server-side action that performs this task. For a private application the registration steps are as follow:

* First, set your application's registration page on the Security & Auth --> Configuration page.
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page. Once you have entered the emails you wish to invite, click the "Invite User(s)" button. This will send an email to each new user with a link to the registration page that you created.
**The following steps are the same for both Public and Private apps:**
* The user enters his/her email, name and password on your registration page
* After the user submits their registration, Backand sends a verification email to authenticate the user's identity
* After the user clicks on the link in the verification email, Backand completes the registration process and redirects to the "Custom Verified Email Page" URL for your application. Configure this on the Security & Auth --> Configuration page
Here is the api of the Sign Up
