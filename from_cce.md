[MERCHANT] is an X type business and is using Stripe for X,Y,Z.

Billing Primitives
The core objects used to construct a Stripe Billing integration are modeled below.

Billing Primitives

Customer Objects
As outlined in the Payments section, customer objects reuse payment details for subscriptions or other purchases. After you receive the payment method token from the buyer, attach it to a customer object.

curl https://api.stripe.com/v1/payment_methods/{{PAYMENT_METHOD_ID}}/attach \
  -u {TENANT_SECRET_KEY}: \
  -d customer={{customer_ID}}         #create customer before this API call
An example request to attach payment method to customer

For Stripe Billing specifically, it’s important to set the customer’s default invoicing card property to ensure that the correct card is automatically charged for recurring subscriptions.

curl https://api.stripe.com/v1/customers/{{CUSTOMER_ID}} \
  -u {TENANT_SECRET_KEY}: \
  -d invoice_settings[default_payment_method]={{PAYMENT_METHOD_ID}}
Setting default invoicing card

Price Creation
In Stripe’s Billing terminology, a price represents actual subscription timing, currency, and price that a customer pays.

curl https://api.stripe.com/v1/prices \
  -u {TENANT_SECRET_KEY}: \
  -d currency=usd \
  -d interval=month \
  -d product={{PRODUCT_ID}} \
  -d nickname="Monthly" \
  -d amount=3000
An example price creation request using the /prices endpoint

There’s a one-to-many relationship between prices and products, so it’s possible (and common) for one product to have many different prices.
