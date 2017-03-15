# bsodmike-billing-task

## API

The RESTful API exposed in this app is documented below.  For the purpose of this task, this API remains 'unsecured'.  Any production use, should implement token-authentication, and include other basic-safeguards such as CORS/CSRF protection via modules such as `helmet` or `lusca`.

### POST /customers
Stripe purchases are typically made using `stripe.js` in the browser to obtain a token for the purchase card details; where this token is passed to internal APIs to process the actual charge.

However, the rules of this task state that only 3-API end-points are 'exposed', and since all end-points are public, I did not create a 4th end-point so as to obtain a customer give the customer token &mdash; although I could have done so, and made that a token-authenticated URI only, but I did not want to risk breaking the previously mentioned rule.

Therefore, creating a customer requires the card details to be passed as well.  This end-point expects a JSON payload with the following required details:

```
{
    // required
    email:  'john.dorian@cia.gov',
    source: {
      name: 'John Dorian',
      number: '4242424242424242',
      cvc: '123',
      expMonth: '12',
      expYear: '20',
    }
    
    // optional
    // An arbitrary string that you can attach to a customer object
    description: 'New Customer',
    
    // A set of key/value pairs that you can attach to a customer object
    meta_data: { }
}
```

Any failure will return a `422` status with error details as the JSON response. 

### POST /charges

This end-point expects a JSON payload with the following required details:

```
{   
    // required
    // The customer token
    customer: 'cus_AIBgNxPoMy8ITG',
    
    currency: 'usd',
    
    // amount in cents; string is also fine.
    amount: 2000,
}
```

Any failure will return a `422` status with error details as the JSON response. 