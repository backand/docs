# Expanding Available Functionality

Amazon Web Services, or AWS, offer a lot of useful functionality for web developers. From hosting, to distributed computing, to even simple data storage, Amazon has backed many of the web's largest websites since its inception. Below we'll look at Amazon S3, and how we can integrate the data storage service into a Backand-powered application.

# A Quick S3 Overview

Amazon's Simple Storage Service, or S3, is an online file storage web service offered as a part of the Amazon Web Services suite of functionality. It was first launched in the US in 2006, and uses standard web service interfaces for communication. Service consumers can simply authorize their app with S3, then perform a series of web service calls to manage their S3 storage bucket. By using this service, a developer can offload concerns about data availability, reliability, and storage space to Amazon's server farms, simply paying a usage-based fee each month for the data stored.

# Server-side Action

[Backand's Noterious app](https://github.com/backand/simple-noterious-app) has an excellent example of how simple an integration with S3 can be. The functionality revolves around the creation of server-side actions to wrap the web service requests made over HTTP. The actual integration looks like this:

```javascript
var data = 
    {
        // enter your aws key
        "key" : "AKIAIJX26SY3COIWV4FQ", 

        // enter your aws secret key
        "secret" : "ogun3pSOyw8FtP5i17s2STBPO9jQ8cs+lnpxwg82", 

        // this should be sent in post body
        "filename" : parameters.filename, 
        "filedata" : parameters.filedata,         

        "region" : "US Standard",
        "bucket" : "backand-free-upload"

    }
var response = $http({method:"PUT",url:CONSTS.apiUrl + "/1/file/s3" , 
               data: data, headers: {"Authorization":userProfile.token}});

return response;
```

The above code is inserted into a new Custom Server-Side JavaScript Action created using the Backand dashboard. It pulls the file information from the parameters passed to the action, then sends a request to S3's file upload endpoint, putting the file into the bucket 'backand-free-upload'. By customizing the above code, you can achieve any underlying file structure you desire.

# Client-side Integration

Of course the server-side code is only half of the story – we need to integrate this wrapped API call into our application for it to be useful. Going back to the Noterious demo application, you can see an example of this taking place in the [Notes model class](https://github.com/backand/simple-noterious-app/blob/6da40ba8b235c35d77f1845a643e80a352d1dc20/src/app/common/models/notes-model.js#L44):

```javascript
//Send file content in base64 to Backand on-demand action
self.s3FileUpload = function(filename, filedata, success, error)
{
  return $http ({
    method: 'POST',
    url: Backand.getApiUrl() + '/1/objects/action/notes?name=upload2s3',
    headers: {
      'Content-Type': 'application/json'
    },
    data:
    {
      "filename":filename,
      "filedata": filedata.substr(filedata.indexOf(',')+1, filedata.length) //need to remove the file prefix type
    }
  });
};
```

The above code is pretty straightforward. It defines a function – s3FileUpload – that takes input from your application in the form of file data, and then communicates with your new custom JavaScript action via an HTTP POST request using JavaScript's built-in AJAX capabilities. Simply call this function from your Angular code, and you will be uploading files in no time!