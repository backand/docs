# Getting Started

Backand's REST API allows you to fully access and manipulate the backend data for your system, giving you full access to the underlying database and related functionality from any program that can send an HTTP request. While we start by wrapping your database in a true RESTful API Interface, we also give you a lot of extras that you can build into your app:

* You can define and execute custom queries that are triggered by a call to a URL
* You can define and execute custom actions that execute whenever you hit a specific endpoint. These can be as simple as sending an email, or as complex as a custom server-side javascript function.
* You can manage user security and access to your application automatically through Backand's security and authorization system
* You can test every single endpoint in your system using Backand's API Playground
* You can get immediate and up-to-date looks at your database's underlying tables and stored data, and even perform real-time manipulations of that data
* You can create, retrieve, update, and delete every object in your system through a simple web call, and return as much - or as little - detail as you need

Let's take a quick look at each available action in the Backand dashboard, and get started with focusing on the front-end of your application!

# Quick Reference

Here are a few basic URL guidelines for quickly accessing your application's data:

## Base URL

The base URL for your application will be `https://api.backand.com:8078/1` - all of your access will be driven from this URL

## Request Format

All requests must be made using JSON request bodies. Make sure to set the `Content-Type` header to `application/json`. Additionally, all requests must include an authentication token in the request headers. This process is deatiled in our [documentation](http://docs.backand.com/en/latest/security/index.html).

## Response Format

All responses are returned as JSON objects. The status code of the response indicates the success (2xx) or failure (4xx) of the request.

## Security & Authentication
|| URL || HTTP Verb || Functionality ||
| /token | POST | Obtains a 24-hour acccess token. A Backand username and password must be provided, along with the application name |
| /user/signup | POST | Registers a new user with the application. Must use a SignUpToken, which is configured for the application |


## Manipulating objects
|| URL || HTTP Verb || Functionality ||
| /objects/{object name} | GET | Gets a list of all objects of type {object name} |
| /objects/{object name} | POST | Creates an object of type {object name} |
| /objects/{object name}/{id} | GET | Gets an object of type {object name} with ID {id} |
| /objects/{object name}/{id} | PUT | Updates an object of type {object name} with ID {id} |
| /objects/{object name}/{id} | DELETE | Deletes an object of type {object name} with ID {id} |

## Custom Object Actions
|| URL || HTTP Verb || Functionality ||
| /objects/action/{object name}/{id}?actionName={action name} | GET | Executes custom action {action name} on object {object name} with an id of {id} |

## Queries
|| URL || HTTP Verb || Functionality ||
| /query/data/{query name} | GET | Executes custom query {query name} |

# Objects

The "Objects" section of the dashboard allows you to modify the specific objects in your application's database. A menu entry is created for each table in your database, and allows you to configure your application at the object level. For each object in your system, you are given access to the following areas to modify and work with.

## Fields

The Fields tab allows you to modify details about each column in the object's database table. For every column, you can perform the following actions:

* Change the name
* Make the column searchable in the text search filter field
* Add a default value
* Mark the column as required
* Enable server-side validation, allowing you to set minimum and maximum values for numeric columns
* Exclude the field from any updates, to prevent the field from changing when the object is updated
* Exclude the field from any creates, to prevent the field from being populated during object creation

## Actions

The Actions tab allows you to define custom actions on your object.

### On Demand

On Demand actions take place only when you hit a specific endpoint (see the Quick Reference section for the endpoint format).

### Database-context Actions

You can create actions that execute within the following database transaction contexts

* Create - during an object's creation stage
* Update - when an object is being updated
* Delete - when an object is being deleted

You can also customize at which point of the database transaction the action occurs:

* Before - The action executes before the database transaction takes place.
* During - The action executes within the context of the relevant database transaction. If the transaction does not complete successfully, any data changes are rolled back.
* After - The action executes after the database transaction completes. If the transaction does not complete successfully, the action does not execute.

### Testing

For every action you create, you can also test the action from this interface. This allows you to verify that your actions are performing as expected at each stage of the transaction. The testing process also gives you the URL that was used to execute the action, allowing you to replay it at will and modify the item data to suit specific scenarios.

## Security

The Security tab allows you to restrict access to the database actions for this object, dynamically adjusting the ability to CREATE, READ, UPDATE, or DELETE by user name or user role.

### Pre-defined Filter

The pre-defined filter can be used to add additional restrictions to the data loaded, such as only loading data associated with the current username.

### Security Template & Override

This allows you to select a security template to apply to the object or, if you choose, override the selected security template with the following security settings.

### Security Settings

The security settings allow you to enable or disable actions on the object based upon user roles.

## Settings

The Settings tab allows you to modify high-level aspects of the object, such as the object name. It also allows you to enable change tracking for this object, and set the column that contains the object's description. This is used when setting the object as a relatedTable relation to another object, and will drive the contents of each drop-down and multiselect provided as a result.

## Data

The Data tab contains an editable data grid representing the current list of objects stored in the database. You can create, modify, or delete the records as you see fit.

## REST API

The REST API tab gives you a listing of the basic RESTful API endpoints available for this object. You can test each endpoint, and see the URLs used in action.

# Queries

Queries are custom database queries, written in SQL, that you can later hit through a specific endpoint (see Quick Reference for details on the URL format).

## Create a query

You can add a new query by clicking on "+New Query" in the menu bar. This allows you to name the query, provide any parameters necessary, and then write the query. It also allows you to set a specific security template for the query to use as it executes, and allows you to override the security template settings when running the query.

## Update an existing Query

To update an existing query, select the query from the navigation menu and press the "Edit" button.

## Test a query

You can test both new and existing queries using the Test query parameters panel on the right hand side of the page. This executes the query, returning both the data of the query and the URL used to execute the query.

# Security & Auth

The Security & Auth section allows you to modify the security settings for your application, setting both application-level and user-level security policies.

## Security configuration

The Security Configuration section allows you to set application-wide settings for access to your project.

### Anonymous Access

When enabled, this allows any user with a link to access your application. You can set a role for anonymous users, and obtain an anonymous access token to use in your application's code.

### Public App

The Public App settings determines whether users can sign up for your app, or if they must be invited. You can also assign a role to each invited user.

### Custom pages

These custom pages are pages within your application code that are used for user registration and login. The Custom Registration Page URL is supplied in the invitation email sent to new users. The Custom Verified Email Page URL is sent to the user after they have verified their email address. Note that these are pages on your own servers, and not a part of Backand.

### API Signup Token

THe API Signup Token is a token that you use whenever you contact the Backand API to register a new user with your application.

### Security Actions

The actions on this page, much like those on the other pages of the application, allow you to define custom actions to occur at each point in the transactional CRUD conversation against a database table, or when called by accessing a specific URL. These actions, though, are tied specifically to the internal Backand Users table, which manages your application''s security. All of the standard custom action options are available for use in this section, and you can easily create new actions, edit existing actions, and test the actions you have created.

## Security Templates

Security templates allow you to create a template that is used to set the permissions of a database table's object. You can create new templates, update existing templates, and rename security templates as you see fit. Each of the checkboxes corresponds to the database action indicated by the column header. When you check "Create" for a user role, for example, and then assign that security template to an object, all users with the specified role will be granted "Create" access to the object.

## Users

The users page lists all users currently registered for your app. You can edit existing user details, delete a user, or invite a new user to your application via email.

## Team

The Team section allows you to add new administrative users to your application. Admin users have full access to the application dashboard, and your application's configuration. You can perform all of the same actions available on the Users page on the Team page as well.

# Log

The Log section stores various types of activity logs for your application.

## Configuration

The Configuration log page tracks all application configuration changes that have occurred. View this for a list of settings changes made through the Backand Admin Dashboard.

## Data History

The data history log logs all data changes for those objects that have change tracking enabled.

## Server Side Exceptions

The Server Side Exceptions log shows all server-side exceptions generated by your application. These most commonly occur during custom server-side javascript actions.

# Settings

The Settings section covers the basic settings for your application.

## App

In the App settings page, you can change the name of your application, change the title, change the default date format, and set the default number of items to be presented whenever data is paginated.

## Database

The Database section contains all of the settings related to your application's database. You can modify the existing connection to connect to another database, or obtain the parameters necessary to modify the application's current database remotely.