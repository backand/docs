## Retrieving data from Facebook

In order to retrieve data from FB you would need to get the Facebook user id after the user sign-up your Backand app.
 After having the Facebook user id you should use Facebook graph API to retrieve the data or perform any other action.

In order to get the Facebook user id (FBId) to Backand, follow these steps:

1. Open Security & Auth >> Security actions menu and Edit **"beforeSocialSignup"** action
1. Change the **Where Condition** to true (bottom of the page)
1. Uncomment the code that saves the Facebook user id: `userInput.fuid = parameters.socialProfile.additionalValues.id;`
1. Save the Action

Next you need to add the fuid field in the users object:

1. Open the model page under **objects** menu
1. Add new Field in the users object named **fuid**
1. Click **Validate & Update** and **Ok** in the dialog

Now, when a user sign-up to your app using Facebook social provider, you will see the users's fbid in the **users** object.

### Get Facebook data in Backand code

After you have the facebook user id, you should use Graph api to get any data that is available.

1. <a href="https://developers.facebook.com/docs/graph-api" target="_blank">The FB Graph API docs</a>
1. You also would need access token of your app, use this <a href="https://developers.facebook.com/tools/accesstoken/" target="_blank">FB tool</a> to get it.
1. More <a href="https://developers.facebook.com/tools-and-support/" target="_blank">FB tools</a>
1. For example, to get a user's friends use the following JavaScript action code:

```javascript
	var fuid = "176102422846507";
	var accessToken = "EAAWouLyM4lEBADjVlK9yslq9YNjx7S5UUx9oKUo6BWxsj9qc77ZCuKZAPvBQUIulpieNJIJ0Uit3K0UFR0oxjxl68DupTb0uoJFXPQFUdTOlneLEprG6b8WxuYN3AX6m05hKpFbBPKczCab1OUetevdvkZCO6rtPUQEUtc68gZDZD";

    var response = $http({
        method: "GET",
        url:"https://graph.facebook.com/v2.8/" + fuid + "/friends?fields=id,name,gender",
        params:{
            "access_token": accessToken
        },
        headers:{"Content-Type":"application/json"}
    });

    console.log(response);
```

### Get Facebook profile image

1. In order to get the FB profile image you just need to point to the following URL with a correct fbId:

<a href="#">http://graph.facebook.com/{fuid}/picture</a>

or

<a href="http://graph.facebook.com/10209560720107355/picture?type=large" target="_blank">http://graph.facebook
.com/10209560720107355/picture?type=large</a>

1. You can also review FB <a href="https://developers.facebook.com/docs/graph-api/reference/user/picture/" target="_blank">docs</a> on how to use Graph API.
