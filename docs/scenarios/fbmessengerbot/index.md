## Creating your own Facebook Messenger Bot with Backand

<img align="right" src="https://www.backand.com/wp-content/uploads/2016/09/bot-iphone.png">


Facebook recently opened up their Messenger platform to enable bots to converse with users through Facebook Apps and on Facebook Pages. In support of this, the Facebook Messenger team created a comprehensive set of [documentation](https://developers.facebook.com/docs/messenger-platform/quickstart) describing the functionality on offer. While they offer a lot of functionality and platform-level integration for free, you still need to create a publicly-accessible application that Facebook can communicate with in order to automatically interact with your end users. 

In this tutorial, we'll walk through creating your own messenger bot on the Backand backend-as-a-service system, and make it live - all in 10 minutes.

## Demo
You can chat with the simple bot example to see the end result at <a hreh="http://m.me/1150283998341709" target="_blank">http://m.me/1150283998341709</a>

## Getting Started

Messenger bots are programs that receive messages from an interface, process those messages, and then send results back to the caller. These results are then displayed to the user by the controlling application - Facebook Messenger in this case. A bot's response can be as simple as echoing text back to the user, or as complex as ordering and shipping new computer components to the end user. The only restrictions are that the bot is authenticated to communicate with Facebook, and that Facebook has approved the bot and allows it to communicate with the world at large. 

Luckily, Backand handles the majority of the server and connectivity functionality for you, allowing you to focus on building out responsive content.

### *Building the server*

Follow these steps to build out the back-end to your bot:

1. Sign up for free at [backand.com](https://www.backand.com) (if you don't already have an account).

2. Create a new app in the Backand dashboard, then navigate to that app's management page.

3. In the new app, open menu 'Objects --> Items', and click on the 'Actions' tab. In the Actions tab, click on the 'Facebook Messenger Bot' template.

4. Click 'Save'

At this point you have a back-end server that is ready to integrate with Facebook Messenger. However, you need to make a few more configuration changes before you're ready to test.


### *Create a Facebook App and Page*

Once you have the back-end and are ready to integrate, the first thing you need to do is create a Facebook Page at [https://www.facebook.com/pages/create/?ref_type=pages_browser](https://www.facebook.com/pages/create/?ref_type=pages_browser). This page will serve as the central integration point between your Backand application and Facebook, as the messenger bot is authorized to interact with Facebook on behalf of this page.

#### Facebook App Creation

Once you have the app's page up and running, you need to configure the Facebook integration. To do so:

* Add a new app at [https://developers.facebook.com/quickstarts/?platform=web](https://developers.facebook.com/quickstarts/?platform=web). Name it and click "Create New Facebook App ID":

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-create-new-app.png)

* Add an email address, select your app's category, and add a web site for your app (any URL is OK):

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-create-new-app-info.png)

* Next, skip the quick start and go to the App Dashboard and click 'Add Product' under the heading 'Product Settings'. Once there, select 'Messenger' from the available options:

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-add-new-product-1.png)

#### *Setup Webhooks*

Once the app page is created, and the app is registered, you need to tell Facebook where to send its messages for processing. You can do this with the following steps:

* In the 'Webhooks' section, click 'Setup Webhooks'.

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-set-webhook1.png)

* Enter the URL for the webhook - this should link back to your Backand application, and will resemble the below: 

```
https://api.backand.com/1/objects/action/items?
name=FBMessengerBot&Authorization=basic+<master token>:<user key>
```

_Note:_ The webhook URL uses Backand's [basic authentication](http://docs.backand.com/en/latest/apidocs/security/index.html#basic-authentication).
You can find the '&lt;master token&gt;' in the 'Security & Auth --> Social & Keys' section.
It also requires a '&lt;user key&gt;' for your app - you can find this in the 'Security & Auth --> Team' section. Simply click on the key icon near one of the Admins to obtain the user key.

* Verify Token: my_test_token

* Select *message_deliveries*, *messages*, *messaging_optins*, and *messaging_postbacks* under Subscription Fields.

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-setup-webhook.png)

#### *Get a Page Access Token*
Once you've configured the app in Facebook, it's time to tie it back into your Backand application. In the Token Generation section, select your Page. A 'Page Access Token' will be generated for you. Copy this 'Page Access Token', and navigate back to your Backand app. Open the 'Action' section of your 'Items' object, and paste the token into the Backand Action where indicated (PAGE_ACCESS_TOKEN).

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-select-page.png)

#### *Subscribe the App to the Page*
Finally, you need to subscribe to the webhooks available for your page. This is managed in the 'Webhooks' section of the Facebook configuration:

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-page-subscribe.png)

### *Run the Bot*

At this point, you're ready to test your bot! Navigate to your Facebook Page and send it a message using Facebook Messenger. You should see the same message echoed back to you with a prefix of "Back& bot says:". This is also sent to the javascript console for immediate analysis.

![image](https://www.backand.com/wp-content/uploads/2016/09/bot-facebook-page.png)

**At this point your basic bot is working. From here, you can customize the bot's server side to build out functionality for your users**

## How to share your bot

A bot is only useful if people choose to interact with it. Below are some methods you can use to advertise your bot and draw in users.

#### *Add a chat button to your webpage*

As a part of their initial documentation, Facebook provided an easy method to add a Chat button to your website. The documentation on how to do this is available [here](https://developers.facebook.com/docs/messenger-platform/plugin-reference).

#### *Create a shortlink*

You can also implement a shortlink that can be used to initiate a chat with your app. Simply use a URL of the form 'https://m.me/&lt;PAGE_USERNAME&gt;' to begin a Messenger chat.

### What's next?

At this point, you have a chatbot interacting with Facebook Messenger, driven by Backand as a back-end. You can customize the bot's code to meet your needs by following the information available in the following section of *Customize the Bot's Code in a Backand Action*.

You can also review Facebook's [complete guide on Messenger integration](https://developers.facebook.com/docs/messenger-platform/product-overview/setup), which provides more comprehensive detail on the platform's capabilities. These can be used to enhance your message responses, adding web-enabled content that your users can follow.

Eventually, you'll want to get your bot approved for use by the public. You can find out how to do that [here](https://developers.facebook.com/docs/messenger-platform/app-review).

Finally, you can enhance your app's intelligence with an AI integration. Find out how to get started at [Wit.ai](https://wit.ai)!

## Customizing the Bot's Code in a Backand Action

### *Receive Messages*

All callbacks and webhooks from Facebook will end up in your object's Facebook Action code. The JavaScript for this action is solely responsible for listening to the incoming POST calls, and responding appropriately. The template code handles all webhooks from Facebook by default. For example, receiving messages is handled by looking for the 'messagingEvent.message' field in the webhook, and then calling the 'receivedMessage()' function as follows:

```
if (request.method == "POST"){

    var data = request.body;
    if (data.object == 'page') {

        // Iterate over each entry
        // There may be multiple if batched
        data.entry.forEach(function(pageEntry) {

            var pageID = pageEntry.id;
            var timeOfEvent = pageEntry.time;

            // Iterate over each messaging event
            pageEntry.messaging.forEach(function(messagingEvent) {
                if (messagingEvent.optin) {
                    //receivedAuthentication(messagingEvent);
                } else if (messagingEvent.message) {
                    receivedMessage(messagingEvent);
                } else if (messagingEvent.delivery) {
                    //receivedDeliveryConfirmation(messagingEvent);
                } else if (messagingEvent.postback) {
                    receivedPostback(messagingEvent);
                } else {
                    console.log("Webhook received unknown messagingEvent: ", messagingEvent);
                }
            });
        });
    }
}
```

### *Send a Text Message*

In receivedMessage, we've added logic that can send a message back to the user. The default behavior is to echo back the text that was received, with some static modifications ('Back& bot says'...):

```
function receivedMessage(event) {

    var senderID = event.sender.id;
    var recipientID = event.recipient.id;
    var timeOfMessage = event.timestamp;
    var message = event.message;

    console.log("Received message for user" + senderID + " and page " + recipientID + " at " + timeOfMessage + " with message");
    console.log(JSON.stringify(message));

    var messageId = message.mid;

    // You may get a text or attachment but not both
    var messageText = message.text;
    var messageAttachments = message.attachments;

    if (messageText) {

    // If we receive a text message, check to see if it matches any special
    // keywords and send back the corresponding example. Otherwise, just echo
    // the text we received.
    switch (messageText) {
          case 'image':
            //sendImageMessage(senderID);
            break;

          case 'button':
            //sendButtonMessage(senderID);
            break;

          case 'backand':
          case 'Backand':
            sendGenericMessage(senderID);
            break;

          case 'receipt':
            //sendReceiptMessage(senderID);
            break;

          default:
            sendTextMessage(senderID, messageText);
        }
    } else if (messageAttachments) {
        sendTextMessage(senderID, "Message with attachment received");
    }
};
```

*sendTextMessage* formats the message to be sent to Facebook, then calls the appropriate API endpoint:

```
function sendTextMessage(recipientId, messageText) {
    var messageData = {
        recipient: {
          id: recipientId
        },
        message: {
          text: "Back& bot says: " + messageText
        }
    };

    callSendAPI(messageData);
};
```

*callSendAPI* calls the Send API to send the message back to the user:

```
function callSendAPI(messageData) {
    try{

        var response = $http({
            method: "POST",
            url:"https://graph.facebook.com/v2.6/me/messages",
            params:{
                "access_token": PAGE_ACCESS_TOKEN
            },
            data: messageData,
            headers:{"Content-Type":"application/json"}
        });

        var recipientId = response.recipient_id;
        var messageId = response.message_id;

        console.log("Successfully sent generic message with id " + messageId + " to recipient " + recipientId);
    }
    catch(err){
        console.error("Unable to send message.");
        console.error(err);
    }

}
```

### *Send a Structured Message*

*receivedMessage* can also send back other kinds of messages if it sees certain keywords. For example, if you send the message 'backand', it will call `sendGenericMessage()` - a function that sends back a Structured Message with a generic template.

```
function sendGenericMessage(recipientId) {
    var messageData = {
        recipient: {
            id: recipientId
        },
        message: {
            attachment: {
                type: "template",
                payload: {
                    template_type: "generic",
                    elements: [{
                        title: "Messanger BAAS",
                        subtitle: "Backand as a service for Facebook Messanger",
                        item_url: "https://www.backand.com/features/",
                        image_url: "https://www.backand.com/wp-content/uploads/2016/01/endless.gif",
                        buttons: [{
                            type: "web_url",
                            url: "https://www.backand.com/features/",
                            title: "Open Web URL"
                        }, {
                            type: "postback",
                            title: "Call Postback",
                            payload: "Payload for first bubble",
                        }],
                    }, {
                        title: "3rd Party Integrations",
                        subtitle: "Connect your Bot to 3rd party services and applications",
                        item_url: "https://www.backand.com/integrations/",
                        image_url: "https://www.backand.com/wp-content/uploads/2016/01/3.png",
                        buttons: [{
                            type: "web_url",
                            url: "https://www.backand.com/integrations/",
                            title: "Open Web URL"
                        }, {
                            type: "postback",
                            title: "Call Postback",
                            payload: "Payload for second bubble",
                        }]
                    }]
                }
            }
        }
    };
    callSendAPI(messageData);
};
```

### *Handle Postbacks*
Structured messages use 'postbacks' to communicate with your application when a user clicks on one of the provided enhanced objects. The postback message contains the payload that was created for the button in the original Structured Message. Buttons on Structured Messages support both opening URLs and communicating via postbacks.

In our webhook handler, we handle the postback by calling the function 'receivedPostback()':

```
....
else if (messagingEvent.postback) {
    receivedPostback(messagingEvent);
}
...
```

This function sends a message back saying that the postback was called:

```
function receivedPostback(event) {
    var senderID = event.sender.id;
    var recipientID = event.recipient.id;
    var timeOfPostback = event.timestamp;

    // The 'payload' param is a developer-defined field which is set in a postback
    // button for Structured Messages.
    var payload = event.postback.payload;

    console.log("Received postback for user " + senderID + " and page " + recipientID + "with payload '" + payload + "' " +
    "at " + timeOfPostback);

    // When a postback is called, we'll send a message back to the sender to
    // let them know it was successful
    sendTextMessage(senderID, "Postback called");
};
```

From here, you can expand the bot to provide a wealth of functionality to your page's users. Simply expand upon the above code until it meets your requirements.
