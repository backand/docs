## Retrieving data from Facebook

First you would need to get the user facebook id after the user signup and then start using Facebook graph API to
retrieve the data.

In order to get the Facebook user id (FBId) to Backand, follow these steps:
1. Open Security & Auth >> Security actions menu and Edit **"beforeSocialSignup"** action
1. Change the **Where Condition** to true (bottom of the page)
1. uncomment the code that saves the Facebook user id: 'userInput.fuid = parameters.socialProfile.additionalValues.id;'
1. Save the Action

Next you need to add the fuid field in the users object:
1. Open the model page under **objects** menu
1. Add new Field in the users object named **fuid**
1. Click **Validate & Update and **Ok** in the dialog

Now when you will signup new user using Facebook social provider, you will see the user in the users object and fbid.

### Get Facebook data in Backand code

After you have the facebook user id, you should use graph api to get any data that is available.

1. <a href="https://developers.facebook.com/docs/graph-api" target="_blank">The FB Graph API docs</a>
1. You also would need access token of you app, use this <a href="https://developers.facebook.com/tools/accesstoken/" target="_blank">FB tool</a> to get it.
1. More <a href="https://developers.facebook.com/tools-and-support/" target="_blank">FB tools</a>
1. To get user friends you would need to use JavaScript action with the following code:

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
<a href="#">http://graph.facebook.com/{{fuid}}/picture</a>
or
<a href="http://graph.facebook.com/10209560720107355/picture?type=large" target="_blank">http://graph.facebook
.com/10209560720107355/picture?type=large</a>

1. You can also review FB <a href="https://developers.facebook.com/docs/graph-api/reference/user/picture/" target="_blank">docs</a> on how to use code.
