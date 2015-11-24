#Realtime Database Communication

The real-time communication is based on the popular open source Socket.io framework, which lets you add real-time functionality to your application. Backand’s Real-Time Database Communication sends events and JSON-formatted data to any authorized connected client. Based on server-side logic in Backand's Actions you can send real-time information to your application.
Instead of having to wait for the user refresh the page, the events are picked up as they happened.

Backand’s real-time database communications are completely secure. They provide you with total control over data access, allowing you to restrict transmission of sensitive data to only authorized roles. And since all communication is SSL-encrypted, you don’t have to worry about the security of your data as it is transmitted to the web.

Using the real-time capability you can improve your app with instant updates to any Angular page, update charts, counters, logs, and other data-driven elements.

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

The backand's SDK already includes all the code needed by the socket.io client JavaScript code. All you need to do is to add a listener in your Angular controller or service. In case you need to implement the client on other platform then JavaScript, please review this the [iOS](http://socket.io/blog/socket-io-on-ios/) and [Android](http://socket.io/blog/native-socket-io-and-android/) documentations.

Using Backand's SDK, you only need this line for the listening:

```
Backand.on('items_updated', function (data) {
  //Get the 'items' object that have changed
  console.log(data);
});
```

The emit event is not needed, when you update a data just use Backand REST API commands.

# Server side code

Using Backand's Action in the Backand dashboard, add the "emit" command to notify the client of updates based upon events and sent data.
The actions can be based on database triggered (Create, Update or Delete) or in the on-demand action driven by client code logic.

There 3 type of emit commands you can use in order to notify on event and send data:

* socket.emitUsers: ```socket.emitUsers('event_Name',object, []);``

This command sends event only to the users specified in the array. In case you need to make this array dynamic use Backand Query. First call the Query and then convert the JSON into the array of users.

Example:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitUsers("items_updated",userInput, ["user2@gmail.com","user1@gmail.com"]);
}
```

* socket.emitRole: ```socket.emitUsers('event_Name',object, 'role');``

This command sends event to all the users in a specified role. Be aware that this command is overriding any data security filter you may have.

Example:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitRole("items_updated",userInput, "Role");
}
```

* socket.emitAll: ```socket.emitAll('event_Name',object);```

This command sends event to all the connected users, regardless there role or permission. Be aware that this command is overriding any data security you may have. Use this for general event broadcasting.

Example:

```
function backandCallback(userInput, dbRow, parameters, userProfile) {
  socket.emitAll("items_updated", userInput);
}
```