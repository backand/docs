[custom actions](../customactions/customactions.md) can be called by sending a GET request to `/objects/action/{objectName}/{id}` The custom action is associated with an object list in order to simplify security configuration. It can also be associated with a specific object id, which is used as an input to the action, but this is optional. You can also define additional parameters in the request, which are passed through to the function. The action returns a custom JSON response.

* **actionName** - The custom action name.
* **parameters** - A JSON object with all predefined parameters for the action.

```
self.callAction = function (objectName, id, actionName, parameters) {
      return $http({
          method: 'GET',
          url: Backand.configuration.apiUrl + '/1/objects/action/' + objectName + '/' + id
          params: {
             parameters: parameters
          }
      });
  };
```
**Note**: You can find a configuration and testing environment for each of the [custom actions](../customactions/customactions.md) in the right side menu for your application on an object's Actions tab. 

