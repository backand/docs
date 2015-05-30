You can run custom pre-configured queries by sending a GET request to `/query/data/{queryName}` The query syntax must follow the SQL rules of the underlying database that your application is using. You can include predefined parameters in your request that are then used by your query.

* **parameters** - A JSON object with all predefined query parameters provided.

```
self.query = function (queryName, parameters) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/query/data/' + queryName
          params: {
             parameters: parameters
          }
      });
  };
```

**Note**: You can find a configuration and testing environment for custom queries in the Application dashboard's right side menu, under "Queries". 
