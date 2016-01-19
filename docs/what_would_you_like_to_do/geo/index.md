## Introduction

Backand lets you store geography points as fields as part of any object.
geography point is an array of latitude and longitude which represents a location on a map.

With Backand geography query you can easily find distance between points or objects that match a distance to a 
specific location.

## Add a Point

To add new point field, open Backand Model, select add field with *point* type.
To POST new data point for latitude 10 and longitude 20 just send the following array:

```JSON
  "geo": [10,20]
```

## Query Point

To retrieve all cities within 25km you need to provide it in meters (25000 meters) from a given [latitude, longitude], 
e.g. [37.8019859, -122.4414805]. To find the distance in Miles you need to multiple by 1.609, e.g. 25 miles => 40.222
 KM ==> 4022.5 Meters

Getting all the restaurants within 25 Miles of San Francisco Marina District [37.8019859, -122.4414805]: 

```json
{ 
  "object": "restaurants", 
  "q": {
    "location" : { "$within" : [[37.8019859, -122.4414805], 4022.5] } 
  } 
}
```

The above noSql is translated into MySQL syntax using the new ST_Distance() function:
 
```SQL

SELECT * FROM `restaurants` WHERE (ST_Distance ( `restaurants`.`location`, ST_GeomFromText('POINT( 37.8019859 -122.4414805 )') ) <= 0.03622413943151704)
   
```

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
        "location" : { "$within" : [[37.8019859, -122.4414805], 4022.5]} 
      } 
    }
  }
});

```