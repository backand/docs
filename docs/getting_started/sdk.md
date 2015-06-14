This section covers [Backand's JavaScript SDK](https://github.com/backand/angularbknd-sdk)

## Configuration Properties

These methods are used to configure your application's usage of Backand

### manageDefaultHeaders 
### setAnonymousToken
### setSignUpToken
### getApiUrl
### setApiUrl
### getTokenName
### setTokenName | string | sets the token name |

## Methods

These methods are used while the app is running to manage users and authentication.

### signIn
#### Usage
Signs the specified user into the application
#### Arguments
* username - the user to authenticate
* password - the user's password
* appname - the app the user is signing in to

### signUp
#### Usage
Registers a user for the application
#### Arguments
* firstname - represents the user's first name
* lastname - represents the user's last name
* email - the new user's email address
* password - The new user's desired password
* confirmPassword - the user's desired password again, used as confirmation of correct entry

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

### getToken 
#### Usage
Returns the current authentication token
#### Arguments
None 

### getTokenName
#### Usage
Returns the configured token name
#### Arguments 
None

### getApiUrl
#### Usage
Returns the URL for the API for this application
#### Arguments
None