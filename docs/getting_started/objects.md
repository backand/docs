The "Objects" section of the dashboard allows you to modify the specific objects in your application's database. A menu entry is created for each table in your database, and allows you to configure your application at the object level. For each object in your system, you are given access to the following areas to modify and work with.

## Model

The Model parent tab allows you to modify the JSON schema of your database's model structure, adding new objects and establishing the relationship between them. Below is a brief overview of how to use this interface:

###Example

```json
[
  {
    "name": "tasks",
    "fields": {
      "description": {
        "type": "string"
      },
      "completed": {
        "type": "boolean"
      },
      "members": {
        "collection": "members",
        "via": "task"
      }
    }
  },
  {
    "name": "members",
    "fields": {
      "name": {
        "type": "string"
      },
      "task": {
        "object": "tasks"
      }
    }
  }
]
```

###Clarifications

* In addition to the fields supplied by the user, Backand defines an 'id' field, of type integer, which will be used as a primary key for the table.
* Renaming fields can only be done from the Fields tab in the application dashboard. Renaming a field or an object in a model will delete the data associated with that field or object.

###Definitions

The model represent a database schema that is defined as a JSON array of one or more object (Table) definitions:

```json
<model> = [  <object>, <object>, ... ]
```

An object definition is a JSON object with a name entry and a fields entry:

```json
<object> = { "name":  <string>, "fields" : <fields> }
```

The fields definition is a JSON list of field attributes:

```json
<fields> =  { "field1" : <field>, "field2": <field>, ... }
```

A field is defined by its type and a set of optional properties. The field definition is a JSON object:

```
<field> = { "type": <type>, <optional properties>}
```
A field may have one of the following types:

* **string** - string column with length up to 255 characters
* **text** - text column with length up to 21,844 characters
* **float**
* **datetime**
* **boolean**

###One-to-Many Relationship

One-to-many relationship between tables are specified using relationship fields. A relationship field will generate the appropriate foreign-key relationship fields in the corresponding relation objects.

Say, for example, that we have a one to many relationship between tables R and S. This means that for each row in R, there are many potentially corresponding rows in S.

In the 'many' side of the relationship (object S), we specify that each row relates to one row in the other object R by providing an object field link:

```json
"myR" : { "object" : "R" }
```

In the 'one' side of the relationship (object R), we specify that each row relates to several rows in S by providing a collection field link, using a 'via' attribute to denote which field on the 'many' side (object S) fulfills the relationship:

```json
"Rs" : { "collection": "S", "via" : "myR" }
```

In the database, a foreign-key constraint will be added between objects S to R, represented by a foreign key field `myR` being created on the object S's data table. This field will hold the primary key of the corresponding row in R for each row in S.

As an example, consider a database describing pet ownership. It has two tables, `person` and `pet`. Each person can own several pets, but a pet has a single owner. Thus, the person-pet relationship is a one to many relationship between person and pet:

The `person` object will have a `pets` a relationship field, which establishes the 'one' side of the relationship by creating a collection of pet objects for each person in the database:

```json
"pets": { "collection": "pet", "via": "owner" }
```

The `pet` table will have an `owner` a relationship field, which establishes the 'many' side of the relationship by linking each pet back to an individual owner instance of type 'person':

```json
"owner": { "object": "person" }
```

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

The Settings tab allows you to modify high-level aspects of the object, such as the object name. It also allows you to enable change tracking for this object, and set the column that contains the object's description. This is used when setting the object as a relatedTable relation to another object, and will drive the contents of each drop-down and multi select provided as a result.

## Data

The Data tab contains an editable data grid representing the current list of objects stored in the database. You can create, modify, or delete the records as you see fit.

## REST API

The REST API tab gives you a listing of the basic RESTful API endpoints available for this object. You can test each endpoint, and see the URLs used in action.

