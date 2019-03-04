# Credits

Attribute | Type | Description
--------- | ---- | -------
uuid | `string` | The unique reference ID for the Voucher.
**initial_quantity** | `number` | The number of Credits that will be deducted from a [User](#users).
expires |  `number` | The expiration date of the Credits.
consumed | `number` | The number of Credits the user has consumed.

The `Voucher` or `Credit` is used to pay for an [`Order`](#orders) containing a [`Credit Product`](#credit-product-object).

## Grant Vouchers to User

Admins are able to update vouchers for the users. 
Note that the number of vouchers is set to this value, not incremented by the value.

> Definition

```
PUT https://api.kvass.ai/users/<user_id>?expand=voucher
```

> Example request:

``` http
POST /users HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
  "voucher": 100
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{"$oid":"57ee9c72d76d431f85111999"},
  "_cls":"User",
  "created":{"$date":1475428903950},
  "modified":{"$date":1475428903951},
  "first_name":"John",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[{
                "city":"Danielsen",
                "service":"google",
                "alias":"",
                "country":"Paraguay",
                "zip_code":"0556",
                "state":"Oslo",
                "street_name":"Iversenstien 7"
             }],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{"$oid":"57ee9c71d76d431f8511142f"},
  "tags":[],
  "stripe_customer_id":"",
  "roles":["user"],
  "deleted":true,
  "voucher": {
        'initial_quantity': 100, 
        'expires': {'$date': 1572339915160}, 
        'consumed': 0, 
        'created': {'$date': 1540803915160}, 
        'uuid': '60a4646c-a6ec-4d2b-a031-25cff5b9a8ae'
    }
}
```

Argument | Type | Description
-------- | ---- | -------
**user_id** | `string` | The [`user`](#users)'s unique ID.
voucher | `number` | The new value of the `initial_amount`.


## Voucher Plan

A [`user`](#users) can also get vouchers by [`subscribing`](#subscriptions) to a `Voucher plan`.
A voucher plan share a lot of commonalities with a normal [`Plan`](#plans), but uses only 
[`Credit Product`](#credit-products) in the `plan.items` instead of regular [`Products`](#products).
The number of vouchers corresponding to the sum of the `plan.items.product.vouchers_required` will be charged at every billing interval for each [`user`](#users) . The `total_amount` in the plan,
 the amount that the user will be charged at every billing interval, will be the sum of all
 `plan.items.product.price`.


### Create a Voucher Plan

Argument | Type | Description
-------- | ---- | -------
**method** | `string` | The type of Plan, in this case a `voucher_plan`.
**name** | `string` | The name of the Plan, in this case a `VOUCHER_PLAN`.
**interval_unit** | `string` | The frequency that the Subscription acts upon. <br> interval_unit choices: `DAY`, `WEEK`, `MONTH`, `MONTH_END`, `ANNUAL`
**billing_interval** | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
**currency** | `string`| Three letter ISO currency code as defined by ISO 4217.7
**items** | `array` | An array of [`credit products`](#credit-products) and `quantity`.

> Definition

```
POST https://api.kvass.ai/plans
```

> Example request:

``` http
POST /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "interval_unit": "WEEK",
    "name": "Voucher Plan",
    "currency": "NOK",
    "static": true,
    "billing_interval": "MONTH",
    "items": [{"product": "<credit_product1_id>", "quantity": 1}],
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "note": "This is a note regarding the Plan"
}

```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5931697ed57ba271c0c7de66"},
    "created": {"$date": 1496410494652},
    "updated": {"$date": 1496410494652},
    "auto_bill_amount": false,
    "billing_interval": "MONTH",
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "name": "Voucher Plan",
    "method": "voucher_plan",
    "total_amount": 100.0,
    "interval_unit": "WEEK",
    "items": [
        {
            "discount": 0.0,
            "product": {"$oid": "58f9f856b70e2a56c4a0db35"},
            "quantity": 1
         }],
    "status": "CREATED",
    "currency": "NOK",
    "note": "This is a note regarding the Plan",
    "deleted": false,
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "static": true
}
```

## Redeem Vouchers in an Order

The User can redeem their voucher to pay for Orders only if all the [`Order.items`](#orders) 
are of type [`Credit Products`](#credit-products)


> Definition

```
POST https://api.kvass.ai/orders/<order_id>/redeem?expand=user.voucher
```

> Example request & response:

``` http
POST /orders/<order_id>/redeem?expand=user.voucher HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai


```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "_id":{"$oid":"5bd6de64cb416c6806eb5c4e"},
   "company":{"$oid":"5bd6de52cb416c6806eb5c41"},
   "user":{"$oid":"5bd6de54cb416c6806eb5c42",
   "_id":{"$oid":"5bd6de54cb416c6806eb5c42"},
      "_cls":"User",
      "created":{"$date":1540808276567},
      "modified":{"$date":1540808292678},
      "first_name":"testy",
      "last_name":"Squeeze",
      "mobile_phone_number":"123456789465",
      "addresses":[],
      "billing_address":{
         "alias":"",
         "service":"google"
      },
      "voucher":{
         "initial_quantity":1,
         "expires":{"$date":1572344289089},
         "consumed":1,
         "created":{"$date":1540808276575},
         "uuid":"ed7e9ca2-64c2-49a4-b6c4-f006c07603aa"
      },
      "bio":"",
      "note":"",
      "avatar":"",
      "last_login":{"$date":1540808276583},
      "company":{"$oid":"5bd6de52cb416c6806eb5c41"},
      "tags":[],
      "stripe_customer_id":"cus_DsIPnTkTxM3UPf",
      "auth0_id":"some-id",
      "national_id":"",
      "new_customer":true,
      "is_company":false,
      "default_payment_method":{"$oid":"5bd6de5acb416c6806eb5c45"},
      "deleted":false,
      "receive_notifications":true
   },
   "items":[
      {
         "product":{"$oid":"5bd6de55cb416c6806eb5c44"},
         "quantity":1,
         "discount":0.0,
         "sub_products":[]
      }
   ],
   "units":1,
   "total_quantity":1,
   "total_amount":100.0,
   "top_up_amount":0.0,
   "top_up_vat":0.0,
   "delivery_time":{
      "$date":1572344292591
   },
   "payments":[],
   "currency":"NOK",
   "resources":[],
   "human_id":"JQLVRN",
   "note":"",
   "stripe_charge_id":"",
   "stripe_refund_id":"",
   "order_status":"SUCCESS",
   "delivery_status":"DONE",
   "created":{"$date":1540808292594},
   "modified":{"$date":1540808292682},
   "deliveries":[],
   "override_company_take":-1.0
}
```
