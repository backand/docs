## Introduction

Backand lets you store geography points as fields as part of any object.
geography point is an array of latitude and longitude which represents a location on a map.

With Backand geography query you can easily find distance between points or objects that match a distance to a 
specific location.

## Add a Point

To add new point field, open Backand Model, select add field with *point* type.
To POST new data point for latitude 10 and longitude 20 just send the following array:

```json
  "geo": [10,20]
```

## Query Point

To query a distance within a geography point you can use any of the follow commands:
`$withinMiles`
`$withinFeet`
`$withinKilometers`
`$within (in meters)`

To get all the restaurants within 25 Miles of San Francisco Marina District [37.8019859, -122.4414805]: 

```json
  { 
    "object": "restaurants", 
    "q": {
      "location" : { "$withinMiles" : [[37.8019859, -122.4414805], 25] } 
    } 
  }
```

The above noSql is translated into MySQL syntax using the new ST_Distance() function:
 
```SQL

  SELECT * FROM `restaurants` WHERE (ST_Distance ( `restaurants`.`location`, ST_GeomFromText('POINT( 37.8019859 -122.4414805 )') ) <= 25 /(69))
   
```

## Query Point with Parameters

In order to query get geography points dynamically, use [Query](http://docs.backand
.com/en/latest/getting_started/queries/index.html) with parameters:

1. Add this *Input Parameters*: lan, lon, dist
2. In the Query use tokens ("&#123;&#123;lan}}", "&#123;&#123;lon}}" and "&#123;&#123;dist}}") to represent the input parameters:
3. 

  { 
    "object": "restaurants", 
    "q": {
      "location" : { "$withinMiles" : [["&#123;&#123;lan}}", "&#123;&#123;lon}}"], "&#123;&#123;dist}}"] } 
    } 
  }


## Filter Point

Using Backand noSql filter you can filter object based on a distance to a specific point using $within command.

Filter all the restaurants within 25 Miles of San Francisco Marina District [37.8019859, -122.4414805]: 

```javascript

  return $http ({
    method: 'GET',
    url: Backand.getApiUrl() + '/1/objects/restaurants',
    params: {
      filter: {
        "q": { 
          "location" : { "$withinMiles" : [[37.8019859, -122.4414805], 25]} 
        } 
      }
    }
  });

```
