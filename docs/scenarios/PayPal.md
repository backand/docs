#Accepting Payments

Once you've decided how you want to monetize your web app, you'll likely begin looking at ways to integrate your code with a vendor that will accept and process payments on your behalf. While this would be a daunting effort to perform in a custom-built environment, Backand's built-in security and stability help to ease many of the requirements that are imposed upon payment applications. Below we'll look at using a third-party vendor – [Stripe](http://www.stripe.com/) – to process payments, and examine how to integrate Stripe with a Backand application.

A quick note: The below text was adapted from the [PayPal developer docs](https://developer.paypal.com/docs/integration/direct/make-your-first-call/#make-an-api-call) and as such may not be directly usable without some modification. Please study the code to understand the content, rather than simply trying to cut-and-paste the client-side components.
PayPal provides many types of payments including Express checkout, transactions, refund, payment with agreementId etc, for this example we choose to implement the fast and straight forward Express checkout.
# Why use a vendor?

The first question that you are likely asking is “Why should I use a third-party vendor for my payment system?” The short answer, albeit not a very good one, is that a solid and reliable payments platform is not only extremely challenging, but extremely expensive as well. Setting aside the data concerns (which we have covered elsewhere), you'll need significant liquid assets available to even begin working with financial systems directly. Furthermore, you'll be accepting full fraud risk for your users – if any of your users tries to use your payments system for illegal or unethical ends, then you will be liable for the damages caused. These are just a few of the many concerns that would come with building a payment system from the ground up without using a payment vendor.

With a payment vendor, on the other hand, you are able to offload all of the above issues to the vendor, allowing you to focus instead on integrating payments into your application. Stripe performs underwriting (which handles any potentially fraudulent individuals by simply preventing them from accepting payments), payment processing, communication with financial systems, and all necessary reporting as the transactions move through the various banks and financial APIs. While it would be possible to perform all of this work yourself, you'll save yourself years of labor by simply leveraging someone else's efforts.

# Server-side Action

We'll begin the application by adding a custom server-side action. This action will execute JavaScript that implements a 2 stages process - payment API call   and approval and Approval API call, in each step we will use an access token obtained using PayPal oauth2/token call, to save calling PayPal on the sconde stage will save the token in a server side cookie (in case the token is expired will have to reach out again to PayPal oauth2/token - in the try/catch code). The first stage prepares the payment and returns a url for redirecting the user to PayPal site , after the user acknowledge the payment, PayPal redirects again to your application and awaits your Approval Api call to complete the payment. Simply use the following code in your server-side action to log your payment:
don't forget to change the PayPal credential in the code below, to get these credentials you can [create a PayPal sandbox account](https://developer.paypal.com/developer/accounts/) ( you'll need to sign-up first in case you have done that already).

```javascript

   var paypalUrl = 'https://api.sandbox.paypal.com/';

   // PayPal has a 2 stage process where: // the first stage prepares the payment and a returns a url for the user to pay
   // and the sconde stage (where the payment is actually done) is where the application send an approval API call to PayPal
   if (parameters.process == 'payment') {
     var payment = postPayment();
     return payment.links[1].href;
   }
   else if (parameters.process == 'approval') {
     return postApproval();
   }

   function postPayment() {
     var authorization = "Bearer " + getAccessToken().access_token;
     var payment = {
       "intent": "sale",
       "redirect_urls": {
         "return_url": "http://localhost:3000/#/paypal",
         "cancel_url": "http://localhost:3000/#/paypal?fail=true"
       },
       "payer": {"payment_method": "paypal"},
       "transactions": [
         {
           "amount": {
             "total": parameters.amount,
             "currency": "USD"
           }
         }
       ]
     };
     try {
       return $http({
         method: 'POST',
         url: paypalUrl + 'v1/payments/payment',
         data: payment,
         headers: {
           "Content-Type": "application/json",
           "Accept-Language": "en_US",
           "Authorization": authorization

         }
       });
     }
     catch (err) {
       if (err.name == 401) {
         cookie.remove('paypal_token');
         return postPayment();
       }
       else {
         throw err;
       }
     }
   }

   function postApproval() {
     var authorization = "Bearer " + getAccessToken().access_token;
     var payer = {"payer_id": parameters.payerId};
     try {
       return $http({
         method: 'POST',
         url: paypalUrl + 'v1/payments/payment/' + parameters.paymentId + '/execute/',
         data: JSON.stringify(payer),
         headers: {"Content-Type": "application/json", "Accept-Language": "en_US", "Authorization": authorization}
       });
     }
     catch (err) {
       if (err.name == 401) {
         cookie.remove('paypal_token');
         return postApproval();
       }
       else {
         throw err;
       }
     }
   }

   function getAccessToken() {
     var token = cookie.get('paypal_token');
     if (!token) {
       var ClientId = 'YOUR_PayPal_CLIENT_ID';
       var Secret = 'YOUR_PayPal_SECRET_KEY';
       var user = btoa(ClientId + ":" + Secret);

       try {
         token = $http(
           {
             method: 'POST',
             url: paypalUrl + 'v1/oauth2/token',
             data: 'grant_type=client_credentials',
             headers: {
               "Accept-Language": "en_US",
               "Authorization": "Basic " + user
             }

           });
       }
       catch (err) {
         if (err.name == 401) {
           var e = new Error("Unauthorized (401), check client id and secret");
           e.name = err.name;
           throw e;
         }
         else {
           throw err;
         }
       }
       cookie.put('paypal_token', token);
     }
     return token;
   }

```

The above code accepts the payment stage in parameters.process and execute the appropriate API call.
in the first stage when process="Payment"- the function postPayment will POST PayPal the amount in usd and our angular client side application pages url where we want the user to be redirected after acknowledging or canceling  the payment.
in the second stage - the Approval stage , the angular application will call the server side action with process="Approval" , PayerId and PaymentId that PayPal implanted in the query string of the return url, the postApproval function will do a POST call to PayPal with these parameters and the result will be a PayPal payment object including all the payment details and status.
# Client-side Integration

Once you've got the server-side action, you are just about ready to start accepting payments. The code below performs two tasks. It first calls a service to prepare the payment (Payment API in PayPal) the server side action then returns a url, the code redirect the user to this url.

```javascript
      var self = this;

        self.charge = function () {
          self.error = "";
          self.success = "";

          //get the form's data
          var form = angular.element(document.querySelector('#paypal-form'))[0];

          //Call Backand action to prepare the payment
          var paypalUrl = BackandService.makePayPalPayment(form.amount.value)
            .then(function (payment) {
              var paypalResponse = payment;
              //redirect to PayPal - for user approval of the payment
              $window.location.href = paypalResponse.data;
            })
            .catch(function (err) {
              if (err.type) {
                self.error = 'PayPal error: ' + err.message;
              } else {
                self.error = 'Other error occurred, possibly with your API' + err.message;
              }
            });
        }
  ```

  Then , when the user approves/cancels the Payment ,PayPal the redirect the browser to the page you sent in the server- side action, in this example we used the same page by checking the querystring paymentid value to decide the stage in which we are at.


  ```javascript
        //check if this is a redirect from PayPal , after the user approves the payment
        // PayPal adds PayerID and  paymentId to the return url we give them

        if ($location.search().PayerID && $location.search().paymentId) {

          //Call Backand action to approve the payment by the facilitator
          BackandService.makePayPalApproval($location.search().PayerID, $location.search().paymentId)
            .then(function (payment) {
              // remove PayPal query string from url
              $location.url($location.path());
              self.success = 'successfully submitted payment for $' + payment.data.transactions[0].amount.total;
            }
          )
      }
```

Once this code has been created, we need to implement the service which calls Backand server side action. This service wraps a HTTP request to the on-demand  action we created previously. It assumes that the action's name is `PayPalPayment`, so be sure to change it to something that matches your actual implementation:

```javascript
/Call PayPalPayment on demand action
    factory.makePayPalPayment = function (amount) {

      return $http({
        method: 'GET',
        url: Backand.getApiUrl() + '/1/objects/action/items/1',
        params: {
          name: 'PayPalPayment',
          parameters: {
            amount: amount,
            process:"payment"
          }
        }
      });
    };

    //Call PayPalApproval on demand action
    factory.makePayPalApproval = function (payerId, paymentId) {
        return $http({
        method: 'GET',
        url: Backand.getApiUrl() + '/1/objects/action/items/1',
        params: {
          name: 'PayPalPayment',
          parameters: {
            payerId: payerId,
            paymentId: paymentId,
            process:"approval"
          }
        }
      });
    };
```

And with that, we've added the basic code necessary to contact PayPal and record a payment.

# Conclusion

The above code gives us the basis of a payments system. It can accept payments from a user, which are recorded against your own PayPal account. You can use this as a basis for a larger application, that provides a deeper integration with PayPal's API. Simply follow the pattern of wrapping the API calls in server-side actions, then calling those actions via the Backand API, and you'll have a solution built very rapidly that is both secure and scalable from day one.

