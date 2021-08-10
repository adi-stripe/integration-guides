# subscriptions-price-update

## Step 1: Create Product

```bash
curl -X POST https://api.stripe.com/v1/products \
  -u {SECRET_KEY}:  \
  -d name="TV Subscription - Original Price"
```

## Step 2: Create Price for 189 DKK

Use the product id from Step 1.

```bash
curl -X POST https://api.stripe.com/v1/prices \
  -u {SECRET_KEY}:  \
  -d currency="dkk" \
  -d product={prod_step_1} \
  -d unit_amount=18900 \
  -d recurring[interval]="month"
```

## Step 3: Create Customer 1

In this step you create a Customer. Note - for speed of use there is a card already attached as default. 

```bash
curl -X POST https://api.stripe.com/v1/customers \
  -u {SECRET_KEY}:  \
  -d invoice_settings[default_payment_method]="pm_card_visa" \
  -d payment_method="pm_card_visa"
```

## Step 4: Create Subscription

Create Subscription for Customer 1 and Price 189 DKK

```bash
curl -X POST https://api.stripe.com/v1/subscriptions \
  -u {SECRET_KEY}:  \
  -d customer={cus_1_from_step_3} \
  -d items[0][price]={price_1_from_step_2}
```

## Step 5: Create Price for 199 DKK

```bash
curl -X POST https://api.stripe.com/v1/prices \
  -u {SECRET_KEY}:  \
  -d currency="dkk" \
  -d product={prod_step_1} \
  -d unit_amount=19900 \
  -d recurring[interval]="month"
```

## Step 6: Create a new Customer

```bash
curl -X POST https://api.stripe.com/v1/customers \
  -u {SECRET_KEY}:  \
  -d invoice_settings[default_payment_method]="pm_card_visa" \
  -d payment_method="pm_card_visa"
```

## Step 7: Create a new Subscription 

The Subscription will be for Customer 2 and attached to Price 199 DKK

```bash
curl -X POST https://api.stripe.com/v1/subscriptions \
  -u {SECRET_KEY}:  \
  -d customer={cus_2_from_step_6} \
  -d items[0][price]={price_2_from_step_5}
```

## Step 8: List Subscriptions

List all the Subscriptions with Price 189 DKK (the original one)

```bash
curl https://api.stripe.com/v1/subscriptions \
  -u {SECRET_KEY}:  \
  -d price={price_1_from_step_2}
```

## Step 9: Update Subscription

For the Subscriptions retrieved in Step 8, update the price from the one for 189 DKK to the one for 199 DKK. Set proration to none so that the change will apply to the next invoice. 

```bash
curl -X POST https://api.stripe.com/v1/subscriptions/{subscription_exposed_id} \
  -u {SECRET_KEY}:  \
  -d items[0][id]={si_step_8_items_data_0_id} \
  -d items[0][price]={price_2_from_step_5} \
  -d proration_behavior="none"
```

## Step 10: Get Subscription

Get Subscription 1 to verify the new Price.

```bash
curl https://api.stripe.com/v1/subscriptions/{subscription_exposed_id} \
  -u {SECRET_KEY}: 
```

