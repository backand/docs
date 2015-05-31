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
## Security configuration
### Anonymous Access
### Public App
### Custom pages
### Sign up
### Security Actions
## Security Templates
## Users
## Team collaboration
# Log
## Configuration
## Data History
## Server Side Exceptions
# Settings
## App
## Database