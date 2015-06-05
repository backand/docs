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

