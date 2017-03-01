# Backand Dashboard

Backand's REST API allows you to fully access and manipulate the backend data for your system, giving you full access to the underlying database and related functionality from any program that can send an HTTP request. While we start by wrapping your database in a true RESTful API Interface, we also give you a lot of extras that you can use to replace the server in your app:

* You can define and execute custom queries that are triggered by a call to a URL
* You can define and execute custom actions that execute whenever you hit a specific endpoint. These can be as simple as sending an email, or as complex as a custom server-side javascript function.
* You can manage user security and access to your application automatically through Backand's security and authorization system
* You can test every single endpoint in your system using Backand's API Playground
* You can get immediate and up-to-date looks at your database's underlying tables and stored data, and even perform real-time manipulations of that data
* You can create, retrieve, update, and delete every object in your system through a simple web call, and return as much - or as little - detail as you need

Let's take a quick look at each available action in the Backand dashboard, and get started with focusing on the front-end of your application!

## Docs & API
The Docs & API tab contains links to kickstart tutorials, tutorials on converting existing applications to use Backand, and other useful documentation. let's take a look at each menu category below.
### Examples
This links to our [examples page](https://www.backand.com/apps/), which has useful sample applications for many different platforms. These sample applications work to show how best to integrate Backand with an application for each mentioned platform, and can be a good starting point for integrating Backand with an existing (or brand new) application.

### Kickstart Tutorials
Our kickstart tutorials provide a list of kickstart apps that you can use to build an app for the platform specified. These kickstart apps ship with Backand already built-in and integrated, and provide you with tools to explore the SDK's functionality. They all work with the default data model, so getting the kickstart app wired up to a new Backand application is a simple matter of changing the appropriate keys in each example.

<aside class='notice'>
We provide a platform selector, which allows you to only view the kickstarts, existing apps, and code samples throughout the dashboard that are relevant to your needs. Simply choose a different platform, and the dashboard will present all examples in the relevant version of JavaScript that matches your selection.
</aside>

### Existing App Tutorials
Our existing app tutorials provide a list of tutorials on connecting Backand into your existing application, with a number of different frameworks supported. These focus on including the Backand SDK, connecting to your app, and working with the data model. If you'd like to see another platform included, please [contact us](https://www.backand.com/contact/)!

<aside class='notice'>
We provide a platform selector, which allows you to only view the kickstarts, existing apps, and code samples throughout the dashboard that are relevant to your needs. Simply choose a different platform, and the dashboard will present all examples in the relevant version of JavaScript that matches your selection.
</aside>
### Realtime Database
### Documentation
### REST API Playground
## Objects
The "Objects" section of the dashboard allows you to modify the specific objects in your application's database. A menu entry is created for each table in your database, and allows you to configure your application at the object level. For each object in your system, you are given access to the following areas to modify and work with.

### Model
> This sample model JSON creates two objects - Items and Users

```json
[
  {
    "name": "items",
    "fields": {
      "name": {
        "type": "string"
      },
      "description": {
        "type": "text"
      },
      "user": {
        "object": "users"
      }
    }
  },
  {
    "name": "users",
    "fields": {
      "email": {
        "type": "string"
      },
      "firstName": {
        "type": "string"
      },
      "lastName": {
        "type": "string"
      },
      "items": {
        "collection": "items",
        "via": "user"
      }
    }
  }
]
```

The Model parent tab allows you to modify your database's model structure, adding new objects and establishing the relationship between them. This can be done using either our GUI model editor, or the JSON schema editor. We also provide a tab demonstrating how to make - and sync - changes to your database using an external database tool (like MySQL Workbench). Below is a brief overview of how to use this interface.

<aside class="notice">
In addition to the fields supplied by the user, Backand defines an 'id' field, of type integer, which will be used as a primary key for the table.
</aside>

<aside class="notice">Backand offers support for automated population of created and updated timestamps. Simply add the `createdAt` or `updatedAt` fields to your model, and if they are present Backand will automatically populate them with the timestamp of the most recent POST or PUT request to that object.</aside>

#### Model Diagram
The Model Diagram tab provides a GUI editor for working with your app's database schema. It works with the model JSON in the background to orchestrate changes to your database. Using the GUI you can easily add new objects, update existing objects, create relations between objects, and all other related database modification tasks.

#### Model JSON
The Model JSON tab provides a straightforward method of editing your database schema using well-formed JSON. You're given a pane to perform your edits, and a comparison pane so that you can easily see what has changed from the current schema. Once your changes are finalized, click "Validate & Update" to finalize your changes.

#### Database (pending)
This tab describes how to make the changes you desire in your model directly in the database. Once you've made your changes via whichever RDBMS tool you prefer, you can click the "Sync your database" button to pull the changes into Backand, updating your schema and object list.

### Object List
For each object in your schema, you are provided a dedicated management area for that object. These are accessed via a list beneath the "New object" entry.

Each object has a set of tabs provided that allow you to modify either the object's data, or the object itself. These tabs are Data, Actions, Security, Settings, and REST API

<aside class="notice">You can also use the "New object" to quickly create a new object to add to your system.</aside>
#### Data tab
The data tab provides you with a quick method by which you can add new records, update existing records, or remove records from your database. For every action you take in this pane, we provide the code needed to reproduce the action using the SDK - simply select the appropriate language/JavaScript framework for your project, and copy the code into your application.

We also give you access to the Filter Data functionality, which can be used to quickly construct request filters that can be applied to GET requests. Simply click on the "Filter Data" link to expose a UI for constructing your filter, and the resulting parameter code will appear in the "Request Code" textbox on the right side of the screen.

#### Actions tab
The Actions tab allows you to work with custom actions in your object. Custom actions allow you to perform automated tasks in your Backand application.
#### Security tab

#### Settings tab

#### REST API tab

## Queries

### Query List

#### NoSQL

#### SQL

#### Testbed

## Background Jobs

### Accepted job types

### Crontab format

### Job List

## Analytics

### Reports

## Security & Auth

### Configuration

### Social & Keys

### Registered Users

### Team

### Security Actions

### Security Templates

## Hosting

### Configuration

### Hosting files

### Storage files

## Log

### API Requests

### Console

### Server Side Exceptions

### Configuration

### Data History

## Settings

### General

### Database

### Configuration

### Billing Portal

### Upgrade Plan

### Payment Method
Documentation pending
