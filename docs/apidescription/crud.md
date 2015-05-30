Once authentication is completed, you can perform all relevant [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations in your application:


####List of Objects


Call `/objects/{name}` with the following parameters to get a list of items:

* **pageSize** - The number of returned items in each getList call (default 20).
* **pageNumber** - The page number starting with 1 (1-based, default 1).
* **filter** - An array of JSON objects where each item has the properties fieldName, operator and value. The operator options depend on the field type.
* **sort** - An array of JSON objects where each item has the properties fieldName and order. The order options are "asc" or "desc".
* **search** - Free text filter search.
* **deep** - When set to true, brings the related parent items in the relatedTables property.

```
  self.getList = function (name, pageSize, pageNumber, filter, sort) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/objects/' + name,
          params: {
            pageSize: pageSize,
            pageNumber: pageNumber,
            sort: sort,
            filter: filter
          }
      });
  };
```
####One Object


Call `/objects/{name}/{id}` with a specific item id and with the following parameters to get a specific item:

* **id** - The item's id, which is the primary key value for the item's database table
* **deep** - When set to true, brings the related parent items in the relatedTables property

```
  self.getOne = function (name, id, deep) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id
          params: {
            deep: deep
          }
      });
  };
```

####Create


POST to `/objects/{name}` with a new object to create (stored in the data portion of the AJAX call) using the following parameters:

* **returnObject** - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running

```
  self.create = function (name, data, returnObject) {
      return $http({
          method: 'POST',
          url : Backand.configuration.apiUrl + '/1/objects/' + name,
          data: data,
          params: {
            returnObject: returnObject
          }
      })
  };
```

####Update


PUT to `/objects/{name}/{id}` to update an existing object (with changes provided in the data portion of the AJAX call). You must also supply the following parameters:

* **id** - The item's id, which is the primary key value for the item's database table 
* **returnObject** - Set this to true when you have server side business rules that causes additional changes to the object. This will force the server to return the object after all business rules have taken effect. If false, it will return the object prior to any business rules running.

```
  self.update = function (name, id, data, returnObject) {
      return $http({
          method: 'PUT',
          url : Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id,
          data: data,
          params: {
            returnObject: returnObject
          }
      })
  };
```

####Delete


DELETE to `/objects/{name}/{id}` to remove an item:

* **id** - The item's id, which is the primary key value for the item's database table.

```
  self.delete = function (name, id) {
      return $http({
          method: 'DELETE',
          url : Backand.configuration.apiUrl + '/1/objects/' + name + '/' + id
      })
  };
```