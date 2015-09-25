# Introduction

The following Security Concepts section describes the Backand concepts of how to manage your app's users. A key issue is to understand that by default there are two independent users objects. One is handled by Backand and is responsible for authentication and role based security. Its data is stored in Backand's local database and cannot take part in your app logic. We offer the other "users" object in our default model. This object should serve you in your app logic and allow you to associate this users object to other objects in your app. Backand provides sync actions to maintain those two objects in sync. (see [Link your app's users with Backand's registered users](security.md#Link your app's users with Backand's registered users) for more info. Along with reading this documentation, we highly recommend to run the [todos-with-users app](https://github.com/backand/todos-with-users). It is a simple app that covers most the users management use cases. Such as allowing users to read all the data but to write and update only theirs, allowing anonymous users just to read and allowing Admin users to write on all the items.

# Authentication

The default authentication setup for Backand applications relies on [OAuth2](http://oauth.net/2/) to provide tokenize authentication. By logging in with your username (your email address), your password, and your app name, you receive an authentication token that is valid for 24 hours. This token is required for all communication with Backand, and as such we highly recommend that you use [Backand's SDK](https://github.com/backand/angularbknd-sdk) to help you manage the access token. You can change the default 24 hours token expiration or to make it last forever by using a refresh token. The refresh token is an encryption of the master and user keys. By changing them you can revoke one or all of you users refresh tokens and re-authenticate them. To ease to work with other servers you can also use [basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) where the username is the master key and the password is the user key. Backand also offers Anonymous Access, which allows you to access your application without the need to authenticate via username and password. For more, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#user-authentication)

# Sign Up
Registering with Backand, and creating an application, automatically sets you as a user with an "Admin" role in your new project (see [roles](security.md#roles) for more info). By default your application is marked private, which means that only users that you have personally invited can sign up for your app. This can be changed in the dashboard by setting your application to Public in the Security & Auth --> Configuration menu. Setting your application to public allows any user to register for - and use - your application. These users are assigned a default role, which needs to be configured when you enable public usage of your app (see [roles](security.md#roles) for more details). For security reasons you cannot change the role of a user through API calls - this can only be accomplished either by having an admin change the appropriate settings on the Security & Auth -> Users page, or by creating a custom server-side action that performs this task. For a private application the registration steps are as follow:

* First, set your application's registration page on the Security & Auth --> Configuration page.
* Next, if you wish to use automated email verification, enable Sign-up Email Verification on the Security & Auth --> Configuration page
* Once you have a registration page, enter the emails to invite on the Security & Auth --> Users page. 
* Once you have entered the emails you wish to invite, click the "Invite User(s)" button. This will send an email to each new user with a link to the registration page that you created.
**The following steps are the same for both Public and Private apps:**
* The user enters his/her email, name and password on your registration page
* After the user submits their registration, Backand sends a verification email to authenticate the user's identity if Sign-up Email Verification is enabled
* If Sign-up Email Verification is not enabled, at this point registration is complete.
* If Sign-up Email Verification is enabled, after the user clicks on the link in the verification email, Backand completes the registration process and redirects to the "Custom Verified Email Page" URL for your application. Configure this on the Security & Auth --> Configuration page  
For more, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#sign-up)

# Remove user from the app
There are two ways to remove a user from the application. You can permanently remove a user from the application by deleting a user from the Registered Users grid. This requires the user to register again if they wish to continue using your app. Alternatively, you can un-check the "approved" checkbox in the user's row on the Security & Auth --> Users page. This allows you to reinstate the user simply by re-checking the "approved" column.

# Anonymous Access
If you wish to enable anonymous access to your application, you need to perform a couple steps. First, you need to turn on the Anonymous Access option in you application's settings. Once that's done, you are given the option to create an AnonymousToken value. By passing this value in a request header (e.g. AnonymousToken=<your token here>) you can perform api actions for your application. finally, you need to set the default role of an anonymous user within your application's settings (see [roles](security.md#roles) for more info).  For more, see the [API Description](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#anonymous-access)

# Link your app's users with Backand's registered users

Backand maintains an internal registered users table which is used to manage your application's security. However, most apps will have their own "users" table, which is used when implementing the app's business logic. Recognizing this, we have created automatic and custom trigger actions that can be used to synchronize the two tables. If you have an object in your system named `users`, and if that object has the fields `email`, `firstName`, and `lastName`, then every user that is registered with Backand for your app will automatically have an entry created in your custom `users` object. This takes place no matter how the user is added - both the signup API and the Backand dashboard will create an automated user record! Additionally, every time you add a user instance to your users object, a new user is created in Backand's internal registered users table. This new user will be given a randomized password, which can then be provided to the user for access to your application.

Below we'll look more in-depth at how this process is managed. Additionally, we'll explore what happens when the users object has an unexpected name (i.e. something other than "users"), or if the fields of the users object are named differently.

There are 3 sync actions located in Configuration -> Security & Auth that are triggered by Backand user registration:

1. Create My App User
1. Update My App User
1. Delete My App User. 

Those are Transactional SQL actions. You can directly modify the SQL statements used to match your application's specific table structure. By default, Backand sets the Where Condition of the action to "false" if it detects that you do not have a users object in your model, meaning that the action will never run. Thus, it is important that after modifying the SQL for your object model, you then set the "Where" condition to "true".

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
This action implements the other side of the relationship - after creating an instance in your custom `users` object, this code creates the corresponding entry in Backand's internal users table. If have named your custom user object anything other than "users", you can add the above action into that object manually to achieve the same functionality. You will need to adjust the backandUser object creation to use the columns in your specific object, as the userInput object may have a different structure than that assumed above.

# <a name="roles"></a>Roles & Security Templates

Each user has a role. When you created your app, you were automatically assigned with an "Admin" role. The "Admin" role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.

