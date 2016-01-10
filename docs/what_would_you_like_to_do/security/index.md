## Introduction
One of the core concerns of any application that deals with sensitive user and business data is security. In this article we'll cover how Backand handles security for your application, covering the entire stack – from user registration to security templates. We'll also discuss anonymous access to your application, which can greatly enhance your application's adoption by the world at large. Backand takes your application's security seriously, and with the following we'll show you how we protect your data effectively.

## User Registration

The security process starts with user registration. Backand uses two-factor authentication by default – requiring you to first invite users to your application, then directing them to a registration page that you configure. Backand takes care of the communication, handling the verification (configurable) and registration processes from your customizable sign-up page. This entire process is known as “private” mode, which is the default for all new applications – it requires that users are invited to join the app.

Once your app is stable and your security is fully configured (or even before if you're confident in your data), you can open your application up to the public. This allows users to sign up to your application directly, without having to be invited. Once they register, Backand sends the user a verification email to ensure that the sign-up is legitimate, if the sign-up email verification setting is enabled.. Once the new user confirms the registration, Backand redirects them to a custom verified email link, representing their successful registration with the system. The user is assigned a default role (see below), and is free to use your app within the permissions framework that you constructed.

## Role-based Security

Backand uses role-based security to secure access to your application data. This is accomplished by assigning each user in your application a role. Roles have varying permission levels throughout your app, from object-specific restrictions on record modification all the way up to full access to your application's configuration. Each role's permission set is configurable at a macro level as well as at an object level, allowing you true flexibility in securing your application.

New users created from the admin dashboard are created in the “admin” role. One of your first steps should be adding an additional security role for new users – whether invited in private mode or registering organically in public mode – that will serve as the default for user sign-ups. Once this default role is set, all new users are assigned the new role automatically. You can then make any permission or access changes you need to from the administrative dashboard.

## Security Templates

Setting a security level for every single object and user class in your database has the potential to be a tedious, error-prone process. Luckily Backand provides a solution through Security Templates. Security Templates define a default set of security permissions for all of the roles in your application that can then be applied to each of the Create, Read, Update, and Delete actions in your database. Templates can be assigned at both the object and query level, allowing you flexibility in configuring user access to your data.

Sometimes, though, a blanket approach isn't sufficient for the final functionality of your application. In those cases, Backand allows you to override security templates at a per-role level. This ability gives you more granular control over your application's data, allowing you to grant or revoke access to sensitive components as necessary. Through use of Security Templates and Roles, you will be able to fully secure your application and ensure only authorized users can perform actions against your data.

## Anonymous Access

In many cases, particularly when working with your application as an API, it is beneficial to grant anonymous access to a specific user. This user can be someone arriving at your site organically, or can be another web service hitting your application's RESTful endpoints. To accommodate these cases, Backand provides anonymous access. This uses token-based authentication to ensure that the anonymous use case is valid and that the user in question is allowed to interact with your app. Furthermore, you can restrict anonymous access to your application based on any of the underlying user roles, ensuring that anonymous uses can only access the elements that you want them to.

## Summary

Securing your application is a priority task, and crucial to ensuring your application's success. Through Backand's use of two-factor authentication to drive registration, you can be sure that the users coming into your application are a low fraud risk, and that both their experience – and your data – are protected. Additionally, you can secure your app through the use of user roles and security templates, which allow you to quickly and easily manage the permissions in your application. Finally, you can grant some use cases anonymous access to your application, easing integration with third parties and handling organic traffic with ease. With Backand's security offerings, you can rest assured that your application is safe and protected.
