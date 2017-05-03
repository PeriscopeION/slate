---
title: API Reference

language_tabs:
  - curl
  - javascript
  - ruby

toc_footers:
  - <a href="mailto:get@readyion.com">Request an API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Authentication

ION expects for the API key to be included in all API requests to the server in a header that looks like the following:

`x-api-key: your-api-key`

# Rates

## Get All Rates

> To retrieve reservation rates, send the myshopify domain in a post request

```curl
curl -d "{ \"shop\": \"website.myshopify.com\" }" \
-H "Content-Type: application/json" \
-H "x-api-key: your-api-key" \
-v https://api.readyion.com/v1/rates
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

```javascript
var xhr = new XMLHttpRequest();
    xhr.open('POST', 'https://api.readyion.com/v1/rates', true);
    xhr.setRequestHeader('Content-Type', 'application/json');
    xhr.setRequestHeader('x-api-key', 'your-api-key')
    xhr.onreadystatechange = function() {
        if(xhr.readyState == 4 && xhr.status == 200) { 
            var rates = JSON.parse(xhr.responseText);
        }
    };
    xhr.send(JSON.stringify({
        'shop': 'website.myshopify.com'
    }));
```

> Response
  [
    {
      "updated_at":1484750689.6205478,
      "active":"t",
      "created_at":1484750689.6204796,
      "shop":"website.myshopify.com",
      "label":"Covered",
      "daily_max":10.75,
      "description":"Compared to Standard Rate of $11.75",
      "id":"f3ae6c93-8f37-43ae-98ad-69dae25f73da",
    },
    ...
  ]

Retrieve a list of all rates available for reservation by passing in the myshopify domain.

```javascript
[
  {
    'id':'abc-123-456',
    'shop': 'website.myshopify.com',
    'label': 'Covered Parking',
    'active': 't',
    'criteria': '---serialized',
    'criteria_json': {
        '1': {
          'hours': '1',
          'cost': '2.0'
        }
        ...
      },
    'daily_max': '10.25',
    'description': 'Provided Description',
    'created_at': 1489256478.9060683,
    'updated_at': 1489256478.9060683
  }
]
```

# Reservations

## Create a Reservation

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

