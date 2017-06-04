---
title: API Reference

language_tabs:
  - shell
  - ruby

toc_footers:
  - <a href="mailto:get@readyion.com">Request an API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Making Requests

ION expects for the API key to be included in all API requests to the server in a header that looks like the following:

`x-api-key: your-api-key`

### Endpoints

For the test environment (ion-test.myshopify.com):

`api-dev.readyion.com`

For production stores with the ION app installed:

`api.readyion.com`


# Locations

## Get All Available Shop Locations

> To retrieve reservation rates, send the myshopify domain in a post request

```shell
curl -H "Content-Type: application/json" \
-H "x-api-key: your-api-key" \
https://api.readyion.com/v1/locations
```

```ruby
uri = URI.parse("https://api.readyion.com/v1/locations");
https = Net::HTTP.new(uri.host,uri.port)
https.use_ssl = true
https.verify_mode = OpenSSL::SSL::VERIFY_NONE
request = Net::HTTP::Get.new(uri.request_uri)
request["x-api-key"] = your-api-key
request["Content-Type"] = "application/json"
response = https.request(request)
return JSON.parse(response.body)
```

> Response

```json
{
  "count": 1,
  "locations": [
    {
      "shopify_domain": "website.myshopify.com",
      "updated_at": 1496419687.7165,
      "created_at": 1496419687.7165,
      "shop": {
        "address": "123 street drive",
        "city": "Stamford",
        "state": "Connecticut",
        "latitude": 123,
        "longitude": 456
      }
  ]
}
```

Retrieve a list of all locations available for reservation.

### HTTP Request

`GET https://api.readyion.com/v1/locations`

# Rates

## Get All Rates

> To retrieve reservation rates, send the myshopify domain in a post request

```shell
curl -d "{ \"shop\": \"website.myshopify.com\" }" \
-H "Content-Type: application/json" \
-H "x-api-key: your-api-key" \
https://api.readyion.com/v1/rates
```

```ruby
body = {
  "shop" => "website.myshopify.com"
}.to_json
uri = URI.parse("https://api.readyion.com/v1/rates");
https = Net::HTTP.new(uri.host,uri.port)
https.use_ssl = true
https.verify_mode = OpenSSL::SSL::VERIFY_NONE
request = Net::HTTP::Post.new(uri.request_uri)
request["x-api-key"] = your-api-key
request["Content-Type"] = "application/json"
request.body = body
response = https.request(request)
return JSON.parse(response.body)
```

> Response

```json
  [
    {
      "shop":"website.myshopify.com",
      "label":"Covered",
      "daily_max":10.75,
      "description":"Compared to Standard Rate of $11.75",
      "id":"f3ae6c93-8f37-43ae-98ad-69dae25f73da",
    },
    ...
  ]
```

Retrieve a list of all rates available for reservation by passing in the myshopify domain.

### HTTP Request

`POST https://api.readyion.com/v1/rates`

### Request JSON Body

Property | Required | Description
--------- | ------- | -----------
shop | true | The myshopify domain i.e. mywebsite.myshopify.com

# Reservations

## Calculate Price

> To calculate the cost of a reservation, send the arrival and departure dates as ISO compatible date strings, a rate ID, and a myshopify domain

```shell
curl -d \
"{ \"shop\": \"mywebsite.myshopify.com\", \"rate\": \"unique-rate-id\", \"from\": \"2018-05-08T13:00:00.000Z\", \"to\":\"2018-05-09T13:00:00.000Z\" }" \
-H "Content-Type: application/json" \
-H "x-api-key: your-api-key" \
https://api.readyion.com/v1/calculate
```

```ruby
body = {
  "shop" => "website.myshopify.com"
}.to_json
uri = URI.parse("https://api.readyion.com/v1/calculate");
https = Net::HTTP.new(uri.host,uri.port)
https.use_ssl = true
https.verify_mode = OpenSSL::SSL::VERIFY_NONE
request = Net::HTTP::Post.new(uri.request_uri)
request["x-api-key"] = your-api-key
request["Content-Type"] = "application/json"
request.body = {
  "shop": "mywebsite.myshopify.com",
  "rate": "unique-rate-id",
  "from": "2018-05-08T13:00:00.000Z",
  "to": "2018-05-09T13:00:00.000Z"
}
response = https.request(request)
return JSON.parse(response.body)
```

> Response

```json
  {
    "message": "Total cost is $20.5 for 1 day and 5 additional hours.",
    "cost": 20.5,
    "days": 1,
    "hours":29
  }
```

Calculates the cost of a reservation based on an arrival (from) and departure (to) and a rate

### HTTP Request

`POST https://api.readyion.com/v1/calculate`

### Request JSON Body

Property | Required | Description
--------- | ------- | -----------
shop | true | The myshopify domain i.e. mywebsite.myshopify.com
rate | true | Unique Rate ID
from | true | ISO compatible date string representing the arrival date
to | true | ISO compatible date string representing the departure date

## Create a Reservation

Create a reservation will return a Shopify Product and Checkout (with web_url) that you can use to checkout with.

### HTTP Request

`POST https://api.readyion.com/v1/create`

### Request JSON Body

Property | Required | Description
--------- | ------- | -----------
shop | true | The myshopify domain i.e. mywebsite.myshopify.com
rate | true | The rate ID
from | true | ISO compliant date string for check in date/time
to | true | ISO compliant date string for check out date/time

```ruby
TBD
```

```shell
curl -d \
"{ \"shop\": \"mywebsite.myshopify.com\", \"rate\": \"unique-rate-id\", \"from\": \"2018-05-08T13:00:00.000Z\", \"to\":\"2018-05-09T13:00:00.000Z\" }" \
-H "Content-Type: application/json" \
-H "x-api-key: your-api-key" \
https://api.readyion.com/v1/create
```

> Reponse

```json
{
   "product":{
      "id":10410717459,
      "title":"2018-05-09T13:00:00.000Z",
      "body_html":"Prepaid Reservation (Created via ION API) <br> <strong>Check-in: </strong> 2018-05-08T13:00:00.000Z <br> <strong>Check-out: </strong> 2018-05-09T13:00:00.000Z",
      "vendor":"prepaid-dev",
      "product_type":"reservation",
      "created_at":"2017-05-03T13:31:09-04:00",
      "handle":"2018-05-09t13-00-00-000z-2",
      "updated_at":"2017-05-03T13:31:10-04:00",
      "published_at":"2017-05-03T13:31:09-04:00",
      "template_suffix":null,
      "published_scope":"global",
      "tags":"",
      "variants":[
         {
            "id":42208460307,
            "product_id":10410717459,
            "title":"Default Title",
            "price":"10.25",
            "sku":"sM0KeIRDvXFgGXp",
            "position":1,
            "grams":0,
            "inventory_policy":"deny",
            "compare_at_price":null,
            "fulfillment_service":"manual",
            "inventory_management":null,
            "option1":"Default Title",
            "option2":null,
            "option3":null,
            "created_at":"2017-05-03T13:31:09-04:00",
            "updated_at":"2017-05-03T13:31:09-04:00",
            "taxable":true,
            "barcode":"https://chart.googleapis.com/chart?chs=200x200&cht=qr&chl=sM0KeIRDvXFgGXp&chld=L|1&choe=UTF-8",
            "image_id":null,
            "inventory_quantity":1,
            "weight":0,
            "weight_unit":"lb",
            "old_inventory_quantity":1,
            "requires_shipping":false
         }
      ],
      "options":[
         {
            "id":12559559827,
            "product_id":10410717459,
            "name":"Title",
            "position":1,
            "values":[
               "Default Title"
            ]
         }
      ],
      "images":[
         {
            "id":26685610195,
            "product_id":10410717459,
            "position":1,
            "created_at":"2017-05-03T13:31:09-04:00",
            "updated_at":"2017-05-03T13:31:09-04:00",
            "src":"https://cdn.shopify.com/s/files/1/1748/3161/products/chart_b065072f-ead3-4b71-84bc-f0b06e3ebc0a.png?v=1493832669",
            "variant_ids":[

            ]
         }
      ],
      "image":{
         "id":26685610195,
         "product_id":10410717459,
         "position":1,
         "created_at":"2017-05-03T13:31:09-04:00",
         "updated_at":"2017-05-03T13:31:09-04:00",
         "src":"https://cdn.shopify.com/s/files/1/1748/3161/products/chart_b065072f-ead3-4b71-84bc-f0b06e3ebc0a.png?v=1493832669",
         "variant_ids":[

         ]
      }
   },
   "checkout": { 
     "completed_at": null,
     "created_at": '2017-06-02T12:20:03-04:00',
     "currency": 'USD',
     "customer_id": null,
     "device_id": null,
     "discount_code": null,
     "email": null,
     "location_id": null,
     "name": '#17312084749',
     "note": '',
     "note_attributes": {},
     "order_id": null,
     "order_status_url": null,
     "order": null,
     "payment_due": '594.50',
     "payment_url": 'https://elb.deposit.shopifycs.com/sessions',
     "payments": [],
     "phone": null,
     "shopify_payments_account_id": null,
     "privacy_policy_url": 'https://checkout.shopify.com/18485575/policies/31549261.html',
     "refund_policy_url": 'https://checkout.shopify.com/18485575/policies/31549197.html',
     "requires_shipping": false,
     "reservation_time_left": 300,
     "reservation_time": 300,
     "source_identifier": null,
     "source_name": '1577999',
     "source_url": null,
     "subtotal_price": '594.50',
     "tax_exempt": false,
     "taxes_included": false,
     "terms_of_service_url": 'https://checkout.shopify.com/18485575/policies/31549325.html',
     "token": '501f55aa17287c3af5d91521401383fe',
     "total_price": '594.50',
     "total_tax": '0.00',
     "updated_at": '2017-06-02T12:20:03-04:00',
     "user_id": null,
     "web_url": 'https://checkout.shopify.com/18485575/checkouts/501f55aa17287c3af5d91521401383fe',
     "web_processing_url": 'https://checkout.shopify.com/18485575/checkouts/501f55aa17287c3af5d91521401383fe/processing',
     "line_items": [ [Object] ],
     "gift_cards": [],
     "tax_lines": [],
     "tax_manipulations": [],
     "shipping_line": null,
     "shipping_rate": null,
     "shipping_address": null,
     "credit_card": null,
     "billing_address": null,
     "applied_discount": null 
   }
}
```