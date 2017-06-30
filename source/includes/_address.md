# Address

## Address Object

Attribute | Type | Description
--------- | ---- | -----
street_name | `string` | The street name of Address
building | `string` | The building of Address
zip_code | `string` | The zip code of Address
city | `string` | The city of Address 
country | `string` | The country of Address 
geo | `array` | The geo position of Address
raw | `object` | All information like address, country, zip code, etc... from service
alias | `string` | The alias of Address
service  | `string` | The service used for Address. _default google_
service_address_id  | `string` | 

### service

Possibilities |
------------- |
google |
pca |

## Retrieves an Address

> Definition

```
GET https://api.sahreactor.io/addresses/lookup
```

> Example request:

``` http
GET /addresses/lookup HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
   "service_address_id":"ChIJR0HKYoduQUYRkho1K1zkZpA",
   "alias":"",
   "country":"Norway",
   "zip_code":"0160",
   "street_name":"Tordenskiolds gate 3",
   "geo":[
      59.9131976,
      10.7361056
   ],
   "raw":{
      "address_components":[
         {
            "types":[
               "street_number"
            ],
            "short_name":"3",
            "long_name":"3"
         },
         {
            "types":[
               "route"
            ],
            "short_name":"Tordenskiolds gate",
            "long_name":"Tordenskiolds gate"
         },
         {
            "types":[
               "political",
               "sublocality",
               "sublocality_level_1"
            ],
            "short_name":"Sentrum",
            "long_name":"Sentrum"
         },
         {
            "types":[
               "postal_town"
            ],
            "short_name":"Oslo",
            "long_name":"Oslo"
         },
         {
            "types":[
               "administrative_area_level_2",
               "political"
            ],
            "short_name":"Oslo kommune",
            "long_name":"Oslo kommune"
         },
         {
            "types":[
               "administrative_area_level_1",
               "political"
            ],
            "short_name":"Oslo",
            "long_name":"Oslo"
         },
         {
            "types":[
               "country",
               "political"
            ],
            "short_name":"NO",
            "long_name":"Norway"
         },
         {
            "types":[
               "postal_code"
            ],
            "short_name":"0160",
            "long_name":"0160"
         }
      ],
      "formatted_address":"Tordenskiolds gate 3, 0160 Oslo, Norway",
      "types":[
         "premise"
      ],
      "geometry":{
         "location_type":"ROOFTOP",
         "location":{
            "lat":59.9131976,
            "lng":10.7361056
         },
         "viewport":{
            "northeast":{
               "lat":59.9145465802915,
               "lng":10.7374546302915
            },
            "southwest":{
               "lat":59.91184861970849,
               "lng":10.7347566697085
            }
         },
         "bounds":{
            "northeast":{
               "lat":59.91333940000001,
               "lng":10.736447
            },
            "southwest":{
               "lat":59.91305579999999,
               "lng":10.7357643
            }
         }
      },
      "place_id":"ChIJR0HKYoduQUYRkho1K1zkZpA"
   },
   "service":"google",
   "city":"Oslo"
}
```

Arguments | Type | Description
--------- | ---- | ------
service | `string` | Chose service you want use
type | `string` | Define the type of your search, if it's street name, zip code, etc
**query** | `string` | Query is what you want search e.q: "Tordenskioldsgate 3, Oslo"