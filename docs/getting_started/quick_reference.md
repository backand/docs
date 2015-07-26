Here are a few basic URL guidelines for quickly accessing your application's data:

## Base URL

The base URL for your application will be `https://api.backand.com:8078/1` - all of your access will be driven from this URL

## Request Format

All requests must be made using JSON request bodies. Make sure to set the `Content-Type` header to `application/json`. Additionally, all requests must include an authentication token in the request headers. This process is detailed in our [documentation](http://docs.backand.com/en/latest/security/index.html).

## Response Format

All responses are returned as JSON objects. The status code of the response indicates the success (2xx) or failure (4xx) of the request.

## Security & Authentication
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /token | POST | Obtains a 24-hour acccess token. A Backand username and password must be provided, along with the app the user is signing in to, this is optinal if you already set the app name in the setAppName configuration property |
| /user/signup | POST | Registers a new user with the application. Must use a SignUpToken, which is configured for the application |
| /user/requestResetPassword | POST | Sends an email to the provided username with a single-use token that can be used to reset the user's password |
| /user/resetPassword | POST | Resets the user's password after verification using a one-time  access token |
| /user/changePassword | POST | Changes the user's password |

## Manipulating objects
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /objects/{object name} | GET | Gets a list of all objects of type {object name} |
| /objects/{object name} | POST | Creates an object of type {object name} |
| /objects/{object name}/{id} | GET | Gets an object of type {object name} with ID {id} |
| /objects/{object name}/{id} | PUT | Updates an object of type {object name} with ID {id} |
| /objects/{object name}/{id} | DELETE | Deletes an object of type {object name} with ID {id} |

## Custom Object Actions
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /objects/action/{object name}/{id}?actionName={action name} | GET | Executes custom action {action name} on object {object name} with an id of {id} |

## Queries
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /query/data/{query name} | GET | Executes custom query {query name} |

## Backand SDK

### Configuration methods
| Function/Property | Type/Return Value | Usage |
| ----------------- | ----------------- | ----- |
| manageDefaultHeaders | void | tells Backand to manage all necessary authorization and authentication tokens for each request |
| setAppName | string | Sets the Backand's app name |
| setAnonymousToken | string | allows anonymous access to the app |
| setSignUpToken | string | allows users to register for the app |
| getApiUrl | string | returns the current API URL |
| setApiUrl | string | sets the API URL |
| getTokenName | string | gets the cookie name where the authorization token is stored |
| setTokenName | string | sets the cookie name where the authorization token is stored |

### Live-use Methods
| Function | Arguments | Usage |
| -------- | --------- | ----- |
| signIn | username, password, appname | signs the user into the application |
| signUp | firstname, lastname, email, password, confirmPassword | registers the user with the application |
| signOut | none | signs the current user out of the application |
| requestResetPassword | email, appname | sends a one-time use password reset token to the user with the specified email |
| resetPassword | newPassword, resetToken | resets the current user's password, consuming the one-time use token |
| changePassword | oldPassword, newPassword | Changes the current user's  password from the old value to the new. |
| getToken | none | returns the current authorization token |

