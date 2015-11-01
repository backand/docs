This section covers [Backand's JavaScript SDK 1.8.x](https://github.com/backand/angularbknd-sdk)

## Getting Started

The Backand SDK provides methods for easily communicating with the Backand server and performing common tasks, such as managing users. To use the SDK, first include the Backand SDK script files in your app:

```
      <!-- Backand SDK for Angular -->
      <script src="//cdn.backand.net/backand/dist/1.8.0/backand.min.js"></script>
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
#### Arguments
* appName - a string of the application name

#### Returns
BackandProvider

### setAnonymousToken
#### Usage
Sets the value of the anonymous use token
#### Arguments
* token - a string of the application's anonymous token

#### Returns
BackandProvider

### setSignUpToken
#### Usage
Sets the value of the user registration token
#### Arguments
* token - a string of the application's sign up token

#### Returns
BackandProvider

### manageHttpInterceptor
#### Usage
Tells Backand to use its HTTP interceptor, which automatically adds authentication headers with the relevant tokens when requests are sent to Backand.
#### Arguments
* isManagingHttpInterceptor - a boolean, true if Backand should use its HTTP interceptor.

#### Default
true
#### Returns
BackandProvider

### manageRefreshToken
#### Usage
Tells Backand to manage re-authenticating using the refresh token when the session has expired. Backand's HTTP interceptor stores the rejected requests, performs the authentication, and resend the requests. The process is seamless to the user. The application must be configured to use refresh tokens in order to enable this option (in Security & Auth --> Social & Keys --> Session Length), as well as to manage the interceptor through the SDK (manageHttpInterceptor).
#### Arguments
* isManagingRefreshToken - a boolean, true if Backand should manage re-authenticating using the refresh token.

#### Default
true
#### Returns
BackandProvider

### runSigninAfterSignup
#### Usage
Tells Backand to seamlessly perform a sign in after a user signs up for your application. This configuration is irrelevant when signing up with a social provider, since the user is always signed in after sign-up.
#### Arguments
* runSigninAfterSignup - a boolean, true if Backand should perform signing in after a user signes up.

#### Default
true
#### Returns
BackandProvider

### setApiUrl
#### Usage
Sets the base API URL for this application
#### Arguments
* ApiUrl - a string of the API URL

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

### Events
Backand.EVENTS holds the following strings which are broadcast from the $rootScope when authentication actions are performed via Backand SDK:
* SIGNIN: 'BackandSignIn'
* SIGNUP: 'BackandSignUp'
* SIGNOUT: 'BackandSignOut'

The following methods are used while the app is running to manage users and authentication.

### signin
#### Usage
Signs the specified user into the application. Broadcasts Backand.EVENTS.SIGNIN upon success.
#### Arguments
* username - the user's email
* password - the user's password

### signup
#### Usage
Registers a user for the application. Broadcasts Backand.EVENTS.SIGNUP upon success.
#### Arguments
* firstname - the user's first name
* lastname - the user's last name
* email - the new user's email address
* password - The new user's desired password
* confirmPassword - the user's desired password again, used as confirmation of correct entry
* parameters - additional parameters to be sent, i.e. CAPTCHA

### socialSignin
#### Usage
Signs the specified user into the application using a third-party social media provider. Broadcasts Backand.EVENTS.SIGNIN upon success.
#### Arguments
* provider - a string, one of the following: 'google', 'facebook', 'gitHub'
* spec - a string of the spec of the pop-up window used to authenticate the user. Default value: 'left=1, top=1, width=600, height=600'. See [W3Schools: Window open() Method](http://www.w3schools.com/jsref/met_win_open.asp) for more options.

### socialSignup
#### Usage
Registers a user for the application using a third-party social media provider. Broadcasts Backand.EVENTS.SIGNUP upon success.
#### Arguments
* provider - a string, one of the following: 'google', 'facebook', 'github'
* parameters - additional parameters to be sent, i.e. CAPTCHA
* spec - a string of the spec of the pop-up window used to authenticate the user. Default value: 'left=1, top=1, width=600, height=600'. See [W3Schools: Window open() Method](http://www.w3schools.com/jsref/met_win_open.asp) for more options.

### getSocialProviders
#### Usage
Returns a providers object containing a list of providers each with a name, label, url, css identifier, and ID
#### Returns
providers object

### signout
#### Usage
Signs the currently authenticated user out of the application and removes all the user data and tokens from the local storage. Broadcasts Backand.EVENTS.SIGNOUT upon success.

### requestResetPassword
#### Usage
Initiates the password reset process, triggering an email containing a one-time password reset token to be sent to the user.
#### Arguments
* email - the email of the user whose password is to be reset

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

### getUserDetails
#### Usage
`getUserDetails` is an asynchronous method that retrieves all information about the current authenticated user: username, user ID, full name, role, tokens, etc. This information is pulled down when the user signs in, and stored in local storage.
#### Arguments
* force - If true, gets the user profile (username, fullName and role) from the server and updates the local storage with the retrieved data. Otherwise, gets it from the local storage. Either way, the rest of the details are taken from the local storage.

#### Returns
A promise for an object containing the user details.

### getUsername
#### Usage
Returns the current user's username which was saved in local storage when the user signed in.
#### Returns
A string of the user's username

### getUserRole
#### Usage
Returns the currently signed-in user's role (User, Admin, Public or ReadOnly), which was saved in local storage when the user signed in.
#### Returns
A string representing the user's role

### getToken
#### Usage
Returns the access token used to communicate with Backand.
#### Returns
A string of the access token.

### getApiUrl
#### Usage
Returns the base API URL for this application
#### Returns
A string representing the base API URL

### isManagingHttpInterceptor
#### Usage
Returns true if the application is configured to use the Backand SDK HTTP interceptor, which attaches authentication headers to requests sent to Backand.
#### Returns
Boolean

### isManagingRefreshToken
#### Usage
Returns true if the application is using refresh tokens (unlimited session length) and is configured to let Backand SDK manage re-authenticating when the session has expired. When true, Backand's HTTP interceptor stores the rejected requests, performs the authentication, and resends the requests. Notice that isManagingHttpInterceptor should also be true in order for the process described above to occur.
#### Returns
Boolean

### setAppName
#### Usage
Sets the Backand app name for your application. Used mainly for demo applications, in which you can configure the application name using its UI, instead of inserting it in the application's code.
#### Arguments
* appName - a string of the application name

## Deprecated Methods
### manageDefaultHeaders
#### Reason
Replaced by manageHttpInterceptor. Managing the Authorization headers is not done by default headers any more but by implementing httpInterceptor internally. 

### getTokenName
#### Reason
The latest version no longer uses cookies. The token name was used as the cookie name. 

### setTokenName
#### Reason
The latest version no longer uses cookies. The token name was used as the cookie name. 
