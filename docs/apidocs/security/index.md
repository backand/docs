## Introduction

The following section describes how to manage your app's users within Backand's user security paradigm. An important element to note up front is that, by default, [Backand](https://www.backand.com) provides two independent user objects. One user object is handled entirely by Backand, and is responsible for authentication and role-based security. The data for this user object is stored in Backand's database, and as such cannot partake of your app's custom logic. The second user object is offered in the default data model for a Backand application, and is titled "users". This object represents a user of your application, as opposed to a generic Backand user, and allows you to associate the user with various other objects in your application. Backand provides sync actions that allow you to keep these two user object lists up-to-date (see [Link your app's users with Backand's registered users](security.md#Link your app's users with Backand's registered users) for more info). We highly recommend running the [todos-with-users app](https://github.com/backand/todos-with-users) in addition to reading this documentation. This simple app covers most of the user management use cases for a Backand application, such as allowing users to read all of your app's data but only allowing them to create and update their own objects, or restricting anonymous users to read-only access, or creating an Admin role that has full read-write-update-delete access to your application's objects.

## Authentication

### OAuth 2.0

The default authentication setup for [Backand](https://www.backand.com) applications relies on [OAuth2](http://oauth.net/2/) to provide tokenized authentication. By logging in with your username (your email address), your password, and your app name, you receive an authentication token that is valid for 24 hours. This token is required for all communication with Backand, and as such we highly recommend that you use [Backand's SDK](https://github.com/backand/angularbknd-sdk) to help you manage the access token. You can change the default expiration of 24 hours by using a refresh token, which allows you to reuse the authentication token indefinitely. The refresh token is an encrypted hash of the master and user keys. You can revoke one (or all) of your user's refresh tokens by changing the refresh token and requiring users to re-authenticate. For more information, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#user-authentication).

To obtain an access token using a username and password, use the following command:

```curl

curl https://api.backand.com/token -d username=guest@backand.com -d password=guest1234 -d grant_type=password -d appName=bkndexample

```

To obtain an access token using the Social Media master key, use the following command:

```curl

curl -X GET https://api.backand.com/1/user/facebook/token?accessToken=EAAPutOqBPlkBANfUQXdmp7xseF16JpSknTGxZBBZAwd1TDigDUqC9i5NixlDOKFNpNQQwqJFHHPs059STwG0qzodlfzOvnE2q4EPxXM43ZBZBUOV44lCjpmFhMwJmeXUgRSBlwxJaKrvZAH7vW7NdBQa3dMLQZAv4RTXRZAcgqNVQZDZD&appName=bkndexample&signupIfNotSignedIn=true

```

Parameters:
--signupIfNotSignedIn - (Optional, default false) - If the user tries to sign in without first registering for the application, the user will receive an error message ("The user is not signed up to {appName}"). If this value is set to true, then the user will be automatically registered with the app if they have not yet been signed up

You can then use the access token to call any Backand API for your application:

```curl

curl https://api.backand.com/1/objects/items -H "Authorization: Bearer OfKDiw6fQwfhmUcoh3WuXken_Sgj18za8SDqn1Ed2S_qI88HWNwGBSpG2ktUkSbNIwcGn6dwos7ivvVARC1UIiCf8fzlNynFNZamCZPDpbOhRjheq3RnU6HJiCbqlks57PLvMnf9PxGK8D3FU8MeaPyUk7mmbAtvgc6GF-9s13YpFjVQkM5XRZ-CsuWjaRoukygLMOivj1iPYxBB4c-hu6ocZKQljiSnw6rroYbD4spmUIkwDnC0rz1jP9ln5KNI3KmO0KLcWJF6E_zWfepdFw"

```

### Basic authentication 

You can also use [basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication) to ease working with other servers - simply enter the master key as the username, and the user key as the password. You can use this approach to obtain a list of Items by using the following API call:
 
```bash

curl https://api.backand.com/1/objects/items -u <master key>:<user key>

```
You can also send the basic authorization token through as a query string:

```bash

curl https://api.backand.com/1/objects/items?authorization=basic+<master key>:<user key>

```

**Warning**: Do not use this method from the client as it exposes your app's secret tokens - this is for server-side calls only.

### Anonymous authentication 

Backand also offers Anonymous Access, which allows you to access your application without the need to authenticate via username and password. By passing this value in a request header (e.g. AnonymousToken=<your token here>) you can perform api actions for your application:

```bash

curl https://api.backand.com/1/objects/items -H "AnonymousToken: <anonymous token>"

``` 

You can also send the anonymous token through in a query string:

```bash

curl https://api.backand.com/1/objects/items?AnonymousToken=<anonymous token>

``` 

## Sign Up

Registering with [Backand](https://www.backand.com), and creating an application, automatically sets you as a user with an "Admin" role in your new project (see [roles](security.md#roles) for more info). By default your application is marked public which mean any user can register to your application.
These users are assigned a default role 'User', which has full CRUD access to your app. You needs to be configured when you enable public usage of your app (see [roles](security.md#roles) for more details).

**Note**: For security reasons you cannot change the role from the sign-up API - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -> Registered Users page, or by creating a custom server-side action 
with Admin rights. Create new Security Action in Before Create trigger with the following code (this will update the 
role to 'Public'):

```JavaScript
  userInput.Role = 'Public';
```

### Saving Additional Parameters During Sign-up

In many cases, we would like to collect additional information from the user during sign up. We can automatically populate the related fields on the Users object as a part of the sign-up request by using the **parameters** parameter for the sign-up call. The server, during the "Create My App User" action, will translate these key-value pairs into the appropriate fields on the "Users" object - as such, it is important that all fields provided already exist on the Users object. The following example demonstrates specifying a user's "company" name as a part of the sign-up:

1. Update the Model and add the field **company** to the **users** object:
    1. Go to **Objects --> Model**
    2. Add this line as a data field in the **users** object model: **"company": {"type": "string"}**
    3. Click on **Validate & Update**

2. When calling Backand.signup(), send the **parameters** objecct through as the last input parameter for the request. The code will resemble the following:

```javascript
    Backand.signup(firstName, lastName, username, password, password, {company: self.company}).then(...);
```

3. There's no need for server-side modificcations - values for "parameters" are handled automatically by the action 'Create My App User' (found in the **Security Actions** section).

4. Modify the UI to collect the 'company' name for the user.
    
Now, when sign-up is completed, you can see the company name in the **users** object's Data tab.

### Private app

You can change the default public app to be private. This can be changed in the dashboard by setting the Public flag for your application to 'false' in the Security & Auth --> Configuration menu. For a private app, the registration steps are as follow:

* First, set your app's registration page on the Security & Auth --> Configuration page.
* Next, if you wish to use automated email verification, enable Sign-up Email Verification on the Security & Auth --> Configuration page
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page. 
* Once you have entered the emails you wish to invite, click the "Invite User(s)" button. This will send an email to each new user with a link to the registration page that you created.

### Email verification process

The user enters his/her email, name and password on your registration page:

* After the user submits their registration, Backand sends a verification email to authenticate the user's identity if Sign-up Email Verification is enabled
* If Sign-up Email Verification is not enabled, at this point registration is complete.
* If Sign-up Email Verification is enabled, after the user clicks on the link in the verification email, Backand completes the registration process and redirects to the "Custom Verified Email Page" URL for your application. Configure this on the Security & Auth --> Configuration page  

For more information, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#sign-up).

## SSO (Single Sign On)
Many organizations make use of tools like Active Directory in order to provide a central source of user-based authentication. This system is often used to drive a Single Sign On (SSO) feature in the organization, allowing users to simply use one set of credentials for all of the apps that they need to work with. Backand provides a method to incorporate SSO functionality using an override server-side action. This action allows you to perform your own authentication (via web-service calls to the domain controller, for example), and lets you return one of three actions. 

* Returning "allow" allows the user access to the requested resource = in this case, you can return additional information in the "additionalTokenInfo" variable, which is added to the Backand authentication result. You may access this later by using the getUserDetails function of the Backand SDK. 
* Returning "deny" overrides Backand's auth, and provides an "Access Denied" mesage. 
* Returning "ignore" allows you to ignore the custom action, instead relying upon Backand's default authentication methods. 

You can configure this action in the Backand dashboard, under Security & Auth => Security Actions.

## Remove user from the app
There are two ways to remove a user from the application. You can permanently remove a user from the application by deleting a user from the Registered Users grid. This requires the user to register again if they wish to continue using your app. Alternatively, you can un-check the "approved" checkbox in the user's row on the Security & Auth --> Users page. This allows you to reinstate the user simply by re-checking the "approved" column.

## Anonymous Access
By default the anonymous access is turned on with 'User' role access, which mean any one can register to your app and make CRUD actions on any object.
If you wish to disable anonymous access to your application, go to Security & Auth --> Configuration and set 'Anonymous Access' to off.

**Note**: To better control the permission of your app we recommend to change the anonymous users role to have less access. For more information on anonymous access, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#anonymous-access).

## Link your app's users with Backand's registered users
[Backand](https://www.backand.com) maintains an internal registered users object which is used to manage your app's 
security. However, most apps will have their own "users" object, which is used when implementing the app's business logic. Recognizing this, we have created automatic and custom trigger actions that can be used to synchronize the two objects. If you have an object in your system named 'users', and if that object has the fields 'email', 'firstName', and 'lastName', then every user that is registered with [Backand](https://www.backand.com) for your app will automatically have an entry created in your custom 'users' object. This takes place no matter how the user is added - both the sign-up API and the [Backand](https://www.backand.com) dashboard will create an automated user record! Additionally, every time you add a user instance to your users object, a new user is created in Backand's internal registered users object. This new user will be given a randomized password, which can then be provided to the user for access to your application.

Below we'll look more in-depth at how this process is managed. Additionally, we'll explore what happens when the users object has an unexpected name (i.e. something other than "users"), or if the fields of the users object are named differently.

There are 3 sync actions located in Configuration -> Security & Auth that are triggered by Backand user registration:

1. Create My App User
1. Update My App User
1. Delete My App User. 

You can directly modify those JavaScript actions if needed. 
**Note**: By default, Backand sets the Where Condition of the action to "false" if it detects that you do not have a users object in your model, meaning that the action will never run.

If you have a users object in your model, Backand adds the following actions, which run before and during user creation

* Before Create: Validate Backand Register User

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
	var validEmail = function(email) 
    {
        var re = /\S+@\S+\.\S+/;
        return re.test(email);
    }
    
    // write your code here
	if (!userInput.email){
        throw new Error("Backand user must have an email.");
    }
    
    if (!validEmail(userInput.email)){
        throw new Error("The email is not valid.");
    }
    if (!userInput.firstName){
        throw new Error("Backand user must have a first name.");
    }
    if (!userInput.lastName){
        throw new Error("Backand user must have a last name.");
    }
}
```

* During Create: Create Backand Register User

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
	
	var randomPassword = function(length){
	    if (!length) length = 10;
	    return Math.random().toString(36).slice(-length);
	}
    if (!parameters.password){
        parameters.password = randomPassword();
    }
	
    var backandUser = {
        password: parameters.password,
        confirmPassword: parameters.password,
        email: userInput.email,
        firstName: userInput.firstName,
        lastName: userInput.lastName
    };
    
    // uncomment if you want to debug debug
    //console.log(parameters);
	
    var x = $http({method:"POST",url:CONSTS.apiUrl + "1/user" ,data:backandUser, headers: {"Authorization":userProfile.token, "AppName":userProfile.app}});

    // uncomment if you want to return the password and sign in as this user
    //return { password: parameters.password };
    return { };
}
```

This action implements the other side of the relationship - after creating an instance in your custom 'users' object, this code creates the corresponding entry in Backand's internal users object. If have named your custom user object anything other than 'users', you can add the above action into that object manually to achieve the same functionality. You will need to adjust the backandUser object creation to use the columns in your specific object, as the userInput object may have a different structure than that assumed above.

## Roles & Security Templates

Each user has a role. When you created your app, you were automatically assigned with an "Admin" role. The "Admin" role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.

