The "Objects" section of the dashboard allows you to modify the specific objects in your application's database. A menu entry is created for each table in your database, and allows you to configure your application at the object level. For each object in your system, you are given access to the following areas to modify and work with.

## Model

The Model parent tab allows you to modify the JSON schema of your database's model structure, adding new objects and establishing the relationship between them. Below is a brief overview of how to use this interface:

###Example

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

One-to-many relationship between objects are specified using relationship fields. A relationship field will generate the appropriate foreign-key relationship fields in the corresponding relation objects.

Say, for example, that we have a one to many relationship between objects R and S. This means that for each row in R, there are many potentially corresponding rows in S.

In the 'many' side of the relationship (object S), we specify that each row relates to one row in the other object R by providing an object field link:

```json
"myR" : { "object" : "R" }
```

In the 'one' side of the relationship (object R), we specify that each row relates to several rows in S by providing a collection field link, using a 'via' attribute to denote which field on the 'many' side (object S) fulfills the relationship:

```json
"Rs" : { "collection": "S", "via" : "myR" }
```

In the database, a foreign-key constraint will be added between tables S and R (pointing in the direction of R), represented by a foreign key field `myR` being created in the object S's data table. This field will hold the primary key of the corresponding row in R for each row in S.

As an example, consider a model describing pet ownership. It has two objects, `users` and `pets`. Each user can own several pets, but a pet has a single owner (user). Thus, the users-pets relationship is a one to many relationship between users and pets:

The `users` object will have a `pets` relationship field, which establishes the 'many' side of the relationship by creating a collection of pets objects for each user in the model.

In `users`:

```json
"pets": { "collection": "pets", "via": "owner" }
```

The `pets` object will have an `owner` relationship field, which establishes the 'one' side of the relationship by linking each pet back to an individual owner.

In `pets`: 

```json
"owner": { "object": "users" }
```

####Example of one-to-many Model

```json
[
  {
    "name": "pets",
    "fields": {
      "name": {
        "type": "string"
      },
      "owner": {
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
      "pets": {
        "collection": "pets",
        "via": "owner"
      }
    }
  }
]
```

###Many-to-Many Relationship
Many-to-Many relationships between objects are added by adding a new object that has a one-to-many relationship with each object participating in the many-to-many relation. Please review [One-to-many relationship](objects.md#one-to-many-relationship) before continuing with this section.

Say, for example, that we have a many to many relationship between objects R and S. This means that for many rows in R, there are many potentially corresponding rows in S.

As an example, consider a different model describing pet ownership. It has two objects, `users` and `pets`. Each user can own several pets, and each pet can have several owners. Thus, the users-pets relationship is many to many relationship between users and pets:

First we need to add a new object - `users-pets` - with one-to-many relationships to both `pets` and `users` objects.

In `users-pets`:

```json
"pet": { "object": "pets" }
"owner": { "object": "users" }
```

In the corresponding objects `users` and `pets`, we need to add a reference to `users-pets` to complete the one-to-many relationship:

In `pets`:

```json
"owners": {"collection": "users-pets", "via": "pet"}
```
  
In `users`:

```json
"pets": {"collection": "users-pets", "via": "owner"}
```

####Example of many-to-many Model

* Start with a Model that contains 2 objects that have no relations between them:

```json
[
  {
    "name": "pets",
    "fields": {
      "name": {
        "type": "string"
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
      }
    }
  }
]
```
  
* Add the new object and the relationship fields:

```json
[
  {
    "name": "pets",
    "fields": {
      "name": {
        "type": "string"
      },
      "owners": {
        "collection": "users-pets",
        "via": "pet"
      }
    }
  },
  {
    "name": "users-pets",
    "fields": {
      "pet": {
        "object": "pets"
      },
      "owner": {
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
      "pets": {
        "collection": "users-pets",
        "via": "owner"
      }
    }
  }
]
```

## Fields

The Fields tab allows you to modify details about each column in the object. For every column, you can perform the following actions:

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

The pre-defined filter can be used to add additional restrictions to the data loaded, such as only loading data associated with the current username. There are 2 placeholders (tokens) that you can use: username - {{sys::username}} and user role - {{sys::role}}.

For example, the following SQL filter restricts the display of items to only show those items whose corresponding user is the user that is currently logged-in:

```SQL
items.user in (select id from users where email = '{{sys::username}}')
```

Then, we can add an option so that an Admin user can see all of the items added by other application users, in addition to showing the items created by the user currently logged-in:

```SQL
'Admin' = '{{sys::role}}' or (items.user in (select id from users where email = '{{sys::username}}'))
```

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

