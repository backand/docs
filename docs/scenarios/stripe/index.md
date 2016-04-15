## Accepting Payments

Once you've decided how you want to monetize your web app, you'll likely begin looking at ways to integrate your code with a vendor that will accept and process payments on your behalf. While this would be a daunting effort to perform in a custom-built environment, Backand's built-in security and stability help to ease many of the requirements that are imposed upon payment applications. Below we'll look at using a third-party vendor – [Stripe](http://www.stripe.com/) – to process payments, and examine how to integrate Stripe with a Backand application.

A quick note: In our [Stripe Example Application](https://github.com/backand/stripe-example), we used [angular-stripe](https://github.com/bendrucker/angular-stripe) – a popular third-party tool that allows you to easily interface with Stripe from your Angular code – to make calls to Stripe's Stripe.js file, which provides most of the payment tokenization functionality. This is obviously a convenience, and you are free to call Stripe.js directly from your application, but the below text assumes you are also using this library. It is also important to note that the code below – particularly the client side code – was adapted from the Stripe example application, and as such may not be directly usable without some modification. Please study the code to understand the content, rather than simply trying to cut-and-paste the client-side components.

## Why use a vendor?

The first question that you are likely asking is “Why should I use a third-party vendor for my payment system?” The short answer, albeit not a very good one, is that a solid and reliable payments platform is not only extremely challenging, but extremely expensive as well. Setting aside the data concerns (which we have covered elsewhere), you'll need significant liquid assets available to even begin working with financial systems directly. Furthermore, you'll be accepting full fraud risk for your users – if any of your users tries to use your payments system for illegal or unethical ends, then you will be liable for the damages caused. These are just a few of the many concerns that would come with building a payment system from the ground up without using a payment vendor.

With a payment vendor, on the other hand, you are able to offload all of the above issues to the vendor, allowing you to focus instead on integrating payments into your application. Stripe performs underwriting (which handles any potentially fraudulent individuals by simply preventing them from accepting payments), payment processing, communication with financial systems, and all necessary reporting as the transactions move through the various banks and financial APIs. While it would be possible to perform all of this work yourself, you'll save yourself years of labor by simply leveraging someone else's efforts.

## Server-side Action

We'll begin the application by adding a custom server-side action. This action will execute JavaScript that accepts a payment token as an argument, and then uses that token to register a payment with Stripe. Simply use the following code in your server-side action to log your payment:

```javascript
  // Secret key - copy from your Stripe account   
  // - https://dashboard.stripe.com/account/apikeys - 
  // or use Backand's test account
  var user = btoa("sk_test_hx4i19p4CJVwJzdf7AajsbBr:");

  response = $http({
    method: "POST",
    url: "https://api.stripe.com/v1/charges",
    params: {
        "amount":parameters.amount,
        "currency":"usd",
        "source": parameters.token
    },
    headers: {
        "Authorization": "Basic " + user,
        "Accept" : "application/json",
        "Content-Type": "application/x-www-form-urlencoded"
    }
  });

return {"data":response};
```

The above code simply takes all of the information needed for a payment – specifically the amount (in pennies) and the payment method token – and sends them to Stripe via a POST request to their [RESTful API](https://stripe.com/docs/api). You can create as many of these actions as you like, wrapping additional Stripe API calls at will.

## Client-side Integration

Once you've got the server-side action, you are just about ready to start accepting payments. There are two steps that need to take place on the client side before you can do so. First, we'll look at setting the API Key.

The API Key tells the Stripe.js file that the actions being taken are to take place under your Stripe account. On the server side, this is the Secret Key that you obtain from the Stripe dashboard. The client side needs something more secure, though, as anyone with your secret key could impersonate your app and cause security issues. Recognizing this, Stripe provides you with a public key that you can use for client-side code. This is secured against a secret key on your Stripe account, and cannot be used to actually register payments or perform transfers. To set your public key, use the following code:

```javascript
  .config(function (stripeProvider) {
    //Enter your Stripe publish key or use Backand test account
    stripeProvider.setPublishableKey('pk_test_pRcGwomWSz2kP8GI0SxLH6ay');
  }) 
```

Once you've set your publishable key, you're ready to start tokenizing payment methods. The code below performs two tasks. It first creates a token from the payment data entered into your app's UI, then it sends that token to the custom action that we created in the previous section by calling a convenience method. We'll first add the tokenization and payment success code:

```javascript
  self.charge = function(){

    self.error = "";
    self.success = "";

    //get the form's data
    var form = angular.element(document.querySelector('#payment-form'))[0];

    //Use angular-stripe service to get the token
    return stripe.card.createToken(form)
      .then(function (token) {
        console.log('token created for card ending in ', token.card.last4);

        //Call Backand action to make the payment
        return BackandService.makePayment(form.amount.value, token.id )
      })
      .then(function (payment) {
        self.success = 'successfully submitted payment for $' + payment.data.data.amount/100.0;
      })
      .catch(function (err) {
        if (err.type && /^Stripe/.test(err.type)) {
          self.error = 'Stripe error: ' +  err.message;
        }
        else {
          self.error = 'Other error occurred, possibly with your API' + err.message;
        }
      });
  }
```

Once this code has been created, we need to write one additional function. This function wraps a HTTP request to the 
custom action we created previously. It assumes that the action's name is 'makePayment', so be sure to change it to 
something that matches your actual implementation:

```javascript
factory.makePayment = function(amount, token){
    return $http({
      method: 'GET',
      url: Backand.getApiUrl() + '/1/objects/action/items?name=makePayment',
      params:{
        parameters:{
          token: token,
          amount: amount
        }
      }
    });
  }
```

And with that, we've added the basic code necessary to contact Stripe and record a payment. 

## Conclusion

The above code gives us the basis of a payments system. It can accept payments from a user, which are recorded against your own Stripe account. You can use this as a basis for a larger application, that provides a deeper integration with Stripe's API. Simply follow the pattern of wrapping the API calls in server-side actions, then calling those actions via the Backand API, and you'll have a solution built very rapidly that is both secure and scalable from day one.
