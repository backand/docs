## Deep vs. Lazy Load
A Complex object is an object that has relationships to other objects in the database. For example, the default model provided when you create a new Backand application has two objects: users and items. There is a one-to-many relationship between users and items, meaning that a specific user may have many different related items. In this example, both users and items are complex objects. The relationship between the two tables, represented as the User object having a collection of Item objects, can be retrieved from the database by loading them through the object's API.

There are two ways in which you can load complex objects: Deep Loading, or Lazy Loading. Deep loading pulls back both the object and all of its related objects. In the Users/Items example, this would pull back both the user, and all items related to that user. Lazy loading, by comparison, only pulls back the parent object, allowing you to more closely control when you pull in the rest of the data. This is important when the number of related objects is large, meaning a significant load time would be necessary.

An example of another complex object would be 'email':


Seeing as most objects in a Backand application tend to  be complex objects, we added an easy way to enable deep 
loading of objects. Simply append '?deep=true' to the object fetch URL, like so:

```
https://api.backand.com/1/objects/users/5?deep=true
```

This will result in the following JSON:

```
{
  "email":"....",
  // other users fields...
  "items":[{"title":"....",...},...]
}
```

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

This request is similar to fetching the entire list of items, which would be done as follows:

```
https://api.backand.com/1/objects/items
```

You can also supply other query string parameters (such as paging, filtering, and sorting). See [List Collection Objects from a Collection Field for a Specific Object ID](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-collection-objects-from-a-collection-field-for-a-specific-object-id) for more details.

Regarding which approach to use - lazy or deep loading - there is no clear answer. It will be dependent upon the individual programming situation. If you need to present all of the relevant data at once, then deep loading is the right approach. However, if the object has a large number of relations to pull down, it might make more sense to do a lazy load, then selectively load related objects as needed.

## Filter and Predefined Filter

You can supply a query string parameter named 'filter' to filter your objects during fetch. This could be useful in 
developm situations, or as a part of a UI with search functionality, allowing your users to see whatever they wish. You can either create a filter with NoSQL syntax (see [NoSQL Query Language](../apidocs/nosql_query_language/NoSQL_Query_Language) for syntax), or with JSON containing a field name, an operator, and a value:

```
[{"fieldName":"firstName","operator":"contains","value":"oh"}]
```

See [getting a list of objects](http://docs.backand.com/en/latest/apidocs/apidescription/index.html#list-of-objects) for full details on filters.

### Predefined Filter

Sometimes, for security reasons you may want to enforce partial reading of object data, usually based upon the 
current user or their role. This can be accomplished on the server-side using a Predefined Filter. Predefined Filters
 can be found in the Security tab of each object. They act as a SQL 'where' condition. Filters are combined using the
  'AND' operator. You can write the filter using either a SQL statement or the NoSQL Query Language reading of 
  partial data, usually depending on the current user or current user role. The option to force it from the server side is called Predefined Filter and you can find it at the Security tab of each object. The predefined filter acts as a SQL where statement condition. If you add additional filter that were described above then they will be added with an "AND" logic. You can write the predefined filter either as a SQL statement or as a [NoSQL Query Language](../apidocs/nosql_query_language/NoSQL_Query_Language) statement.

## Queries and On Demand Actions

So far we have discussed how to read objects that were defined in the database model. To create custom data structures, we can use Queries and On Demand Actions. Queries are identical to SQL database queries but you can write them using NoSQL syntax (see [NoSQL Query Language](../apidocs/nosql_query_language/NoSQL_Query_Language) for more details). With On Demand Actions, you can orchestrate any structure that you want using javascript on the server side. You can request model objects, queries, or other on demand actions. See [On Demand Actions Javascript Code](http://docs.backand.com/en/latest/apidocs/customactions/index.html#server-side-javascript-code) for more information.

#### Pagination

The LIMIT clause can be used to constrain the number of rows returned by the SELECT statement. LIMIT takes one or two numeric arguments (number of records and offset), which must both be nonnegative integer constants. We can compute the constants either on [JavaScript action](../../apidocs/customactions/index.html#server-side-javascript-code) or by using a prepared statement:

```
            SET @records := 20;
            PREPARE stmt FROM
            " SELECT    *
              FROM      Users
              LIMIT     ?, ?";
            -- {{ "{{pageNum" }}}} is the client side parameter representing the requested page number
            SET @offset := @records * ({{ "{{pageNum" }}}} -1 );
            EXECUTE stmt USING @offset,@records;
```
