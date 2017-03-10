## Retrieving data from Facebook using the Graph API

Once you've integrated with facebook, you can use Facebook's [Graph API](https://developers.facebook.com/docs/graph-api) to augment your user data with information directly from the user's Facebook account. To do so, you'll just need to grab the user's Facebook User ID, and use the Graph API to retrieve data, update or publish new content, or any of the other functionality Facebook has to offer.

### Obtaining the Facebook User ID
To store the user's Facebook user ID (FUID) in Backand, follow these steps:

1. Open Security & Auth >> Security actions menu and edit the **"beforeSocialSignup"** action
1. Change the **Where Condition** of this action to "true" (this is set at the bottom of the content pane)
1. Uncomment the provided code that saves the Facebook user id: `userInput.fuid = parameters.socialProfile.additionalValues.id;`
1. Save the Action

Next, you need to add the FUID field to the "users" object:

1. Open the model page, from the **Objects** menu
1. Edit the model to add a new field - **fuid**, type string - to the "users" object
1. Click **Validate & Update** to validate the model, then **Ok** in the confirmation dialog to save your changes.

From this point onward, any user that signs up to your app using Facebook as their social media provider will have their FUID automatically populated in the **users** object.

### Getting Facebook data into Backand code
Once you have the Facebook user ID, you can use the Graph API to access any available Facebook data.

* See the Facebook Graph API docs at [https://developers.facebook.com/docs/graph-api](https://developers.facebook.com/docs/graph-api)
* You'll also need a valid Facebook Access Token for your application. Use [Facebook's access token tool](https://developers.facebook.com/tools/accesstoken/) to obtain one.
* Facebook offers [additional tools](https://developers.facebook.com/tools-and-support/)

At this point, you're ready to work with the API. For example, you can use this sample code to get a list of a user's friends in Facebook:

```javascript
	var fuid = "176102422846507";
	var facebookAccessToken = "EAAWouLyM4lEBADjVlK9yslq9YNjx7S5UUx9oKUo6BWxsj9qc77ZCuKZAPvBQUIulpieNJIJ0Uit3K0UFR0oxjxl68DupTb0uoJFXPQFUdTOlneLEprG6b8WxuYN3AX6m05hKpFbBPKczCab1OUetevdvkZCO6rtPUQEUtc68gZDZD";

    var response = $http({
        method: "GET",
        url:"https://graph.facebook.com/v2.8/" + fuid + "/friends?fields=id,name,gender",
        params:{
            "access_token": facebookAccessToken
        },
        headers:{"Content-Type":"application/json"}
    });

    console.log(response);
```

### Getting a Facebook profile image

You can easily obtain a user's Facebook profile using a single URL reference. Here is the template:

```html
<a href="#">http://graph.facebook.com/{fuid}/picture</a>
```

Simply replace **{fuid}** with the user's Facebook User ID.

You can pull in a larger version of the profile picutre using the following HTML:

```html
<a href="http://graph.facebook.com/{fuid}/picture?type=large" target="_blank">http://graph.facebook
.com/{fuid}/picture?type=large</a>
```

Finally, you can refer to [Facebook's Docs](https://developers.facebook.com/docs/graph-api/reference/user/picture/) on their Graph API, and use the information to create whatever type of integration with Facebook that you desire.
