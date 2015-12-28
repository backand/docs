#Realtime Database Communication

Backand's real-time communication functionalty is based on the popular open-source Socket.io framework, which lets you add real-time functionality to your application. Backand’s Real-Time Database Communication sends events and JSON-formatted data to any authorized connected client. With Real-Time Communcation from Backand, you can send real-time information to your application based on server-side logic in your application's custom actions. With this new feature events are picked up as they happen, rather than having to wait for a user-driven event to trigger a data reload.

Backand’s real-time database communications are completely secure. They provide you with total control over data access, allowing you to restrict transmission of sensitive data to only authorized roles. And since all communication is SSL-encrypted, you don’t have to worry about the security of your data as it is transmitted across the web.

Using the real-time capability can enahnce your app with instant updates to any Angular page, including updating charts, counters, logs, and other data-driven elements.

#Setup

1. Upgrade to Backand SDK 1.8.2 or above.
2. Include the following script in the index.html page:

  ```
<!-- Backand Realtime -->
  <script src="https://api.backand.com:4000/socket.io/socket.io.js"></script>
  ```

3. Update Angular configuration section with:

  ```
BackandProvider.runSocket(true);
  ```

# Angular client code

Backand's SDK already includes all the code needed for the client-side Socket.IO JavaScript integration. All you need to do is add a listener in your Angular controller or service. In the event that you need to implement your client on a platform other than JavaScript, review the relevant [iOS](http://socket.io/blog/socket-io-on-ios/) or [Android](http://socket.io/blog/native-socket-io-and-android/) documentation.

Using Backand's SDK, you can add event listening to your application with a simple function:

```
Backand.on('items_updated', function (data) {
  //Get the 'items' object that have changed
  console.log(data);
});
```

And that's it! You don't even need to provide an emit event to track changes - Backand will automatically update you whenever you change data using Backand's REST API out of the box.

# Server side code

When working with a Custom JavaScript Action in the Backand dashboard, you can add the "emit" command to notify the client of updates based upon events and sent data that are important to your use case. The actions can be based on database triggers (Create, Update or Delete), or in an on-demand action called from client-side logic.

There 3 type of emit commands you can use in order to notify consuming applications of event data:

* socket.emitUsers: `socket.emitUsers('event_Name',object, []);`

This command sends event only to users specified in the second argument, which should be formatted as an array. To make this array dynamic, you'll need to use a Backand Query action. First call the Query to obtain the users, then convert the JSON into an array of user IDs (email addresses).

Example:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitUsers("items_updated",userInput, ["user2@gmail.com","user1@gmail.com"]);
}
```

* socket.emitRole: ```socket.emitUsers('event_Name',object, 'role');``

This command sends the event to all the users in a specified role. Be aware that this command will overrided any data security filter you may have configured for your application.

Example:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitRole("items_updated",userInput, "Role");
}
```

* socket.emitAll: ```socket.emitAll('event_Name',object);```

This command sends event to all connected users, regardless of their role or permission settings. Be aware that this command verrides any data security filters you may have configured for your application. Use this emit type for general event broadcasting.

Example:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitAll("items_updated", userInput);
}
```
