## Introduction
Recently, we released a tutorial on the [Backand GitHub page](https://github.com/backand/angular-yeoman-todos/tree/todo-with-users) that shows off a lot of the features available by default in a Backand-powered application. After completing the tutorial, you'll have a simple ToDo list application with full support for user and role-based security. Below we'll discuss each of the features demonstrated in the tutorial, and cover any items remaining that are necessary for a full deployment.

## RESTful API Out-of-the-Box

One of the first things we do in the tutorial, after actually creating the application with Backand, is create a new database using our JSON-based schema language. This powerful tool allows you to quickly build a database and the associated tables and objects without having a client installed. More importantly, once the database has been created, Backand automatically creates a RESTful API based on your underlying database schema. This allows you to have immediate access to your database via a series of REST API calls that create, retrieve, update, and delete records at whim. With some simple JavaScript, you can quickly build out a data layer that would take months in a stand-alone project! Furthermore, the moment you make a change to your schema, Backand will detect the change and update the API endpoints accordingly. This automated updating greatly eases the process of migrating database schema changes, and takes some of the load of server programming off your shoulders, allowing you to focus on your application's functionality.

## Security Roles

While the RESTful API is indeed a powerful tool, the main goal of this tutorial is to explore user- and role-based security. Through Backand's dashboard you are granted a powerful array of tools that you can leverage to secure your underlying application. You can assign user-specific security settings for each object in your app, including everything from table update calls to custom triggers and actions. 

However, doing this for every user that signs up for your application would be incredibly tedious. Luckily, Backand provides User Roles that you can assign to the users in your application. Roles set a basic template for interacting with your application's API, allowing you to restrict all users with that role to only specific actions in your application. These roles can also be overridden at an endpoint level, allowing you to grant roles specific access to resources while still maintaining their general security profile.

After the tutorial has been completed, you will have three roles for your users – administrators who have full control, users who can view all records in the application (but only update or delete those that they create themselves), and read-only users that are only able to view the ToDo items in your application.

## Login and Anonymous Access

In addition to security roles, this tutorial will introduce you to the user access functionality available in Backand. You'll implement a mechanism for inviting users to register for your application, and enable two-step verification for your application's users, and all of the complexity will be managed by Backand! Furthermore, you'll learn how to enable anonymous access to your application through a few simple administrative dashboard settings. After the tutorial has been completed, you will have a fully secure application that allows you to invite users to the system, automatically assign them a security role, and even enable anonymous access to your ToDo list should you wish to do so.

## Remaining Items

While the tutorial will walk you through the entire application – from inception to completion – there will still be a few tasks remaining before you can show off your new Backand-powered ToDo list app. The first section of the tutorial focuses on setting up a local application development environment, which allows you to develop and run the code on your local machine. However, once you've implemented the code and are ready to show it off, you still need to track down a web server to use for deployment. Luckily this web server does not need to be particularly robust, as your application requires zero server-side customization – simply deploy the application code to a publicly-accessible folder and you are set!

Once you've published your application on a web server, you have a few more steps remaining in order to fully deploy the application. As a part of the tutorial you set several internal sign-up and authentication URLs that point at your local development instance. Obviously these will no longer function once you have deployed the code! Simply change the appropriate user security settings in your application's Backand dashboard, and your application will be ready to show the world!

## Summary

This tutorial gives you a quick look at the power offered by Backand's back end tools and APIs. You'll explore creating a database and implementing a RESTful API for that data with zero effort. You'll customize your application's security and learn how to restrict users based on role and based on endpoint. You'll even spend some time working with custom server-side actions that execute on-demand. By the time you have finished the tutorial, you will have implemented a secure ToDo list application with user authentication – and all with minimal effort!
