# Data Science Labs

The lab is where the magic happens. This is the integration endpoints to the Kvass data science platform.


## Check Server Health 

Checks whether or not the Kvass Data Science platform is running.

Returns `{'status': 'UP'}` if server is running and `{'status': 'DOWN'}` if not.


> Definition

```
GET https://api.kvass.ai/labs/health
```

> Example request:

``` http
GET /labs/health HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "status": "UP"
}
```


## Train Recommendation Model

> Definition

```
POST https://api.kvass.ai/labs/recommendations/train
```

> Example request:

``` http
POST /labs/recommendations/train HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
   "model_type": "content_recommender-user-product"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

This endpoint trains a specific recommendation engine model based on the [`model type`](#list-of-model-types) in the request. 


Attribute | Type | Description
---------- | --- | -------
**model_type** | `string` | The model type which you want trained _Required field_

</n> </n>   
<aside class="notice">This is necessary to do before getting recommendations and similarities.</aside>


## Recommendation Labs

> Definition

```
POST https://api.kvass.ai/labs/recommendations/recommend
```

> Example request:

``` http
POST /labs/recommendations/recommend HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
   "model_type": "content_recommender-user-product",
   "size": 3,
   "query": "<id>"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "product": "Product 1",
        "query": "<id from request>",
        "rank": 1
    },
        {
        "product": "Product 2",
        "query": "<id from request>",
        "rank": 2
    },
    {
        "product": "Product 3",
        "query": "<id from request>",
        "rank": 3
    }
]
```

Get recommendations based on model type in the request. The recommendations are ranked in the array
by the field `rank`. The higher the rank, the higher the subjects recommendation. 




Attribute | Type | Description
---------- | --- | -------
**model_type** | `string` | The [`model type`](#list-of-model-types) _Required field_
**query** | `string` | The ID of the queried subject, to which you want recommendations for _Required field_
size | `number` | The number of recommendations in the results _Default is 3_

  
## Similarity Labs

> Definition

```
POST https://api.kvass.ai/labs/recommendations/similar
```

> Example request:

``` http
POST /labs/recommendations/similar HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
   "model_type": "content_similarity_graph-product-product",
   "number_of_recommendations": 3,
   "features": ["tags"]
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "product": "Product 1",
        "distance": 0.98,
        "rank": 1
    },
        {
        "product": "Product 2",
        "distance": 0.95,
        "rank": 2
    },
    {
        "product": "Product 3",
        "distance": 0.91,
        "rank": 3
    }
]
```

Get similarities and the degree of similarity for a given subject, like a [`product`](#products). 
The results in the array are ranked after the field `rank`. 
The `distance` is the degree of similarity to the initial subject. 

Attribute | Type | Description
---------- | --- | -------
**model_type** | `string` | The [`model type`](#list-of-model-types) _Required field_
**features** | `string` | An array of features _Required field_
**query** | `string` | The ID of the queried subject, to which you want similarity to _Required field_
size | `number` | The number of recommendations in the results _Default is 3_

  

## List of Model Types

Model name | Description
---------- | ------
`content_recommender-user-product` | Product recommendations for a user
`content_similarity_graph-product-product` | Similar products for a product
