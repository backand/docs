Backand provides role-based security that allows you to determine specific permissions for each group of users. Backand uses [OAuth2](http://oauth.net/2/) authentication to identify users. Backand's implementation of OAuth2 authentication requires you to send the username, password, and appname (application name). In response, you receive an authentication token that must be supplied for all further communication with Backand.

You can either provide this token with each request, or use a cookie to persist the authentication token (recommended). Providing this token is required to use Backand's REST API. We have prepared a Backand Provider that will help you the authentication activities. Start by including the Backand SDK script files in your app:

```
      <!-- We use client cookies to save the REST API token -->
      <script src="//code.angularjs.org/1.3.0/angular-cookies.min.js"></script>
      <!-- Backand SDK for Angular -->
      <script src="//cdn.backand.net/backand/dist/1.5.1/backand.min.js"></script>
``` 
You will also need to add the Backand and angular-cookies dependencies to your angular app definition:

```
      //app.js
      angular.module('YOUR-APP-NAME', ['backand', 'ngCookies'])
```
 
Once this is complete, you are ready to sign in to Backand, for other users to sign up to your app read about it here. There is also an option for anonymous access, read about it here.


####Sign In
Use the Backand provider with the following parameters to get an OAuth2 access token:

* **username** - Backand's username.
* **password** - Backand's .password
* **appName** - The application name.

```
      // SignInCtrl.js
      function SignInCtrl(Backand, $cookieStore) {
        $scope.signIn = function() {
          Backand.signin($scope.username, $scope.password, $scope.appName)
          .then(
            function (token) {
              //save the token in the cookie
              $cookieStore.put(Backand.configuration.tokenName, token);
            },
            function (data, status, headers, config) {
              //handle error
            }
          );
        }
      }
      // Use an Angular HTTP Interceptor to add the authentication token to each HTTP request
      function httpInterceptor($q, $log, $cookieStore) {
        return {
          request: function(config) {
            config.headers['Authorization'] = $cookieStore.get('backand_token');
            return config;
          }
        };
      }
    
```

####Sign Up

With the `/user/signup` api you can enable other users to sign up to your app. The sign up process consists of the following steps:

- From your app's registration page, call the `/user/signup` action
- The email sent contains a verification link. When the user clicks the included link, Backand verifies the user's email address and completes the sign-up process by redirecting the user to a pre-defined success page (typically the application's sign-in page).

The sign up api uses the SignUpToken in the request header. You can find this token in the Security & Auth Configuration menu 
(*) Configuration of the sign up process is located in the App left side menu: Security & Auth --> Configuration

The parameters for the sign-up call are:
* **SignUpToken** - A security token to prevent malicious attacks. this should be added to the header
* **user** - A JSON object representing the user's first name, last name, email, and password.

```
{
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "password": "string",
  "confirmPassword": "string"
}
```
```
  self.signUp = function (user, signUpToken) {
      return $http({
          method: 'POST',
          url : Backand.configuration.apiUrl + '/1/user/signup,
          headers: {
            'SignUpToken': signUpToken
          },
          data: user
      })
  };
```
####Anonymous Access

If you want to allow anonymous users to access the application (i.e. without username and password), you can use the AnonymousToken parameter:


```
      // Use an Angular HTTP Interceptor to add the anonymous token to each HTTP request
      function httpInterceptor($q, $log) {
        return {
          request: function(config) {
            config.headers['AnonymousToken'] = anonymousToken;
            return config;
          }
        };
      }
    
```
