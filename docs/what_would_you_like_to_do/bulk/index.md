## Bulk Operations

Backand's bulk operations endpoint can be used to operate on multiple items in a single request. The request has the following general form:

```
  [
    {
      method:"string", 
      url:"string", 
      data:"string", 
      parameters:"string", 
      headers: {
        "string":"string"
      }
    }
  ]
```

The above represents an array of parameter hashes. Each parameter hash represents a different action to be performed as a part of the bulk operaation. The parameter hashes are broken down as follows:

| Field name | Possible Values | Required? | Description |
| ----- | ----------- |------|-----|
| method | POST/ PUT/ DELETE | yes | The HTTP verb to be used |
| url | url for the object | yes | This is the URL used to access the object being modified in bulk |
| data| JSON hash | yes | This only applies to POST and PUT actions. It represents the data to be created (for POST requests), or the updates to existing data (for PUT requests). Provided as a series of key-value pairs |
| parameters | JSON string | no | additional parameters, and values for those parameters, to send with the request |
| headers | JSON hash |no| This is used to set custom HTTP headers. You can use it to dynamically set the Authorization header for each REST call. If no value is provided, the generic bulk call authorization headers are used. |

The key element of this operation is the "headers" parameter. This represents the HTTP headers used to perform the request, and are used to uniquely identify your application as each request as made. The headers parameter should have the following form:

```
  Headers:{"Authorization": Bearer **YOUR_ACCESS_TOKEN**, "AppName": **YOUR_APP_NAME**}
```

The value for "YOUR_ACCESS_TOKEN" is obtained via a call to 'https://api.backand.com/token' - simply replace "YOUR_ACCESS_TOKEN" with the bearer token returned by this endpoint, and "YOUR_APP_NAME" with your application's name that was specified during app creation. Each action hash can accept a different header parameter, allowing multiple operations to be undertaken by multiple users as a part of the same bulk request. If the 'headers' parameter is not included, the headers used for the call to the bulk operations endpoint are used for the relevant action instead.

Requests to perform bulk actions are sent as HTTP POST requests to the bulk operations URL:

```
  https://api.backand.com/1/bulk
```

Below are several examples of the types of bulk actions that can be performed using this new functionality.

### Example 1: Adding multiple rows

To add multiple objects to your application, simply provide multiple POST action hashes in your request's data array. The general form of the request is:

```
[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME",
    "data": {
      "fieldname": "value"
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME",
    "data": {
      "fieldname": "value"
    }
  }
]
```
Simply change the 'url' parameter of each action hash to match your object's endpoint in your application. Let's look at a specific example that adds multiple 'news' objects to an application:

```
[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Breaking news",
      "author": 1,
      "complete": false
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Politics",
      "author": 2,
      "complete": true
    }
  }
]

```

As you can see above, this bulk operation will add two new objects of type 'news' to your application. The first will have the heading of "Breaking News", while the second will have the heading of "Politics".

### Example 2: Deleting multiple rows

To delete multiple objects from your application, simply provide multiple DELETE action hashes in your request's data array. The general form of the request will be:

```
[
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME/OBJECT_ID1"
  },
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/YOUR_OBJECT_NAME/OBJECT_ID2"
  }
]

```

By changing the YOUR_OBJECT_NAME and OBJECT_ID# parameters, you can select which specific IDs will be deleted in your application. For example, if you wanted to delete 'news' objects with IDs of 2 and 3, you would use the following set of action hashes:

```
[
    {
        "method":"DELETE",
        "url":"https://api.backand.com/1/objects/news/2"
    },
    {
        "method":"DELETE",
        "url":"https://api.backand.com/1/objects/news/3"
    }
]

```

### Example 3: Multiple commands

You can mix and match operations to be performed to match whatever actions you need to take. For example, the following set of action hashes will create a new 'news' object, create a new 'authors' object, update an existing 'authors' object with an ID of 1, and delete a 'news' object with an ID of 3:


```

[
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/news",
    "data": {
      "heading": "Breaking news",
      "author": 1,
      "complete": false
    }
  },
  {
    "method": "POST",
    "url": "https://api.backand.com/1/objects/authors",
    "data": {
      "email": "ann@backand.com",
      "firstname": "ann"
    }
  },
  {
    "method": "PUT",
    "url": "https://api.backand.com/1/objects/authors/1",
    "data": {
      "firstname": "Brian"
    }
  },
  {
    "method": "DELETE",
    "url": "https://api.backand.com/1/objects/news/3"
  }
]

```

