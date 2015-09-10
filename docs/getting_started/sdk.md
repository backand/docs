This section covers [Backand's JavaScript SDK 1.8.x](https://github.com/backand/angularbknd-sdk)

## Getting Started

Backand SDK provides methods for easily communicating with the Backand server and performing common tasks, such as managing users. To use the SDK, first include the Backand SDK script files in your app:

```
      <!-- Backand SDK for Angular -->
      <script src="//cdn.backand.net/backand/dist/1.5.1/backand.min.js"></script>
``` 

Then add the Backand dependency to your angular app definition:
```
      //app.js
      angular.module('YOUR-APP-NAME', ['backand']);
```
## Configuration Properties Setters

These methods are used to configure your application's usage of Backand. Call these methods in the application config stage, on BackandProvider. For example, the following code snippet sets the application name and tokens:

```
    angular.module('YOUR-APP-NAME')
      .config(function (BackandProvider) {
          BackandProvider.setAppName(APP_NAME)
            .setAnonymousToken(ANONYMOUS_TOKEN)
            .setSignUpToken(SIGN_UP_TOKEN);
```

### setAppName
#### Usage
Sets the Backand app name for your application
#### Returns
BackandProvider

### setAnonymousToken
#### Usage
Sets the value of the anonymous use token
#### Returns
BackandProvider

### setSignUpToken
#### Usage
Sets the value of the user registration token
#### Returns
BackandProvider

### manageHttpInterceptor
#### Usage
Tells Backand to manage the default headers, which automatically populates the backand configuration with all relevant tokens when requests are made. 
#### Default
true
#### Returns
BackandProvider

### manageRefreshToken
#### Usage
Tells Backand to manage re-authenticating using the refresh token when the session has expired. Backand's http interceptor stores the rejected requests, performs the authentication, and sends again the request. The process is seemless to the user. For enabling this option, the application must be configured to use refresh tokens (in Security & Auth --> Social & Keys --> Session Length) and to manage interceptor through the SDK (manageHttpInterceptor).
#### Default
true
#### Returns
BackandProvider

### runSigninAfterSignup
#### Usage
Tells Backand to seemlessly perform signing in after a user signes up.
#### Default
true
#### Returns
BackandProvider

### setApiUrl
#### Usage
Sets the base API url for this application
#### Default
"https://api.backand.com"
#### Returns
BackandProvider

## Configuration Properties Getters

Use these methods to get configuration properties in the config stage of your application.

### getApiUrl
#### Usage
Returns the base API URL for this application
#### Returns
A string representing the base API URL

## Methods

These methods are used while the app is running to manage users and authentication.

### signIn
#### Usage
Signs the specified user into the application
#### Arguments
* username - the user to authenticate
* password - the user's password
* appname - the app the user is signing in to (optional if you have already set the app name using the setAppName configuration property)

### signUp
#### Usage
Registers a user for the application
#### Arguments
* firstname - represents the user's first name
* lastname - represents the user's last name
* email - the new user's email address
* password - The new user's desired password
* confirmPassword - the user's desired password again, used as confirmation of correct entry

### socialSignIn
#### Usage
Signs the specified user into the application using a third-party social media provider
#### Arguments
* provider - one of the following: google, facebook, github
* returnAddress - the url to return to, after the user signs in

### socialSignUp
#### Usage
Registers a user for the application using a third-party social media provider
#### Arguments
* provider - one of the following: google, facebook, github
* returnAddress - the url to return to, after the user signs up

### getSocialProviders
#### Usage
Returns a providers object containing a list of providers each with a name, label, url, css, and ID
#### Returns
providers object

### signOut
#### Usage
Signs the currently authenticated user out of the application
#### Arguments
None

### requestResetPassword
#### Usage
Initiates the password reset process, triggering an email containing a one-time password reset token to be sent to the user.
#### Arguments
* email - the emil of the user whose password is to be reset
* appname - the name of the app the user is registered with

### resetPassword
#### Usage
Resets the user's password, consuming the one-time token sent from requestResetPassword
#### Arguments 
* newPassword - The new password to use
* resetToken - The one-time use token sent to the user's email address

### changePassword
#### Usage
Changes the authenticated user's password
#### Arguments
* oldPassword - The user's old password
* newPassword - The user's desired new password
