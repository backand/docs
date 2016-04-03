## Accepting Payments

Once you've decided how you want to monetize your web app, you'll likely begin looking at ways to integrate your code with a vendor that will accept and process payments on your behalf. While this would be a daunting effort to perform in a custom-built environment, Backand's built-in security and stability help to ease many of the requirements that are imposed upon payment applications. Below we'll look at using a third-party vendor – [PayPal](http://www.paypal.com/) – to process payments, and examine how to integrate PayPal with a Backand application.

A quick note: The below text was adapted from the [PayPal developer docs](https://developer.paypal.com/docs/integration/direct/make-your-first-call/#make-an-api-call), and as such may not be directly usable without some modification. Please study the code to understand the content, rather than simply trying to cut-and-paste the client-side components.

PayPal provides many options for accepting payments, including Express Checkout, transactions, refunds, payment with agreementId, and so on. In this example, we will implement Express Chackout as it is the simplest and most straightforward method to enable.

## Why use a vendor?

The first question that you are likely asking is “Why should I use a third-party vendor for my payment system?” The short answer, albeit not a very good one, is that a solid and reliable payments platform is not only extremely challenging, but extremely expensive as well. Setting aside the data concerns (which we have covered elsewhere), you'll need significant liquid assets available to even begin working with financial systems directly. Furthermore, you'll be accepting full fraud risk for your users – if any of your users tries to use your payments system for illegal or unethical ends, then you will be liable for the damages caused. These are just a few of the many concerns that would come with building a payment system from the ground up without using a payment vendor.

With a payment vendor, on the other hand, you are able to offload all of the above issues to the vendor, allowing you to focus instead on integrating payments into your application. PayPal performs underwriting (which handles any potentially fraudulent individuals by simply preventing them from accepting payments), payment processing, communication with financial systems, and all necessary reporting as the transactions move through the various banks and financial APIs. While it would be possible to perform all of this work yourself, you'll save yourself years of labor by simply leveraging someone else's efforts.

## Server-side Action

We'll begin the application by adding a custom server-side action. This action will execute JavaScript that implements a 2 stage process. It performs the payment API call, and then the Approval API call. Each step uses an access token obtained from PayPal's oauth2/token call. To save a call to the oauth endpoint for an extra token, we'll cache the oauth token in a server side cookie (although if the cookie expires, you'll need to refresh the token from the oauth2/token endpoint. See the try-catch block below). The first stage prepares the payment and returns a url for redirecting the user to PayPal site. The second stage occurs after the user has acknowledged the payment and been brought back to your application, at which point you call the Approval API to complete the payment. THe code example below is a server-side action that implements all of the functionality necessary to register an Express Checkout payment with PayPal.

Please note: Don't forget to change the PayPal credentials in the code below. These credentials represent your association with PayPal, and must be correct in order for your customer's funds to be correctly routed. To obtain these credentials, you can [create a PayPal sandbox account](https://developer.paypal.com/developer/accounts/). Note that you will first need to register for PayPal if you have not already done so.

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

The above code takes the payment stage information in parameters.process, and executes the approprite PayPal API call. In the first stage, when directing the user to PayPal to confirm the payment, parameters.process will contain "Payment". This results in the function postPayment being called, which sends a POST command to PayPal with the payment information, along with a URL to redirect to when the user either accepts or cancels the payment. In the second payment stage, when contacting the Approval API to complete the payment, parameters.process will contain "Approval". This will result in the code calling postApproval, which issues a POST call to PayPal using the PayerID and PaymentID stored in the query string of the return URL. This function will return a PayPal payment object including all of the relevant payment details and the payment status.

## Client-side Integration

Once you've got the server-side action, you are just about ready to start accepting payments. The code below performs two tasks. It first calls a service to prepare the payment (the Payment API in PayPal), which returns a URL to which the user is redirected.

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

  Once the user has accepted or cenceled the payment, PayPal redirects them back to the URL that was provided to the server-side action. In this example, we use the same page by checking the query string parameter paymentid. This allows the code to detect which stage of the payment process we are at.

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

Once this code has been added to your project, we need to implement the service that calls Backand's server side payment action. This service wraps a HTTP request to the on-demand custom action that we created earlier in the tutorial. It assumes the action name is 'PayPalPayment', so be sure to change it as appropriate to match your actual implementation:

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

## Conclusion

The above code gives us the basis of a payments system. It can accept payments from a user, which are recorded against your own PayPal account. You can use this as a starting point for a larger application that provides a deeper integration with PayPal's API. Simply follow the pattern of wrapping the API calls in server-side actions, then calling those actions via the Backand API, and you'll have a solution built very rapidly that is both secure and scalable from day one.

