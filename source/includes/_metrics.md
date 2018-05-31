# Metrics

This section describes the collection, aggregating and receiving of metrics automatically 
generated on the KVASS platform while processing different tasks.

It is also possible to create custom metrics over the API and receiving these over the API.


## The Metric Object
Attribute | Type | Description
--------- | ---- | -------
name | `string` | The index key of the metric
time_stamp | `object` | The date of the metric as a time stamp
count | `float` | The number of instances of name per time unit
sum_values | `float` | The sum of count per time unit
external | `boolean` | Defines where the metric is created. _Default is `false`_
key | `string` | ID field. Composed by the company's uuid, the metric name and the time stamp

</br></br>
The `name` of the metric usually has a primary key and multiple secondary keys. 
These are separated with a dot, `.`. For example `foo.bar`.
</br>
This is for grouping the metrics based on some commonalities.
An example of this is the metrics aggregated for every API request. The primary key is `requests`,
This key have multiple of secondary keys, like `type` and `status_code`. The metrics for
`requests.type.POST` returns statistics for all `POST` requests. 
`requests.status_code.200` returns statistics for all requests with an HTTP status code 200.


## Get Metrics by Name

This endpoint returns the count, sum of values and an array of metrics for a given name between two dates
and [`resolution`](#metric-resolutions). The example returns the stats for the metric name `foo` for 2 days
with a daily resolution.
It returns the metrics for both `foo.zoo` and `foo.bar`.

> Definition

```
GET https://api.kvass.ai/metrics/<name>?resolution=day
```

> Example request:

``` http
GET /metrics/foo HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{"foo.bar": 
    {"count": 1.0, 
    "values": [0.0, 1.0], 
    "total_sum": 1.0}, 
 "foo.zoo": 
    {"count": 1.0, 
    "values": [2.0, 0.0], 
    "total_sum": 2.0}
 "_meta": 
    {"resolution": "day", 
    "from_date": "2018-04-07T00:00:00+00:00", 
    "to_date": "2018-04-08T23:59:59.99999+00:00"}, 
}

```


Argument | Type | Description
-------- | ---- | -------
from_date | `number` | The date as a time stamp the metrics will be aggregated from. _Default is the day before today_
to_date | `number` | The date as a time stamp the metrics will be aggregated to. _Default is today_
resolution | `string` | One of [`metric resolutions`](#metric-resolutions)
metrics | `stirng` | Filter results by sub-keys under the metric name in the request. See [`under`](#get-metrics-by-multiple-keys)


## Get Metrics Filter by Multiple Keys

It is also possible to filter metrics on multiple sub-keys on a given `name`. 
This is the same endpoint as [`get metrics by name`](#get-metrics-by-name), but with the 
query parameter `metrics`.
 
Getting stats on API usage (requests) is a good example on this. 
The main API request types are `POST`, `GET`, `PUT`, `DELETE` and `OPTIONS`.
If you use `GET /metrics/request` in [`get metrics by name`](#get-metrics-by-name), you will get all
requests, regardless of type. 

If you want to filter by `POST` and `GET` request you could use this method.

> Definition

```
GET https://api.kvass.ai/metrics/<name>?metrics=key,key
```

> Example request:

``` http
GET /metrics/requests.type?metrics=post,get&resolution=day HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{"requests.type.POST": 
    {"count": 14532.0, 
    "values": [7366.0, 7166.0], 
    "total_sum": 14532.0}, 
 "requests.type.GET": 
    {"count": 120000.0, 
    "values": [50000.0, 70000.0], 
    "total_sum": 120000.0}
 "_meta": 
    {"resolution": "day", 
    "from_date": "2018-04-07T00:00:00+00:00", 
    "to_date": "2018-04-08T23:59:59.99999+00:00"}, 
}

```


## Get Metric Count

This endpoint returns the count of instances of a specific metric key between two dates.


> Definition

```
GET https://api.kvass.ai/metrics/<name>/count
```

> Example request:

``` http
GET /metrics/foo.bar/count HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{"count": 456}

```

Argument | Type | Description
-------- | ---- | -------
from_date | `number` | The date as a time stamp the metrics will be aggregated from. _Default is the day before today_
to_date | `number` | The date as a time stamp the metrics will be aggregated to. _Default is today_


## Get Metrics Frequency

This endpoint returns the frequency of a specific metric key between two dates.

> Definition

```
GET https://api.kvass.ai/metrics/<name>/freq
```

> Example request:

``` http
GET /metrics/foo.bar/freq HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{"freq": {"1": 123}}

```

Argument | Type | Description
-------- | ---- | -------
from_date | `number` | The date as a time stamp the metrics will be aggregated from. _Default is the day before today_
to_date | `number` | The date as a time stamp the metrics will be aggregated to. _Default is today_


## Get Metrics Total Sum

This endpoint returns the total sum of a specific metric key between two dates.

> Definition

```
GET https://api.kvass.ai/metrics/<name>/sum
```

> Example request:

``` http
GET /metrics/foo.bar/sum HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{"sum": 123}

```

Argument | Type | Description
-------- | ---- | -------
from_date | `number` | The date as a time stamp the metrics will be aggregated from. _Default is the day before today_
to_date | `number` | The date as a time stamp the metrics will be aggregated to. _Default is today_


## Create External Metrics

This endpoint lets you create custom metrics and store them in the database. 

You can receive these custom metrics with either of the `GET /metrics` methods listed over.
Metrics created over this endpoint will have the flag `external=true`.

> Definition

```
POST https://api.kvass.ai/metrics
```

> Example request:

``` http
POST /metrics HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io

{
    "name": "foo.bar",
    "value": 1000,
    "time" 1523297656
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "company": {"$oid": "5acbad570190d9573d512d11"}, 
    "count": 1.0,  
    "values": {}, 
    "external": true, 
    "sum_values": 1000.0, 
    "_id": "d721daa2-46be-4685-8fea-b6a17b05fe88:foo.bar:1523297656:ext", 
    "time_stamp": {"$date": 1523297656}, 
    "name": "foo.bar"
}
```

Argument | Type | Description
-------- | ---- | -------
**name** | `string` | The metric key on which your metrics are grouped on. 
value | `number` | The value of the metric instance. _Default is 1_
time | `number` | The timestamp of the event the metric happened. _Default is now_


## Get List of all Metric Keys

This endpoint returns an array with all saved instances of the metric keys.


> Definition

```
GET https://api.kvass.ai/metrics
```

> Example request:

``` http
GET /metrics HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io

```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    "foo.bar", 
    "foo.zoo", 
    "foo.woo", 
    "requests", 
    "requests.type.GET", 
    "requests.route./metrics", 
    "requests.path./metrics",
    "..."
]

```

</n> </n>   
<aside class="notice">This endpoint could return an array of _considerable_ length. Only use this in dev process for checking available metric keys.</aside>


## Metric Resolutions

The metrics could be given in these resolutions for a given metric name.

Resolution | Argument | Description
------- | -------- | --------
Daily | `day` | Daily metrics _Default_
Hourly | `hour` | Hourly metrics 
Minutely | `minute` |Minutely metrics
