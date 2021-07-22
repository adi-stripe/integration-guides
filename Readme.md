# API Introduction

Create a Customer

```bash
curl -X POST https://api.stripe.com/v1/customers \
  -u {SECRET_KEY}:  \
  -d ="undefined"
```

Update the Customer with an email address

Use the Customer ID from the result of the previous step

```bash
curl -X POST https://api.stripe.com/v1/customers/{customer} \
  -u {SECRET_KEY}:  \
  -d email="customer@example.com"
```

Create a Product

```bash
curl -X POST https://api.stripe.com/v1/products \
  -u {SECRET_KEY}:  \
  -d name="Example Product"
```

Create a Price

Use the Product ID from the previous response

```bash
curl -X POST https://api.stripe.com/v1/prices \
  -u {SECRET_KEY}:  \
  -d product="prod_JpRQi1OQg9ol1d" \
  -d currency="usd" \
  -d unit_amount=4200
```

Create a Checkout Session

Use the Price ID and Customer ID from the previous steps

```bash
curl -X POST https://api.stripe.com/v1/checkout/sessions \
  -u {SECRET_KEY}:  \
  -d mode="payment" \
  -d success_url="https://example.com/?success" \
  -d cancel_url="https://example.com/?cancel" \
  -d payment_method_types[0]="card" \
  -d line_items[0][price]="price_1JBmYzLieeNbcBYsrZt3V1Ok" \
  -d line_items[0][quantity]=1 \
  -d customer="cus_JpRO9l6zURaSqx"
```

