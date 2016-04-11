## Bulk

To operate on multiple items use backand bulk endpoint.

The request has the following form

```[{method:"string", url:"string", data:"string", parameters:"string", headers:{"string":"string"}}]```

| Field name |Values ||
| ----- | ----------- |------|
| method | POST/ PUT/ DELETE |required|
| url | The object url |required|
|data| Applies to POST and PUT. The object data you want to create or update.|required
|parameters| Additional parameters you want to add to the REST call|not required
|headers| You can set different Authorization to each REST call. If left blank uses the bulk call authorization|not required

The bulk call headers should contain the following items
```
Headers:{"Authorization": Bearer YOUR_ACCESS_TOKEN, "AppName": YOUR_APP_NAME}
```

The Bulk call uses the POST verb


### Adding multiple rows:

 To add multiple rows in one REST call
```url: https://api.backand.com/1/bulk```

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
e.g.
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

 ### Delete multiple rows:

 To delete multiple rows use the following REST configuration.
```url: https://api.backand.com/1/bulk```

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
e.g.
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

## Multiple commands

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

