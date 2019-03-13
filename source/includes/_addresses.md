# Addresses

Address is the class which structures all addresses used by [`User`](@users), [`Provider`](@providers) 
and the delivery system (pickup and drop off).

## Address Object

Attribute | Type | Description
--------- | ---- | -----
street_name | `string` | The street name of Address
building | `string` | The building number of Address
zip_code | `string` | The zip code of Address
city | `string` | The city of Address 
country | `string` | The country of Address 
geo | `array` | The geo position of Address
raw | `object` | All information like address, country, zip code, etc... from service
alias | `string` | The alias of Address
organization_name | `string` | The name of the organization at the given address if applicable 
service  | `string` | The service used for Address _default google_

### Services

We currently support localization with Google.

## Retrieving an Address

> Definition

```
GET https://api.kvass.ai/addresses/lookup
```

> Example request:

``` http
GET /addresses/lookup?service=google&type=streetname&query=Karl%20Johans%20gate%2022 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "alias": "",
    "country": "Norway",
    "service": "google",
    "street_name": "Karl Johans gate 22",
    "city": "Oslo",
    "geo": [
        59.9130629,
        10.7402665
    ],
    "service_address_id": "ChIJ7ZkfWX1uQUYRLjlo7CGVLFo",
    "raw": {
        "geometry": {
            "viewport": {
                "northeast": {
                    "lat": 59.9143406802915,
                    "lng": 10.7415464302915
                },
                "southwest": {
                    "lat": 59.91164271970849,
                    "lng": 10.7388484697085
                }
            },
            "location": {
                "lat": 59.9130629,
                "lng": 10.7402665
            },
            "bounds": {
                "northeast": {
                    "lat": 59.9134037,
                    "lng": 10.7410763
                },
                "southwest": {
                    "lat": 59.91257969999999,
                    "lng": 10.7393186
                }
            },
            "location_type": "ROOFTOP"
        },
        "place_id": "ChIJ7ZkfWX1uQUYRLjlo7CGVLFo",
        "formatted_address": "Stortingsbygningen, Karl Johans gate 22, 0026 Oslo, Norway",
        "types": [
            "premise"
        ],
        "address_components": [
            {
                "long_name": "Stortingsbygningen",
                "short_name": "Stortingsbygningen",
                "types": [
                    "premise"
                ]
            },
            {
                "long_name": "22",
                "short_name": "22",
                "types": [
                    "street_number"
                ]
            },
            {
                "long_name": "Karl Johans gate",
                "short_name": "Karl Johans gate",
                "types": [
                    "route"
                ]
            },
            {
                "long_name": "Sentrum",
                "short_name": "Sentrum",
                "types": [
                    "political",
                    "sublocality",
                    "sublocality_level_1"
                ]
            },
            {
                "long_name": "Oslo",
                "short_name": "Oslo",
                "types": [
                    "postal_town"
                ]
            },
            {
                "long_name": "Oslo kommune",
                "short_name": "Oslo kommune",
                "types": [
                    "administrative_area_level_2",
                    "political"
                ]
            },
            {
                "long_name": "Oslo",
                "short_name": "Oslo",
                "types": [
                    "administrative_area_level_1",
                    "political"
                ]
            },
            {
                "long_name": "Norway",
                "short_name": "NO",
                "types": [
                    "country",
                    "political"
                ]
            },
            {
                "long_name": "0026",
                "short_name": "0026",
                "types": [
                    "postal_code"
                ]
            }
        ]
    },
    "zip_code": "0026"
}
```

Arguments | Type | Description
--------- | ---- | ------
**service** | `string` | Chose the service you want to use
**query** | `string` | Query is what you want to search for e.g.: "Tordenskioldsgate 3, Oslo"
type | `string` | Define your search parameter, if it's street name, zip code, etc
