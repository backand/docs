# Read Data
# Deep vs. Lazy Load
We will discuss two options to read complex object. Deep and Lazy Load. But first, what is a complex object? A complex object is an object with relations to other object. For example, in the default model that you get when you start a new Backand app there are two objects: users and items. There is a one-to-many relation between users and items which means that a specific user may have many items were each of those items relate into one specific user. In this example both users and items are complex objects. We are more interesting in the users object since the relation to items is described as a collection field and reads into json as an array of items objects.
```
{
  "email":"....",
  // other users fields...
  "items":[{"title":"....",...},...]
}
```
In most cases, most objects are complex objects. Backand supports two approaches to read them: Deep and Lazy Load. To perform a deep read for a specific user, simply add deep=true to the query string.
```
https://api.backand.com/1/objects/users/5?deep=true
```
This will result with the above json.  
It takes two requests to perform a Lazy Load. The first request is a shallow request for users or a specific user.
```
https://api.backand.com/1/objects/users
or 
https://api.backand.com/1/objects/users/5
```
The second request is for all the items of a specific user.
```
https://api.backand.com/1/objects/users/5/items
```
This request is similar to get the entire list of items
```
https://api.backand.com/1/objects/items
```
You can use the other query string parameters such as paging filtering and sorting. see [List Collection Objects from a Collection Field for a Specific Object ID](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-collection-objects-from-a-collection-field-for-a-specific-object-id)
