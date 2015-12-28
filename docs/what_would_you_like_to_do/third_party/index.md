#Leveraging the Work of Others

Libraries have built the modern world. In a post originally on Google Plus, Steve Yegge detailed how it was [platform development](https://gist.github.com/chitchcock/1281611) that provided the basis for success of a lot of the major technology companies, using Amazon, Microsoft, and Facebook as examples. True platform-based development isn't possible without a mechanism to integrate with those platforms, though. Below we'll look at the options available to you when developing a Backand-backed web app, detailing which APIs work best at each stage of the application.

# Guidelines on Integrations

As a general rule, you'll see two overall ideas put forth in this article:

1. The client side of your application can integrate with any library that is available on the client side. Most commonly, this will consist of JavaScript libraries.
1. The server side of your application can integrate with any web service that bases its communication off of HTTP web requests.

Most modern libraries you are likely to consume will fit into one of these two categories, so integration usually shouldn't be a concern. However, some functionality will not be available to you if it relies upon complex server libraries or non-HTTP integrations, such as SOAP web services. As such, it is critically important that you plan your library usage thoroughly, and understand the requirements and auxiliary dependencies of each vendor you choose to integrate with. You can see a few examples of this in action on Backand's GitHub account:

* [This project](https://github.com/backand/stripe-example) shows how to use Backand perform a basic integration with Stripe.
* Backand's [Noterious app](https://github.com/backand/simple-noterious-app), a note board application backed by Backand's SDK, demonstrates using Backand to integrate with Amazon Web Services within a larger application.

# Client-side Integrations

As mentioned above, any library available to a client-side web app should be available to your AngularJS app's client-side code. Simply include the required library along with your other dependencies, and you should be able to use the library just as you would in any other application. One potential issue to be faced, though, is that of callbacks. Due to Backand providing the server-side integration for your web app, you will want to create On-Demand Custom Actions to respond to any callbacks sent from the third party you are consuming. Simply create the endpoint, specify the expected parameters, and implement the processing for the delayed notification within the JavaScript action, then provide the URL for that action as your application's callback URL. This approach allows your app to scale more quickly, as the processing load is handled primarily by Backand as opposed to your own servers.

# Server-side Integrations

Server-side integrations of third party libraries are restricted to those libraries that communicate via HTTP requests. This can mean a RESTful API, or just a series of simple HTTP web services that accept JSON parameters. Any library that can integrate natively with JavaScript code can be implemented in this manner. Take, for example, an integration with [Stripe's REST API](https://stripe.com/docs/api). While Stripe helpfully provides encapsulating libraries in a number of languages, all of their functionality is available via simple web requests which can be accessed via CURL. These endpoints are ideal for server-side actions, which can use JavaScript's built in AJAX communication functionality to hit the same endpoints. Simply wrap each endpoint in a custom server-side action, then modify your client-side code to call your action wrappers. In this way, you can integrate with any HTTP API.

While many of these libraries have code that can exist in either the client or the server, the approach above is ideal when security is a consideration. If you're dealing with a customer's sensitive personal info, a client side-based implementation may expose your users to unnecessary risk. By moving these calls to server-side custom actions, you can protect your users' information by encrypting the communications with HTTPS web requests (already provided by Backand), and put the sensitive code behind your application's administrative dashboard.
