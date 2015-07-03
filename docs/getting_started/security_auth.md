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

### API Signup Token

The API Signup Token is a token that you use whenever you contact the Backand API to register a new user with your application.

### Security Actions

The actions on this page, much like those on the other pages of the application, allow you to define custom actions to occur at each point in the transactional CRUD conversation against a database table, or when called by accessing a specific URL. These actions, though, are tied specifically to the internal Backand Users table, which manages your application''s security. All of the standard custom action options are available for use in this section, and you can easily create new actions, edit existing actions, and test the actions you have created.

There are several actions provided by default by Backand. The User Invitation Email and Admin Invitiation Email actions allow you to modify the emails sent to a user when they are invited to the application. The User Approval action allows you to modify the approval email received by the user after email verification. The New User Verification action allows you to modify the generic verification email sent when sign-up verification is enabled. Finally, the Request Reset Password action allows you to modify the email sent when a user requests a password reset token.

## Security Templates

Security templates allow you to create a template that is used to set the permissions of a database table's object. You can create new templates, update existing templates, and rename security templates as you see fit. Each of the checkboxes corresponds to the database action indicated by the column header. When you check "Create" for a user role, for example, and then assign that security template to an object, all users with the specified role will be granted "Create" access to the object.

## Users

The users page lists all users currently registered for your app. You can edit existing user details, delete a user, or invite a new user to your application via email. You can also manually enter - and edit - user passwords from this interface.

## Team

The Team section allows you to add new administrative users to your application. Admin users have full access to the application dashboard, and your application's configuration. You can perform all of the same actions available on the Users page on the Team page as well.

