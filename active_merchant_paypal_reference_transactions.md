# Implement Paypal Reference transactions with Active Merchant

[Reference transactions](https://developer.paypal.com/docs/classic/express-checkout/integration-guide/ECReferenceTxns) allow merchants to make subsequent transactions from a customer without the need to re-login into Paypal again. It can be implemented either as recurring payments to support subscription-based services or simply to setup a billing agreement that can be stored and used by merchants to make another payment.

Implement payments with a billing agreement can greatly improve customer experence of your application by reducing customers' effort during checkout. The basic flow for it would be following:

1. The first time a customer checkout on your application, it will set up a billing agreement along with the payment request between your application and customer and send this to Paypal.
2. Customer login into Paypal, accept the payment and billing agreement.
3. Your application complete the payment and store the billing agreement ID into the database.
4. Next time customer check out, your application can directly make payments with Paypal based on the stored billing agreement ID without redirecting customer to Paypal login page.

In this post, I will share you how to implement reference transactions in Ruby with [active_merchant](https://github.com/Shopify/active_merchant) gem and Paypal Express Checkout API.

Important note: In order to use reference transactions in production, your paypal merchant account should get the permission from Paypal. Please contact with Paypal support team about it.

#### Setup paypal gateway

Using your merchant account username, password and signature to setup paypal express gateway:

```ruby
paypal_gateway = ActiveMerchant::Billing::PaypalExpressGateway.new(
  login: ENV["PAYPAL_USERNAME"],
  password: ENV["PAYPAL_PASSWORD"],
  signature: ENV["PAYPAL_SIGNATURE"]
)
```

#### Setup payment and billing agreement

You can setup a payment with billing agreement similar to normal express checkout payment, the only difference is that you need to specify param `billing_agreement` into your options hash. For example:

```ruby
purchase = paypal_gateway.setup_purchase(
  100,
  billing_agreement: {
    type: 'MerchantInitiatedBillingSingleAgreement',
    description: 'Sample billing agreement'
  },
  ip: request.remote_ip,
  return_url: "http://localhost:3000/return",
  cancel_return_url: "http://localhost:3000/cancel",
  description: 'Sample',
  subtotal: 100,
  currency: 'USD',
  shipping: 0,
  handling: 0,
  tax: 0,
  items: [{ name: 'Sample item', quantity: 1, amount: 100 }],
  allow_guest_checkout: true
)
```

In the above example, we set billing agreement type as `MerchantInitiatedBillingSingleAgreement`, which means Paypal will create a single agreement for all transactions between your application and a customer. Other options include `MerchantInitiatedBilling` in which Paypal will create new agreement for every express checkout request and `RecurringPayments` which only used for recurring payments.

After setting up payment successfully, we will redirect the customer to Paypal login page. When login, he/she would see the following notification:

![Paypal](https://dl.dropboxusercontent.com/u/12981551/Review_your_information.png)

If the customer agree, he/she would be redirected back to the return url provided in previous request alongs with `token` and `PayerID` data. From there, your application will double check the payment response using the `token`, make request to store the billing address ID and store it into the database:

```ruby
purchase = paypal_gateway.details_for(params[:token])

return false unless purchase.success?

billing_agreement = paypal_gateway.store(params[:token], {})

if billing_agreement.success?
  billing_agreement_id = billing_agreement.authorization
  # Save billing agreement ID into the database
end
```

Finally, using the stored billing agreement ID to complete the payment, and all subsequent transactions of customer can go directly to this step:

```ruby
purchase = paypal_gateway.reference_transaction(
  100,
  reference_id: billing_agreement_id,
  description: 'Sample',
  subtotal: 100,
  currency: 'USD',
  shipping: 0,
  handling: 0,
  tax: 0,
  items: [{ name: 'Sample item', quantity: 1, amount: 100 }]
)

do_something if purchase.success?
```

#### Cancel a billing agreement

To cancel a billing agreement, you could use the `unstore` method:

```ruby
paypal_gateway.unstore(billing_agreement_id, {})
```

And that's all for this post. Thanks :)