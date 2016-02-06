##Introduction
Once your web application's data has been imported and configured, the next step is working with your data. Backand, using your underlying database structure as a key, builds a RESTful API in front of your database, allowing for full programmatic access to each of the HTTP verbs that you would want to use to interact with your server objects. Below we'll cover the concepts behind REST, and how Backand makes creating and testing a RESTful interface simple.

## What is REST?

REST stands for Representational (RE) State (S) Transfer (T). It is a software architectural style that is designed for the creation of (and interaction with) scalable web services. Most RESTful web services – including Backand's implementation – use HTTP verbs such as GET, POST, PUT, and DELETE, to perform specific actions on the server. The end result is a stateless representation of object interaction, providing a clearly-defined interface to an underlying set of objects in an application. This allows for communication with the server to occur as a series of object changes, reducing the complex server logic needed with other, more stateful architectures.

## Automatically Building an API

When you import your database into your application, Backand analyzes your database's underlying schema. It uses the database schema itself, along with metadata associated with each of your database's tables and columns, to build a RESTful API that accurately represents the underlying data structure. Not only does it give you API endpoints for each of the common object actions (such as CREATE, RETRIEVE, UPDATE, and DELETE), but it also uses your database's metadata to fully represent the potential ranges of your application's objects, from representing foreign key relationships all the way down to allowing minimum and maximum values on a specific column in a table. It also keeps track of the nullability of each column, marking non-null fields as required parameters in all related API endpoints. 

By automatically providing these endpoints for your database, Backand provides a scalable and understandable API based on web communication standards that would normally take months to develop, and does so within moments of attaching your database. The endpoints follow all defined security roles, and respond with well-formed JSON that pulls in as much – or as little – of your object as you need.

## The REST API Playground

While a RESTful API is useful, it can also be opaque while the application is being built around it. To get past this nebulous initial state, Backand offers the REST API Playground. The REST API Playground is a dynamic web page built using [Swagger 2.0](http://swagger.io/), an automated front-end for well-defined JSON web APIs. It provides information on each of the endpoints defined in your application, and gives you complete access to test these endpoints in an easy-to-use web-based interface. Through this interface you can see every parameter involved in each API endpoint in your application, and quickly test the endpoints using the supplied parameter prompts and descriptions. This tool can greatly increase development speed, as interested parties can simply use the REST API Playground to build out their JSON web requests prior to incorporating them into the application logic.

## Actions and Queries

Each API endpoint created by Backand also fully integrates with the Action system. You can associate actions with each of the API calls, which simply map to the underlying database actions. POST calls trigger database CREATE actions, PUT calls trigger UPDATE actions, DELETE calls trigger DELETE actions, and so on. Furthermore, the endpoints of each of your custom actions are provided in a helpful “Actions” section, allowing you to test each custom action independently, and with the same useful interface. Finally, endpoints for custom queries that you define in your application are also delineated in the API Playground, allowing for quick iteration on the underlying queries and greatly easing the troubleshooting process.

## Summary

Interacting with the data in your application is something that is crucial to the success of your product. Through Backand's automatic REST scaffolding, you can quickly and easily get up and running with a scalable web service that can provide developers with access to all of the potential modifications they might need to make to your data. With access to both custom actions and queries, as well as the predefined database triggers you create in the dashboard, Backand's RESTful API offers you the full functionality of a scalable web service – all with the click of a button.
