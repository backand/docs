This section covers [Backand's JavaScript SDK 1.7.x](https://github.com/backand/angularbknd-sdk)

## Configuration Properties

These methods are used to configure your application's usage of Backand

### manageDefaultHeaders
#### Usage
Tells Backand to manage the default headers, which automatically populates the backand configuration with all relevant tokens when requests are made. 
#### Returns
Nothing

### setAppName
#### Usage
Sets the Backand app name for your application
#### Returns
Nothing

### setAnonymousToken
#### Usage
Sets the value of the anonymous use token
#### Returns
Nothing

### setSignUpToken
#### Usage
Sets the value of the user registration token
#### Returns
Nothing

### getApiUrl
#### Usage
Returns the base API URL for this application
#### Returns
A string representing the base API URL

### setApiUrl
#### Usage
Sets the base API url for this application
#### Returns
Nothing

### getTokenName
#### Usage
Gets the name of the cookie in which the authorization token is stored
#### Returns
The cookie name

### setTokenName
#### Usage
Sets the name of the cookie in which the authorization token is stored
#### Returns
Nothing

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
