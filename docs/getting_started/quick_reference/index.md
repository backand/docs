Here are a few basic usage guidelines for quickly accessing your application's data:

## API Endpoints

All API methods are accessible through our SDK, available on our github [here](https://github.com/backand/vanilla-sdk).

## Native calls
- Property: none
- Sample call: `backand.signin(username, password)`

| Function | Arguments | Usage |
| -------- | --------- | ----- |
| `useAnonymousAuth` | boolean | Indicates whether or not to use anonymous authentication |
| `signin` | username, password | signs a user in |
| `signup` | firstName, lastName, email, password, confirmPassword, parameters | registers a user with your application |
| `socialSignin` | provider | performs signin with a social media provider |
| `socialSigninWithToken` | provider, token | performs token-based sign in with a social media provider |
| `socialSignup` | provider, email | Registers the user with the specified provider |
| `requestResetPassword` | username | initiates the reset password process for a user |
| `resetPassword` | newPassword, resetToken | Resets the user's password to the new value. |
| `changePassword` | oldPassword, newPassword | Changes the user's password |
| `signout` | *none* | Signs the current user out |
| `getSocialProviders`  | *none* | Returns the list of supported providers |

## Manipulating objects
- Property: `object`
- Sample call `backand.object.getList('users');`

| Function | Arguments | Usage |
| -------- | --------- | ----- |
| getList | object_name, params | Gets a list of all objects of type {object_name} |
| getOne | object_name, id, params | Gets an object of type {object name} with ID {id} |
| create | object_name, data, params |  Creates an object of type {object name} with {data} |
| update | object_name, id, data, params | Updates an object of type {object_name} with ID {id} to match {data} |
| remove | object_name, id, params | Deletes an object of type {object name} with ID {id} |

## Custom Object Actions
- Property: `object`
- Sample call `backand.object.get('users', 'actionName', {});`

| Function | Arguments | Usage |
| -------- | --------- | ----- |
| get | object, action, params | executes custom action {action} on object {object} via HTTP GET |
| post | object, action, data, params | executes custom action {action} on object {object} via HTTP POST with data {data} |

## Queries
- Property: `query`
- Sample call `backand.object.get(name, params);`

| Function | Arguments | Usage |
| -------- | --------- | ----- |
| get | name, params | executes custom query {name} on object {object} via HTTP GET |
| post | name, data, params | executes custom query {name} on object {object} via HTTP POST with data {data} |

## User management methods
- Property: `user`
- Sample call: `backand.user.getUsername();`

| Function | Arguments | Usage |
| -------- | --------- | ----- |
| getUserDetails | force | returns the current user details |
| getUsername | none | returns the current user's username |
| getUserRole | none | returns the current user's role |
| getToken | none | returns the current authorization token |
| getRefreshToken | none | returns the current refresh token |

## File management
- Property: `file`
- Sample call: `backand.file.upload(object, action, filename, fileData)`

| Function | Arguments | Usage |
| -------- | --------- | ----- |
| upload | object, action, filename, fileData | Uploads the specified file to the specified action |
| remove | object, action, filename | Removes the specified file from the specified action |

### Configuration methods
- Property: none
- Sample call: `backand.setAppName(appName);`

| Function | Argument Type | Return Value | Usage |
| ----------------- | ------------ | ----------- | ----- |
| setAppName | string | BackandProvider | Sets the Backand app name |
| setAnonymousToken | string | BackandProvider | allows anonymous access to the app |
| setSignUpToken | string | BackandProvider | allows users to register for the app |
| manageHttpInterceptor | boolean | BackandProvider | tells Backand to manage all necessary authorization and authentication tokens for each request |
| manageRefreshToken | boolean | BackandProvider | tells Backand to manage re-authenticating using a refresh token when the session has expired |
| runSigninAfterSignup | boolean | BackandProvider | tells Backand to perform signing in after a user signs up |
| setApiUrl | string |  BackandProvider | sets the API URL |
| getApiUrl | void | string | returns the current API URL |

## Response Format

All responses are returned as promises. You can use `.then()` or `.catch()` to work with the results of the API operations. Data results are stored in the `data` property of the response object
