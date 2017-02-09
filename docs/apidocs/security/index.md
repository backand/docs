
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

```javascript
function backandCallback(userInput, dbRow, params, userProfile) {
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

```javascript
function backandCallback(userInput, dbRow, params, userProfile) {

  var randomPassword = function(length){
      if (!length) length = 10;
      return Math.random().toString(36).slice(-length);
  }
    if (!parameters.password){
        params.password = randomPassword();
    }

    var backandUser = {
        password: params.password,
        confirmPassword: params.password,
        email: userInput.email,
        firstName: userInput.firstName,
        lastName: userInput.lastName
    };

    // uncomment if you want to debug
    //console.log(params);
    var x = $http({method:"POST",url:CONSTS.apiUrl + "1/user" ,data:backandUser, headers: {"Authorization":userProfile.token, "AppName":userProfile.app}});

    // uncomment if you want to return the password and sign in as this user
    //return { password: params.password };
    return { };
}
```

This action implements the other side of the relationship - after creating an instance in your custom 'users' object, this code creates the corresponding entry in Backand's internal users object. If have named your custom user object anything other than 'users', you can add the above action into that object manually to achieve the same functionality. You will need to adjust the backandUser object creation to use the columns in your specific object, as the userInput object may have a different structure than that assumed above.

## Roles & Security Templates

Each user has a role. When you created your app, you were automatically assigned with an "Admin" role. The "Admin" role is a special role that allows you to make configuration changes in your app with Backand's administration tools. Non-admin roles, which should be used to secure your application, are used to define the permissions for each of the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions for your objects and queries. Each Object and Query in your application is associated with a Security Template. In the security template, you assign different permissions for the possible [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions to each user role, allowing you to control your user's access to the system's create, read, update and delete actions for your objects. Template configuration is found on the Security & Auth --> Security Template page.

While Security Templates provide reusable permissions platforms that can be spread across a number of roles, you can also override security template settings and provide specific permissions for each individual role. This allows you to have more granular control over the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) actions that can be performed by the users in your system. When a user with insufficient security access tries to perform an action for which they do not have permission, a 403 (Forbidden) error response is returned.
