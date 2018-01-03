---
title: API Reference

toc_footers:
  - <a href='/info/api.html'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Authentication
The API authentication is done over HTTPS. Each request is authenticated using a private key contained within the URL. A request will be rejected if it is not requested over HTTPS or the private key is invalid.

The private key can be viewed at <a href="/account/api.html">API Admin</a>.

# Info

## Legal

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/info/legal.json?platform=web
```

> Example Response

```json
{
	"info": "<div><h1>Our terms and conditions</h1></div>"
}
```

Returns WillyWeather's terms and conditions.

### Request

`GET api.willyweather.com.au/v2/{api key}/info/legal.json`

Parameter | Type | Options | Required
--------- | ---- | ------- | --------
platform | string | `web`, `mobile` | true

## Info By Location Id

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/{location id}/info.json?platform=iphone&weatherTypes=weather&mapTypes=radar-rainfall&forecastGraphTypes=wind&observationalGraphTypes=temperature&graphKeyTypes=wind
```

> Example Response

```json
{
    "weatherTypes": {
        "weather": "<div class=\"weather-info\">Info content goes here</div>"
    },
    "mapTypes": {
        "radar-rainfall": "<div class=\"radar-info\">Info content goes here</div>"
    },
    "forecastGraphTypes": {
        "wind": "<div>Info content goes here</div>"  
    },
    "observationalGraphTypes": {
        "temperature": "<div>Info content goes here</div>"
    },
    "graphKeyTypes": {
        "wind": "<div>Info content goes here</div>"
    }
}
```

Returns WillyWeather's help and info.

### Request

`GET api.willyweather.com.au/v2/{api key}/locations/{location id}/info.json`

Parameter | Type | Options | Required
--------- | ---- | ------- | --------
platform | string | `iphone`, `android` | true
weatherTypes | csv | `weather`, `wind`, `moonphases`, `rainfall`, `swell`, `tides`, `sunrisesunset`,` uv` | option
mapTypes | csv | `radar-rainfall`, `satellite`, `synoptic`, `rainfall`, `temperature`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `cyclone` | option
forecastGraphTypes | csv | `precis`, `rainfallprobability`, `sunrisesunset`, `swell-period`, `swell-height`, `swell-period`, `temperature`, `tides`, `wind`, `uv` | option
observationalGraphTypes | csv | `temperature`, `dew-point`, `pressure`, `rainfall`, `wind` | option
graphKeyTypes | csv | `wind`, `swell-height` | option
units | csv | <a href="#units">Units</a> | false

<aside class="notice">
Either <code>weatherTypes</code>, <code>mapTypes</code>, <code>forecastGraphTypes</code> or <code>observationalGraphTypes</code> or <code>graphKeyTypes</code> is required.
</aside>

# Locations

## Get Location

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/{location id}.json
```

> Example Response

```json
{
    "id": 2212,
    "name": "Collaroy Beach",
    "region": "Sydney",
    "state": "NSW",
    "postcode": "2097",
    "timeZone": "Australia/Sydney",
    "lat": -33.73212,
    "lng": 151.30123,
    "typeId": 2
}
```

Returns the details of a single location.

### Request

`GET /api/v2/locations/{location id}.json`

Parameter | Type | Options | Required
--------- | ---- | ------- | --------
id | int |  | true 

<aside class="notice">
Note - get a Location <code>id</code> using the <a href="#search">Search</a> endpoint.
</aside>

### Response

Response is a Location.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
region | string | | the name of the region that the location belongs to
state | string | | the name of the state that the location belongs to
postcode | string | |
timeZone | string | <a href="http://php.net/manual/en/timezones.php">php timezone</a> | (e.g. Sydney/Australia)
lat | double | | the exact coordinates of a location (note, this may differ from popular map services)
lng | double | | the exact coordinates of a location (note, this may differ from popular map services)
typeId | int | | **(see LocationType Ids)**

### Location Type Ids

Location `typeId` is used to determine what weather data is applicable to a location. All that needs to be considered is whether a location has tides forecasts, swell forecast or both.

Weather Type | Type Ids
------------ | --------
weather | all
wind | all
rainfall | all
sunrisesunset | all
moonphases | all
uv | all
tides | 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 19
swell | 2, 5, 6, 7, 8, 9, 10, 11, 12, 13, 24, 25

# Maps

## Get Map Providers

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/maps.json?mapTypes=regional-radar&lat=-25.97&lng=133.91&verbose=true&offset=-60&limit=30
```

> Example Response

```json
[
	{
		"id": 71,
		"name": "Sydney (Terrey Hills)",
		"lat": -33.701,
		"lng": 151.21,
		"bounds": {
			"minLat": -36.051,
			"minLng": 148.51,
			"maxLat": -31.351,
			"maxLng": 153.91
		},
		"typeId": 1,
		"zoom": 256000,
		"radius": 256000,
		"interval": 6,
		"overlayPath": "//cdn2.willyweather.com.au/radar/",
		"overlays": [
			{
				"dateTime": "2016-03-26 22:42:00",
				"name": "71-201403262242.png"
			}
		],
		"classification": "radar",
		"mapLegend": {
        	"keys": [
        		{
        			"colour": "#f5f5ff",
        			"range": {
        				"min": 0.2,
        				"max": 0.5
        			},
        			"label": "light"
        		},
        		{
        			"colour": "#b4b4ff",
        			"range": {
        				"min": 0.5,
        				"max": 1.5
        			},
        			"label": ""
        		}
        	]
        }
	}
]
```

Returns all map providers that can be filtered by `mapType`.

### Request

`GET api.willyweather.com.au/v2/{api key}/maps.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapTypes | csv | `regional-radar`, `radar`, `satellite`, `synoptic`, `temperature`, `wind`, `rainfall`, `swell`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `thunderstorms`, `lightning`, `fog`, `frost`, `mixing-height`, `drought-factor`, `cyclone` | **see Map Types for conversion of typeId** | true
lat | double | | latitude | false
lng | double | | longitude | false
verbose | boolean | | include overlay images with the response | false
offset | int | | minutes that overlay images should start from | true
limit | int | | minutes that overlay images should end at | false

<aside class="notice">
<code>offset</code> is required if <code>verbose</code> is <code>true</code>, which is the default if not included in the request.
</aside>

<aside class="notice">
When <code>lat</code> and <code>lng</code> are provided the returned Map Providers will be the closest ones to these coordinates, that also support the specified <code>mapTypes</code>.
</aside>

### Response 

Response is an array of Map Providers.

### Map Provider

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | | 
name | string | |
lat | double | | latitude
lng | double | | longitude
bounds | object | | **(see Bounds)**
typeId | int | | **(see MapTypes)**
zoom | int | | when using google maps apis, this is the zoom height
radius | int | | the radius of data that the provider offers in km (e.g. radar can be 512)
interval | int | | time in minutes between each image
overlayPath | string | | the root directory path for overlay images
overlays | array | | an array of overlay objects **(see Overlay)**
classification | string | | the type of map provider (e.g. radar, satellite, synoptic)

### Bounds

The bounding box coordinates for the overlay images, `min` being the bottom left corner, `max` being the top right corner.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
minLat | double | |
minLng | double | |
maxLat | double | |
maxLng | double | |

### Overlay

The path of the map image on our servers.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
name | string | | path of image, to be appended to overlayPath in main response

### MapLegend

The legend describing the values of the data.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
keys | array | | an array of key objects **(see Key)**

### Key

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
colour | string | | hexadecimal colour code
range | object | | **(see Range)**
label | string | | 

### Range

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
min | float | | min value of the range
max | float | | max value of the range
 

### Map Types
* regional rain radar : 1
* national rain radar : 3
* satellite : 4
* synoptic : 100
* temperature : 200
* wind : 300
* rain : 400
* swell : 500
* uv : 600
* apparent-temperature : 700
* dew-point : 800
* relative-humidity : 900
* cloud-cover : 1000
* thunderstorms : 1100
* lightning : 1200
* fog : 1300
* frost : 1400
* mixing-height : 1500
* drought-factor : 1600
* cyclone : 7600

### Map Classifications
* radar
* satellite
* synoptic
* temperature
* wind
* rain
* swell
* uv
* apparent-temperature
* dew-point
* relative-humidity
* cloud-cover
* thunderstorms
* lightning
* fog
* frost
* mixing-height
* drought-factor
* cyclone

## Map Data By Provider

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/maps/71.json?offset=-60&limit=30
```

> Example Response

```json
{
	"id": 71,
	"name": "Sydney (Terrey Hills)",
	"lat": -33.701,
	"lng": 151.21,
	"bounds": {
		"minLat": -36.051,
		"minLng": 148.51,
		"maxLat": -31.351,
		"maxLng": 153.91
	},
	"typeId": 1,
	"zoom": 256000,
	"radius": 256000,
	"interval": 6,
	"overlayPath": "//cdn2.willyweather.com.au/radar/",
	"overlays": [
		{
			"dateTime": "2014-03-26 22:30:00",
			"name": "71-201403262230.png"
		},
		{
			"dateTime": "2014-03-26 22:36:00",
			"name": "71-201403262236.png"
		},
		{
			"dateTime": "2014-03-26 22:42:00",
			"name": "71-201403262242.png"
		}
	],
	"classification": "radar",
	"mapLegend": {
        "keys": [
            {
                "colour": "#f5f5ff",
                "range": {
                    "min": 0.2,
                    "max": 0.5
                },
                "label": "light"
            },
            {
                "colour": "#b4b4ff",
                "range": {
                    "min": 0.5,
                    "max": 1.5
                },
                "label": ""
            }
        ]
    }
}
```

Get a map provider with overlay data.

### Request

`GET api.willyweather.com.au/v2/{api key}/maps/{provider id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapTypes | csv | `regional-radar`, `radar`, `satellite`, `synoptic`, `temperature`, `wind`, `rainfall`, `swell`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `thunderstorms`, `lightning`, `fog`, `frost`, `mixing-height`, `drought-factor`, `cyclone` | **<a href="/#get-map-providers">Get Map Providers</a>** | true
offset | int | | minutes that overlay images should start from | true
limit | int | | minutes that overlay images should end at | false

### Response

Response is a Map Provider. See <a href="#get-map-providers">Get Map Providers</a> for description of a Map Provider response.

## Map Data By Location Id

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/maps.json?mapTypes=regional-radar&offset=-60&limit=30
```

> Example Response

```json

[
	{
		"id": 71,
		"name": "Sydney (Terrey Hills)",
		"lat": -33.701,
		"lng": 151.21,
		"bounds": {
			"minLat": -36.051,
			"minLng": 148.51,
			"maxLat": -31.351,
			"maxLng": 153.91
		},
		"typeId": 1,
		"zoom": 256000,
		"radius": 256000, 
		"interval": 6,
		"overlayPath": "\/\/cdn2.willyweather.com.au\/radar\/",
		"overlays": [
			{
				"dateTime": "2014-03-26 22:30:00",
				"name": "71-201403262230.png"
			},
			{
				"dateTime": "2014-03-26 22:36:00",
				"name": "71-201403262236.png"
			},
			{
				"dateTime": "2014-03-26 22:42:00",
				"name": "71-201403262242.png"
			}
		],
		"classification": "radar",
        "mapLegend": {
        	"keys": [
        		{
        			"colour": "#f5f5ff",
        			"range": {
        				"min": 0.2,
        				"max": 0.5
        			},
        			"label": "light"
        		},
        		{
        			"colour": "#b4b4ff",
        			"range": {
        				"min": 0.5,
        				"max": 1.5
        			},
        			"label": ""
        		}
        	]
        }		
	}
]
```

Get map providers linked to a location.

### Request

`GET api.willyweather.com.au/v2/locations/{location id}/maps.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapTypes | csv | `regional-radar`, `radar`, `satellite`, `synoptic`, `temperature`, `wind`, `rainfall`, `swell`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `thunderstorms`, `lightning`, `fog`, `frost`, `mixing-height`, `drought-factor`, `cyclone` | **<a href="/#get-map-providers">Get Map Providers</a>** | true
offset | int | | minutes that overlay images should start from | true
limit | int | | minutes that overlay images should end at | false

### Response

Response is an array of Map Providers. See <a href="#get-map-providers">Get Map Providers</a> for a description of a Map Provider response.

# Regions

## Get Region

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/regions/{region id}.json
```

> Example Response

```json
{
    "id": 52,
    "name": "Adelaide",
    "state": "SA",
    "timeZone": "Australia/Adelaide",
    "typeId": 1
}
```

Returns the details of a single Region.

### Request
`GET api.willyweather.com.au/api/v2/regions/{region id}.json`

### Response
Response is a Region.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
state | string | | the name of the state that the region belongs to
timeZone | string | <a href="http://php.net/manual/en/timezones.php">php timezone</a> | (e.g. Sydney/Australia)
typeId | int | | **(see RegionType Ids)**

### Region Type Ids

Region `typeId` is used to determine what weather data is applicable to a region. All that needs to be considered is whether a region has tides forecasts, swell forecast or both.

Weather Type | Type Ids
------------ | --------
weather | all
wind | all
rainfall | all
sunrisesunset | all
moonphases | all
uv | all
tides | 1, 3
swell | 1


## Get Regions

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/regions.json
```

> Example Response

```json
[
    {
        "id": 52,
        "name": "Adelaide",
        "state": "SA",
        "timeZone": "Australia/Adelaide",
        "typeId": 1
    }
]
```

Returns the details of all Regions.

### Request
`GET api.willyweather.com.au/api/v2/regions.json`

### Response
Response is an array of Regions. See <a href="#get-region">Get Region</a> for a description of a Region response.

## Get Regions by State

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/states/{state id}/regions.json
```

> Example Response

```json
[
    {
        "id": 52,
        "name": "Adelaide",
        "state": "SA",
        "timeZone": "Australia/Adelaide",
        "typeId": 1
    }
]
```

Returns all Regions within a State.

### Request
`GET api.willyweather.com.au/api/v2/states/{state id}/regions.json`

### Response
Response is an array of Regions. See <a href="#get-region">Get Region</a> for a description of a Region response.

# Search

## Location Search By Query

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/search.json?query=beach&limit=2
```

> Example Response

```json
[
	{
		"id": 2212,
		"name": "Collaroy Beach",
		"region": "Sydney",
		"state": "NSW",
		"postcode": "2097",
		"timeZone": "Australia/Sydney",
		"lat": -33.73212,
		"lng": 151.30123,
		"typeId": 2
	},
	{
		"id": 1353,
		"name": "Callala Beach",
		"region": "Illawarra",
		"state": "NSW",
		"postcode": "2540",
		"timeZone": "Australia/Sydney",
		"lat": -35.00908,
		"lng": 150.69652,
		"typeId": 14
	}
]
```

Get a list of Locations that have a name or post code matching the query.

### Request

`GET api.willyweather.com.au/v2/{api key}/search.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
query | string | | location name or postcode | true
limit | int | `1` - `50` | limit the number of locations in response (default 25) | false

### Response

Response is an array of Locations. See <a href="#locations">Locations</a> for description of a Location response.

## Location Search By Coordinates

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/search.json?lat=-33.89&lng=151.27&units=distance:km
```

> Example Response

```json
{
	"location":
		{
			"id": 19169,
			"name": "Mackenzies Bay",
			"region": "Sydney",
			"state": "NSW",
			"postcode": "0",
			"timeZone": "Australia/Sydney",
			"lat": -33.8996141,
			"lng": 151.272962,
			"typeId": 12,
			"distance": 1
		}
	,
	"units": {
		"distance": "km"
	}
}
```

Get the closest Location to a set of coordinates.

### Request

`GET api.willyweather.com.au/v2/{api key}/search.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
lat | double | | latitude | true
lng | double | | longitude | true
units | csv | See <a href="/#units">Units</a>. Only distance can be specified | | true

### Response

A single Location and units. See <a href="#locations">Location</a> for a description of a Location response. See <a href="#units">Units</a> for a description of a Units response.

## Closest Locations

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/search/closest.json?id=4988&weatherTypes=swell,tides,general&units=distance:km
```

> Example Response

```json
{
	"swell": [
		{
			"id": 19169,
			"name": "Mackenzies Bay",
			"region": "Sydney",
			"state": "NSW",
			"postcode": "0",
			"timeZone": "Australia/Sydney",
			"lat": -33.8996141,
			"lng": 151.272962,
			"typeId": 12,
			"distance": 1
		}
	],
	"tides": [
		{
			"id": 19169,
			"name": "Mackenzies Bay",
			"region": "Sydney",
			"state": "NSW",
			"postcode": "0",
			"timeZone": "Australia/Sydney",
			"lat": -33.8996141,
			"lng": 151.272962,
			"typeId": 12,
			"distance": 1
		}
	],
	"general": [
		{
			"id": 5011,
			"name": "North Bondi",
			"region": "Sydney",
			"state": "NSW",
			"postcode": "2026",
			"timeZone": "Australia/Sydney",
			"lat": -33.8857,
			"lng": 151.27979,
			"typeId": 1,
			"distance": 0.7
		}
	],
	"units": {
		"distance": "km"
	}
}
```

Get a list of closest locations surrounding another location, filtered by weatherType

### Request

`GET api.willyweather.com.au/v2/{api key}/search/closest.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | string | | location id | true
units | csv | See <a href="#units">Units</a>. Only distance can be specified | | true
weatherTypes | csv | general, tides, swell | | true

### Response

An array of location objects, broken up into their weather types. See <a href="#locations">Locations</a> for a description of a Location response.

# States

## Get State

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/states/{state id}.json
```

> Example Response

```json
{
    "id": 4,
    "name": "Australian Capital Territory",
    "abbreviation": "ACT",
    "typeId": 1
}
```

Returns the details of a single State.

### Request
`GET api.willyweather.com.au/api/v2/state/{state id}.json`

### Response
Response is a State.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
abbreviation | string | |
typeId | int | | **(see State Type Ids)**

### State Type Ids

State `typeId` is used to determine what weather data is applicable to a state. All that needs to be considered is whether a region has tides forecasts, swell forecast or both.

Weather Type | Type Ids
------------ | --------
weather | all
wind | all
rainfall | all
sunrisesunset | all
moonphases | all
uv | all
tides | 1, 3
swell | 1

## Get States

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/states.json
```

> Example Response

```json
[
    {
        "id": 4,
        "name": "Australian Capital Territory",
        "abbreviation": "ACT",
        "typeId": 1
    }
]
```

Returns the details of all States.

### Request
`GET api.willyweather.com.au/api/v2/states.json`

### Response
Response is an array of States. See <a href="#get-state">Get State</a> for description of a State response.

# Warnings

## All Warnings

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/warnings.json?classifications=storm
```

> Example Response

```json
[
	{
		"code": "IDN28500",
		"name": "NSW Severe Weather Warning 1",
		"issueDateTime": "2014-03-26 16:46:03",
		"expireDateTime": "2014-03-27 00:46:03",
		"warningType": {
			"id": 1,
			"code": "severe-weather",
			"name": "Severe Weather",
			"classification": "storm"
			},
		"content": {
			"text": "IDN28500 Australian Government Bureau of Meteorology New South Wales Severe Weather Warning for heavy rain for people in the Northern Tablelands and North West Slopes & Plains forecast districts. Issued at 3:45 am EDT on Thursday 27 March 2014. HEAVY RAIN FOR PARTS OF NORTHEAST NEW SOUTH WALES Weather Situation A strong high pressure system is centred over the southern Tasman Sea, while a low pressure trough lies across western New South Wales. An extensive cloud band associated with this trough is delivering widespread rain over eastern New South Wales. Heavy rain is forecast for parts of the Northern Tablelands and North West Slopes & Plains forecast districts this afternoon and evening. Locations nearer the Queensland border are more likely to be affected, and flash flooding is possible. The State Emergency Service advises that people should: - Don't drive, ride or walk through flood water. - Keep clear of creeks and storm drains. - If you are trapped by flash flooding, seek refuge in the highest available place and ring 000 if you need rescue. For emergency help in floods and storms, ring your local SES Unit on 132 500. The next warning will be issued by 5:00 am EDT Thursday. This warning is also available through TV and Radio broadcasts; the Bureau's website at www.bom.gov.au or call 1300 659 218. The Bureau and State Emergency Service would appreciate this warning being broadcast regularly. ",
			"html": ""
		}
	},
	{
		"code": "IDQ20032",
		"name": "Severe Weather Warning 1",
		"issueDateTime": "2014-03-26 19:02:49",
		"expireDateTime": "2014-03-27 03:02:49",
		"warningType": {
			"id": 1,
			"code": "severe-weather",
			"name": "Severe Weather",
			"classification": "storm"
		},
		"content": {
			"text": " IDQ20032 Bureau of Meteorology Queensland Regional Office TOP PRIORITY FOR IMMEDIATE BROADCAST SEVERE WEATHER WARNING for HEAVY RAINFALL For people in the Wide Bay and Burnett, Darling Downs and Granite Belt, Southeast Coast and parts of the Central Highlands and Coalfields and Maranoa and Warrego Forecast Districts. Issued at 5:02 am Thursday, 27 March 2014. Synoptic Situation: A developing surface trough extends from the central interior into the central Darling Downs. The trough is forecast to drift slowly south during today, before crossing southeastern districts on Friday. A deep moist east to northeast flow will extend from the Coral Sea, across southeastern districts, and into the trough. Rain areas, currently extending through districts to the east of the trough, are expected to develop further in the next 24 hours. Heavy rain, which may lead to flash flooding, is expected to develop during today through the Wide Bay and Burnett, southeastern Central Highlands and Coalfields, the eastern Maranoa and Warrego and the Darling Downs and Granite Belt districts. Heavy rainfall areas are likely to extend to the Southeast Coast district during the afternoon and evening. 24 hour totals of 50 to 150mm are likely, with some isolated heavier falls expected. Locations which may be affected include Gold Coast, Brisbane, Ipswich, Maroochydore, Warwick, Toowoomba, Dalby, Stanthorpe, Goondiwindi, Gympie, Bundaberg, Hervey Bay, Maryborough, Taroom, Kingaroy and St George. Queensland Fire and Emergency Services advises that people should: * Avoid driving, walking or riding through flood waters. * Keep clear of creeks and storm drains. * For emergency assistance contact the SES on 132 500. The next warning is due to be issued by 11:05 am. Warnings are also available through TV and Radio broadcasts, the Bureau's website at www.bom.gov.au or call 1300 659 219. The Bureau and Queensland Fire and Emergency Services would appreciate warnings being broadcast regularly. ",
			"html": "<code>Some warnings have html content</code>"
		}
	}
]
```

Returns all warnings, filtered by warning classification.

### Request

`GET api.willyweather.com.au/v2/{api key}/warnings.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
classifications | csv | `avalanche`, `blizzard`, `closed-water`, `cold`, `cold-rain`, `dust-smoke-pollution`, `earthquake`, `farming`, `fire`, `flood`, `fog`, `frost`, `fruit-disease`, `general`, `hazmat`, `heat`, `hiking`, `hurricane`, `leaf-disease`, `marine`, `road`, `sheep`, `snow`, `storm`, `strong-wind`, `surf`, `tornado`, `tsunami`, `typhoon`, `volcano`, `wind-chill` | the classifications are a fixed list and all new warnings fit an existing classification | false
verbose | boolean |  | include the content attribute with the response | false

<aside class="notice">
	<code>verbose = true</code> by default.
</aside>

### Response

An array of Warning objects, see <a href="#warning">Warning</a> for a description of a Warning response.

## Warning

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/warnings/IDQ20870.json
```

> Example Response

```json
{
	"code": "IDQ20870",
	"name": "Flood Warning - Georgina/Eyre Ck",
	"issueDateTime": "2016-03-26 00:15:46",
	"expireDateTime": "2016-03-29 03:15:46",
	"warningType": {
		"id": 4,
		"code": "floods",
		"name": "Flood",
		"classification": "flood"
	},
	"content": {
		"text": "IDQ20870 Australian Government Bureau of 
				Meteorology Queensland MODERATE FLOOD WARNING FOR
				EYRE CREEK Issued at 10:15 am by the 
				Bureau of Meteorology, Brisbane.",
		"html": "<div>
					<p>IDQ20870</p>
					<p>Australian Government Bureau of Meteorology
					<strong> Queensland</strong></p>
					<h2>MODERATE FLOOD WARNING FOR EYRE CREEK</h2>
					<p><strong>Issued at 10:15 am</strong></p>
					<p>by the Bureau of Meteorology, Brisbane.</p>
				</div>"
	}
}
```

Get a Warning object by `code`.

### Request

`GET api.willyweather.com.au/v2/{api key}/warnings/{warning code}.json`

### Response - Warning Object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
code | string | | a unique identifier, provided by the local weather authority
name | string | 
issueDateTime | string | | `YYYY-MM-DD HH:MM:SS`
expireDateTime | string | | `YYYY-MM-DD HH:MM:SS`
warningType | object | **(see Warning Types)** |
content | object | **(see Content)** |

<aside class="notice">
	<code>issueDateTime</code> and <code>expireDateTime</code> are in <strong>UTC time</strong>.
</aside>
<aside class="notice">
    When the <code>expireDateTime</code> of a warning is in the past, it will be removed from the system. A <code>404</code> will be issued for expired warnings. 
</aside>

### Warning Types

Warning Type codes can differ (and change) depending on the provider and some are quite similar to one another, so we created a list of classifications that we group warnings into.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | | 
code | string | | a unique identifier
name | string | | warning name formatted for display
classifications | csv | `avalanche`, `blizzard`, `closed-water`, `cold`, `cold-rain`, `dust-smoke-pollution`, `earthquake`, `farming`, `fire`, `flood`, `fog`, `frost`, `fruit-disease`, `general`, `hazmat`, `heat`, `hiking`, `hurricane`, `leaf-disease`, `marine`, `road`, `sheep`, `snow`, `storm`, `strong-wind`, `surf`, `tornado`, `tsunami`, `typhoon`, `volcano`, `wind-chill` | the classifications are a fixed list and all new warnings fit an existing classification

### Content

Warnings will occasionally come pre-formatted in html, as well as in plain text

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
text | string | |
html | string | |

## Warnings By Area

> Example Request 

```
https://api.willyweather.com.au/v2/{api key}/locations/5381/warnings.json?classifications=storm,flood&verbose=false
```

> Example Response

```json
[
	{
		"code": "IDQ20885",
		"name": "Queensland flood warning summary",
		"issueDateTime": "2016-03-26 21:53:44",
		"expireDateTime": "2016-03-30 00:53:44",
		"warningType": {
			"id": 22,
			"code": "flood-summary",
			"name": "Flood Summary (state wide)",
			"classification": "flood"
		}	
	},
	{
		"code": "IDQ20780",
		"name": "Flood Warning - Coastal rivers - Maryborough to Gold Coast",
		"issueDateTime": "2016-03-26 21:52:49",
		"expireDateTime": "2016-03-28 00:52:49",
		"warningType": {
			"id": 4,
			"code": "floods",
			"name": "Flood",
			"classification": "flood"
		}
	}
]
```
There are three ways to request Warnings by area.

* **Location id**
* **Region id**
* **State id**

<aside class="notice">
Get <a href="#locations">Locations</a>, <a href="#states">States</a> and <a href="#regions">Regions</a>
</aside>

These will all return an array of Warnings, filtered by classification.

### Requests

`GET api.willyweather.com.au/v2/{api key}/locations/{location id}/warnings.json`

`GET api.willyweather.com.au/v2/{api key}/regions/{region id}/warnings.json`

`GET api.willyweather.com.au/v2/{api key}/states/{state id}/warnings.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
classifications | csv | `avalanche`, `blizzard`, `closed-water`, `cold`, `cold-rain`, `dust-smoke-pollution`, `earthquake`, `farming`, `fire`, `flood`, `fog`, `frost`, `fruit-disease`, `general`, `hazmat`, `heat`, `hiking`, `hurricane`, `leaf-disease`, `marine`, `road`, `sheep`, `snow`, `storm`, `strong-wind`, `surf`, `tornado`, `tsunami`, `typhoon`, `volcano`, `wind-chill` | the classifications are a fixed list and all new warnings fit an existing classification | false
verbose | boolean |  | include the content attribute with the response | false

### Response

An array of Warning objects, see <a href="#warning">Warning</a> for a description of a Warning response.

# Weather

## Overview

> Example Location object included in all weather endpoint responses

```json
{
	"location": {
		"id": 4988,
		"name": "Bondi Beach",
		"region": "Sydney",
		"state": "NSW",
		"postcode": "2026",
		"timeZone": "Australia/Sydney",
		"lat": -33.89054,
		"lng": 151.27486,
		"typeId": 2
	}
}
```

Weather data consists of many parts that can all be requested simultaneously, but must be requested by `location id`. We've broken them up into separate sections so it is easier to read.

<aside class="notice">
    Any weather request will be returned with a location object as the first element in the response (see example). These have been omitted from the example responses below for the sake of reducing clutter
</aside>

<aside class="notice">
	All Forecast <code>dateTime</code> values are in <strong>local time</strong> and are formatted as <strong>YYYY-MM-DD HH:MM:SS</strong>.
</aside>
<aside class="notice">
	All Graph <code>dateTime</code> values are in <strong>local time</strong> and are formatted as a <strong>Unix time stamp</strong>.
</aside>

### Request

`GET api.willyweather.com.au/v2/{api key}/locations/{location id}/weather.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max forecast days returned | false
forecasts | csv |  |  | false
forecastGraphs | csv |  |  | false
observationalGraphs | csv |  |  | false
observational | csv |  |  | false
regionPrecis | csv |  |  | false
units | string | See <a href="#units">Units</a>. |  | false

<aside class="notice">
    <code>days = 7</code> by default.
</aside>

### Forecast Response

A forecast response consists of an object for each `weatherType` requested. Each will contain the array `days`, which contains the forecast data (usually a `dateTime` and `entries`, the latter consisting of an array of forecast days). See each specific section for details on the response information unique to each weather type.

> Example Large Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=weather,wind&forecastGraphs=temperature,precis&observationalGraphs=pressure,temperature&observational=true&regionPrecis=true&days=1
```

> Example Response (without content)

```json
{
	"location": {},
	"forecasts": {
		"weather": {},
		"wind": {}
	},
	"forecastGraphs": {
		"temperature": {},
		"precis": {}
	},
	"observational": {},
	"observationalGraphs": {
		"pressure": {},
		"temperature": {}
	},
	"regionPrecis": {}
}
```

## Forecasts - Moon Phases

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=moonphases&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"moonphases": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"setDateTime": "2014-03-27 02:52:00",
							"riseDateTime": "2014-03-27 16:30:00",
							"phase": "Waning Crescent",
							"phaseCode": "wanc",
							"percentageFull": 14,
							"hemisphere": "s"
						}
					]
				}
			],
			"quarters": [
				{
					"dateTime": "2014-03-31 00:00:00",
					"phase": "New",
					"phaseCode": "new",
					"percentageFull": 0,
					"hemisphere": "s"
				},
				{
					"dateTime": "2014-04-07 00:00:00",
					"phase": "First Quarter",
					"phaseCode": "first",
					"percentageFull": 50,
					"hemisphere": "s"
				},
				{
					"dateTime": "2014-04-15 00:00:00",
					"phase": "Full",
					"phaseCode": "full",
					"percentageFull": 100,
					"hemisphere": "s"
				},
				{
					"dateTime": "2014-04-22 00:00:00",
					"phase": "Last Quarter",
					"phaseCode": "last",
					"percentageFull": 50,
					"hemisphere": "s"
				}
			]
		}
	}
}
```

A Moon Phases forecast will always consist of an array of forecast days, accompanied by a set of quarters, being the next 4 main phases of the moon (new, first, full, last).

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Contains phase information (name, fill % and hemisphere) as well as the rise and set times where applicable.

<aside class="warning">
The Moon does not necessarily have both a rise and a set time for each day (i.e. it may have risen the previous day or be setting the following day) and will return <code>null</code> if there is no rise/set.
</aside>


Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
setDateTime | string | | `YYYY-MM-DD HH:MM:SS` - can be null
riseDateTime | string | | `YYYY-MM-DD HH:MM:SS` - can be null
phase | string | `Waning Gibbous`, `Waning Crescent`, `Waxing Gibbous`, `Waxing Crescent`, `New`, `First Quarter`, `Full`, `Last Quarter` | the full name of the current phase
phaseCode | string | `wang`, `wanc`, `waxg`, `waxc`, `new`, `full`, `first`, `last` | a short code for phase
percentageFull | int | 
hemisphere | string | `s`, `n` | hemisphere affects how the moon appears to the eye

### Quarters

Contains next four quarters. Each quarter contains phase information (name, fill % and hemisphere).

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS` - can be null
phase | string | `Waning Gibbous`, `Waning Crescent`, `Waxing Gibbous`, `Waxing Crescent`, `New`, `First Quarter`, `Full`, `Last Quarter` | the full name of the current phase
phaseCode | string | `wang`, `wanc`, `waxg`, `waxc`, `new`, `full`, `first`, `last` | a short code for phase
percentageFull | int |
hemisphere | string | `s`, `n` | hemisphere affects how the moon appears to the eye

## Forecasts - Precis

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=precis&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"precis": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 00:00:00",
							"precisCode": "showers-rain",
							"precis": "Rain",
							"precisOverlayCode": null,
							"night": false
						}
					]
				}
			],
			"issueDateTime": "2014-03-27 08:14:44"
		}
	}
}
```
Unlike <a href="#forecasts-weather">Weather</a>, a Precis forecast can provide multiple values throughout a day (e.g. every 3 hours).

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
precisCode | string | `fine`, `mostly-fine`, `high-cloud`, `partly-cloudy`, `mostly-cloudy`, `cloudy`, `overcast`, `shower-or-two`, `chance-shower-fine`, `chance-shower-cloud`, `drizzle`, `few-showers`, `showers-rain`, `heavy-showers-rain`, `chance-thunderstorm-fine`, `chance-thunderstorm-cloud`, `chance-thunderstorm-showers`, `thunderstorm`, `chance-snow-fine`, `chance-snow-cloud`, `snow-and-rain`, `light-snow`, `snow`, `heavy-snow`, `wind`, `frost`, `fog`, `hail`, `dust` |
precis | string | | formal, formatted name for precisCode (e.g. "Partly Cloudy")
precisOverlayCode | string | `wind`, `frost`, `fog`, `hail` | this gives a second level of detail to the precis, we use it to show a small icon on top of the existing image. can be `null`
night | boolean | | 

## Forecasts - Rainfall

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=rainfall&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"rainfall": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 00:00:00",
							"startRange": 15,
							"endRange": 25,
							"rangeDivide": "-",
							"rangeCode": "15-25",
							"probability": 100
						}
					]
				}
			],
			"units": {
				"amount": "mm"
			},
			"issueDateTime": "2014-03-27 09:38:00"
		}
	}
}
```

A Rainfall forecast contains a daily summary of rain, in contrast to a <a href="#forecasts-rainfall-probability">Rainfall Probability</a> forecast which will give periodic probability values throughout each day.

Some areas will provide a forecast 'amount' of rainfall, which we display as a range.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
startRange | int |  `1`, `5`, `10`, `15`, `25`, `50`, `null` | can be null when the rangeCode has reached is maximum possible value `100`
endRange | int | `5`,  `10`, `15`, `25`, `50`, `100` |
rangeDivide | string | `>`, `-` | used to link the startRange and endRange (e.g. 15-25). The `>` will be included when there is only a startRange OR endRange (e.g. >50)
rangeCode | string |  `0`, `1-5`, `5-10`, `10-15`, `15-25`, `25-50`, `50-100`, `100` | |
probability | int | `0` - `100` | chance rainfall will occur on that day

<aside class="notice">
The daily probabilty for rainfall will not align with the probability included in <a href="#forecasts-rainfall-probability">Rainfall Probability</a>
</aside>

## Forecasts - Rainfall Probability

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=rainfallprobability&days=1
```

```json
{
	"location": {},
	"forecasts": {
		"rainfallprobability": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 02:00:00",
							"probability": 70
						},
						{
							"dateTime": "2014-03-27 05:00:00",
							"probability": 65
						},
						{
							"dateTime": "2014-03-27 08:00:00",
							"probability": 65
						}
					]
				}
			],
			"units": {
                "percentage": "%"
            },
			"issueDateTime": "2014-03-27 08:55:30"
		}
	}
}
```

A Rainfall Probability forecast contains periodic probability forecasts throughout a day, in contrast to a <a href="#forecasts-rainfall">Rainfall</a> forecast which returns a summary for an entire day.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
probability | int | `0` - `100` | chance rainfall will occur during that period


## Forecasts - Sunrise/Sunset

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=sunrisesunset&days=1
```

```json
{
	"location": {},
	"forecasts": {
		"sunrisesunset": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"firstLightDateTime": "2014-03-27 06:42:53",
							"riseDateTime": "2014-03-27 07:08:16",
							"setDateTime": "2014-03-27 19:01:03",
							"lastLightDateTime": "2014-03-27 19:26:26"
						}
					]
				}
			]
		}
	}
}
```

WillyWeather defines 'first light' and 'last light' as the period before Sunrise and after Sunset when the sky is still illuminated.

This is known formally as <a href="https://en.wikipedia.org/wiki/Twilight#Civil_twilight">Civil Twilight</a>.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
firstLightDateTime | string | | `YYYY-MM-DD HH:MM:SS`
riseDateTime | string | | `YYYY-MM-DD HH:MM:SS`
setDateTime | string | | `YYYY-MM-DD HH:MM:SS`
lastLightDateTime | string | | `YYYY-MM-DD HH:MM:SS`

## Forecasts - Swell

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecasts=swell&days=1
```

```json
{
	"location": {},
	"forecasts": {
		"swell": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 02:00:00",
							"direction": 86.51,
							"directionText": "E",
							"height": 1.5,
							"period": 6.76
						},
						{
							"dateTime": "2014-03-27 05:00:00",
							"direction": 86.92,
							"directionText": "E",
							"height": 1.5,
							"period": 6.81
						},
						{
							"dateTime": "2014-03-27 08:00:00",
							"direction": 84.62,
							"directionText": "E",
							"height": 1.5,
							"period": 6.81
						}
					]
				}
			],
			"units": {
				"height": "m",
				"period": "sec"
			},
			"issueDateTime": "2014-03-26 16:23:30"
		}
	}
}
```

The swell forecast includes both height and period. WillyWeather uses <a href="http://polar.ncep.noaa.gov/waves/index2.shtml">WaveWatch III from NOAA</a>, which provides offshore swell data for the entire globe.

**For Locations that do not have swell data, <code>null</code> will be returned**.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
direction | double | `0` - `360` | degrees, clockwise from North (0). describes the direction the swell originates from
directionText | string | `N`, `NNE`, `NE`, `ENE`, `E`, `ESE`, `SE`, `SSE`, `S`, `SSW`, `SW`, `WSW`, `W`, `WNW`, `NW`, `NNW` | cardinal direction text
height | double | | offshore swell height
period | double | | time in seconds for two consecutive wave crests to pass a stationary point
	
## Forecasts - Temperature

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=temperature&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"temperature": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 00:00:00",
							"temperature": 18.2
						},
						{
							"dateTime": "2014-03-27 01:00:00",
							"temperature": 18
						},
						{
							"dateTime": "2014-03-27 02:00:00",
							"temperature": 17.8
						}
					]
				}
			],
			"units": {
				"temperature": "c"
			},
			"issueDateTime": "2014-03-27 08:35:13"
		}
	}
}
```

A Temperature forecast contains periodic temperature forecasts throughout a day, in contrast to <a href="#forecasts-weather">Weather</a> which provides a daily summary with min/max values.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
temperature | double | |

## Forecasts - Tides

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecasts=tides&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"tides": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 06:02:00",
							"height": 5.61,
							"type": "high"
						},
						{
							"dateTime": "2014-03-27 12:38:00",
							"height": 1.08,
							"type": "low"
						},
						{
							"dateTime": "2014-03-27 18:47:00",
							"height": 4.89,
							"type": "high"
						}
					]
				}
			],
			"units": {
				"height": "ft"
			},
			"issueDateTime": "2014-03-27 00:53:19"
		}
	}
}
```

A tide forecast contains all tide events (high or low) for each day. The number of tide events per day differs.

**For Locations that do not have tide data, <code>null</code> will be returned**.

<aside class="notice">
The number of tide events per day may differ.
</aside>

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
height | double | | 
type | string | `high`, `low` |

## Forecasts - UV

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=uv&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"uv": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 11:00:00",
							"index": 0,
							"scale": "low"
						},
						{
							"dateTime": "2014-03-27 12:00:00",
							"index": 0,
							"scale": "low"
						},
						{
							"dateTime": "2014-03-27 13:00:00",
							"index": 6,
							"scale": "high"
						},
						{
							"dateTime": "2014-03-27 14:00:00",
							"index": 7,
							"scale": "high"
						}
					],
					"alert": {
					    "maxIndex": 7,
					    "scale": "high",
					    "startDateTime": "2014-03-27 13:00:00",
					    "endDateTime":"2014-03-27 14:00:00"
					}
				}
			],
			"issueDateTime": "2014-03-27 05:02:10"
		}
	}
}
```

A UV forecast contains periodic UV forecasts throughout a day along with an alert. The alert will only be provided if the forecast day has an index greater than 3.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
index | double | `0` - `20` | <a href="https://en.wikipedia.org/wiki/Ultraviolet_index#How_to_use_the_index">uv index</a>
scale | string | `low`, `moderate`, `high`, `very-high`, `extreme` | a name given to <a href="https://en.wikipedia.org/wiki/Ultraviolet_index#How_to_use_the_index">uv index</a> ranges

### Alert

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
maxIndex | double | `3` - `20` | <a href="https://en.wikipedia.org/wiki/Ultraviolet_index#How_to_use_the_index">uv index</a>
scale | string | `low`, `moderate`, `high`, `very-high`, `extreme` | a name given to <a href="https://en.wikipedia.org/wiki/Ultraviolet_index#How_to_use_the_index">uv index</a> ranges
startDateTime | string | | `YYYY-MM-DD HH:MM:SS`
endDateTime | string | | `YYYY-MM-DD HH:MM:SS`


## Forecasts - Weather

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=weather&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"weather": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 00:00:00",
							"precisCode": "showers-rain",
							"precis": "Rain at times",
							"precisOverlayCode": null,
							"night": false,
							"min": 17,
							"max": 22
						}
					]	
				}
			],
			"units": {
				"temperature": "c"
			},
			"issueDateTime": "2014-03-27 08:14:44"
		}
	}
}
```

A Weather forecast contains all the information required to provide a brief summary of a day's forecast.

### Days

An array of forecast days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
precisCode | array | `fine`, `mostly-fine`, `high-cloud`, `partly-cloudy`, `mostly-cloudy`, `cloudy`, `overcast`, `shower-or-two`, `chance-shower-fine`, `chance-shower-cloud`, `drizzle`, `few-showers`, `showers-rain`, `heavy-showers-rain`, `chance-thunderstorm-fine`, `chance-thunderstorm-cloud`, `chance-thunderstorm-showers`, `thunderstorm`, `chance-snow-fine`, `chance-snow-cloud`, `snow-and-rain`, `light-snow`, `snow`, `heavy-snow`, `wind`, `frost`, `fog`, `hail`, `dust` |
precis | string | | formal, formatted name for precisCode (e.g. "Partly Cloudy")
precisOverlayCode | string | `wind`, `frost`, `fog`, `hail` | this gives a second level of detail to the precis, we use it to show a small icon on top of the existing image. can be `null`
night | boolean | |
min | int | | minimum daily temperature
max | int | | maximum daily temperature  

## Forecasts - Wind

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=wind&days=1
```

> Example Response

```json
{
	"location": {},
	"forecasts": {
		"wind": {
			"days": [
				{
					"dateTime": "2014-03-27 00:00:00",
					"entries": [
						{
							"dateTime": "2014-03-27 00:00:00",
							"speed": 5,
							"direction": 13,
							"directionText": "NNE"
						},
						{
							"dateTime": "2014-03-27 01:00:00",
							"speed": 4.7,
							"direction": 18,
							"directionText": "NNE"
						},
						{
							"dateTime": "2014-03-27 02:00:00",
							"speed": 4.3,
							"direction": 20,
							"directionText": "NNE"
						}
					]
				}
			],
			"units": {
				"speed": "km/h"
			},
			"issueDateTime": "2014-03-27 10:02:44"
		}
	}
}
```

The wind forecast provides periodic data for each day.

### Days

An array of forecast days

### Forecast Days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
speed | double | | |
direction | double | `0` - `360` | degrees, clockwise from North (0). describes the direction the wind originates from
directionText | string | `N`, `NNE`, `NE`, `ENE`, `E`, `ESE`, `SE`, `SSE`, `S`, `SSW`, `SW`, `WSW`, `W`, `WNW`, `NW`, `NNW` | cardinal direction text

## Forecast Graphs - Precis

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=precis&days=1
```

> Example Response

```json
{
	"location": {},
	"forecastGraphs": {
		"precis": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "precis",
						"lineFill": false,
						"showPoints": true,
						"pointRenderer": "PrecisSummaryPointRenderer",
						"pointFormatter": "PrecisSummaryPointFormatter"
					},	
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"precisCode": "cloudy",
									"night": true
								},
								{
									"x": 1395882000,
									"precisCode": "shower-or-two",
									"night": true
								},
								{
									"x": 1395885600,
									"precisCode": "shower-or-two",
									"night": true
								}
							]
						}
					]
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"issueDateTime": "2014-03-27 08:14:44",
			"nextIssueDateTime": "2014-03-27 09:14:44"
		}
	}
}
```

Precis forecast graphs do not have a y axis, they are just icons used to summarise the weather at that point in time during the day.

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int | | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | |  **(see Config below)**
groups | object | | **(see Groups below)**

### Config

The config is used to assist in styling the series. Each series will have different values, this is specific to `precis`.	

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `precis` |
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
showPoints | boolean | `true` | whether to show data points or just display a line
pointRenderer | string | `PrecisSummaryPointRenderer` |
pointFormatter | string | `PrecisSummaryPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
precisCode | string | `fine`, `mostly-fine`, `high-cloud`, `partly-cloudy`, `mostly-cloudy`, `cloudy`, `overcast`, `shower-or-two`, `chance-shower-fine`, `chance-shower-cloud`, `drizzle`, `few-showers`, `showers-rain`, `heavy-showers-rain`, `chance-thunderstorm-fine`, `chance-thunderstorm-cloud`, `chance-thunderstorm-showers`, `thunderstorm`, `chance-snow-fine`, `chance-snow-cloud`, `snow-and-rain`, `light-snow`, `snow`, `heavy-snow`, `wind`, `frost`, `fog`, `hail`, `dust`
night | boolean | | used to show a moon instead of a sun in icons

## Forecast Graphs - Rainfall Probability

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=rainfallprobability&days=1
```

```json
{
	"location": {},
	"forecastGraphs": {
		"rainfallprobability": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "rainfallprobability",
						"color": "#289df5",
						"lineWidth": 8,
						"lineFill": false,
						"lineRenderer": "BarLineRenderer",
						"showPoints": false,
						"pointFormatter": "RainfallProbabilityPointFormatter"
					},
					"yAxisDataMin": 40,
					"yAxisDataMax": 50,
					"yAxisMin": 0,
					"yAxisMax": 100,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395885600,
									"y": 50
								},
								{
									"x": 1395896400,
									"y": 50
								},
								{
									"x": 1395907200,
									"y": 45
								}
							]
						}
					],
					"controlPoints": {
						"pre": {
							"x": 1395874800,
							"y": 45
						},
						"post": {
							"x": 1395972000,
							"y": 35
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"units": {
			    "percentage": "%"
            },
			"issueDateTime": "2014-03-27 08:55:30",
			"nextIssueDateTime": "2014-03-27 22:55:30"
		}
	}
}
```

Rainfall probabilty graphs display periodic values giving a chance of rain.

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | |  **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int | | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

The config is used to assist in styling the series. Each series will have different values, this is specific to `rainfallprobability`.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `rainfallprobability` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `BarLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `RainfallProbabilityPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | `0` - `100` | rainfall probability

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Forecast Graphs - Sunrise/Sunset

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=sunrisesunset&days=1
```

```json
{
	"location": {},
	"forecastGraphs": {
		"sunrisesunset": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "sunrisesunset",
						"lineFill": true,
						"lineRenderer": "SunLineRenderer",
						"showPoints": false,
						"pointFormatter": "SunriseSunsetPointFormatter"
					},
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395902317,
									"description": "First Light"
								},
								{
									"x": 1395903806,
									"description": "Sunrise"
								},
								{
									"x": 1395946625,
									"description": "Sunset"
								},
								{
									"x": 1395948114,
									"description": "Last Light"
								}
							]
						}
					]
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			}
		}
	}
}
```

Sunrise / Sunset forecast graph, we use this as a background to our normal graphs to assist in the indication of time.

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

The config is used to assist in styling the series. Each series will have different values, this is specific to `sunrisesunset`.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `sunrisesunset` |
color | string | | hexadecimal colour code
lineFill | boolean | `true` | whether the area under the graph should have a fill or not
lineRenderer | string | `SunLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `SunriseSunsetPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
description | string | `First Light`, `Sunrise`, `Sunset`, `Last Light` | 

## Forecast Graphs - Swell Height

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=swell-height&days=1
```

> Example Response

```json
{
	"location": {},
	"forecastGraphs": {
		"swell-height": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "swell-height",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": true,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": true,
						"pointRenderer": "ArrowPointRenderer",
						"pointFormatter": "DirectionPointFormatter"
					},
					"yAxisDataMin": 1.3,
					"yAxisDataMax": 1.6,
					"yAxisMin": 0,
					"yAxisMax": 8,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"y": 1.6,
									"direction": 84.2,
									"directionText": "E",
									"description": "moderate",
									"pointStyle": {
										"fill": "#5dacfd",
										"stroke": "#4178b1"
									}
								},
								{
									"x": 1395885600,
									"y": 1.5,
									"direction": 86.51,
									"directionText": "E",
									"description": "moderate",
									"pointStyle": {
										"fill": "#5dacfd",
										"stroke": "#4178b1"
									}
								},
								{
									"x": 1395896400,
									"y": 1.5,
									"direction": 86.92,
									"directionText": "E",
									"description": "moderate",
									"pointStyle": {
										"fill": "#5dacfd",
										"stroke": "#4178b1"
									}
								}
							]
						}
					],
					"controlPoints": {
						"pre": {
							"x": 1395874800,
							"y": 1.6,
							"direction": 84.2,
							"directionText": "E",
							"description": "moderate",
							"pointStyle": {
								"fill": "#5dacfd",
								"stroke": "#4178b1"
							}
						},
						"post": {
							"x": 1395972000,
							"y": 1.3,
							"direction": 73.08,
							"directionText": "ENE",
							"description": "moderate",
							"pointStyle": {
								"fill": "#5dacfd",
								"stroke": "#4178b1"
							}
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"units": {
                "height": "m"
            },
			"issueDateTime": "2014-03-26 16:23:30",
			"nextIssueDateTime": "2014-03-27 16:23:30"
		}
	}
}
```

Swell Height forecast graph series, can be plotted on the same graph as a <a href="#forecast-graphs-swell-period">Swell Period</a> graph.

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

The config is used to assist in styling the series. Each series will have different values, this is specific to `swell-height`.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `swell-height` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `true` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `true` | whether to show data points or just display a line
pointRenderer | string | `ArrowPointRenderer` |
pointFormatter | string | `DirectionPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | swell height
direction | double | `0` - `360` | degrees, clockwise from North (0). describes the direction the swell originates from
directionText | string | `N`, `NNE`, `NE`, `ENE`, `E`, `ESE`, `SE`, `SSE`, `S`, `SSW`, `SW`, `WSW`, `W`, `WNW`, `NW`, `NNW` | cardinal direction text
description | string | `glassy`, `smooth`, `slight`, `moderate`, `rough`, `very-rough`, `high`, `very-high`, `phenomenal` |
pointStyle | object | | **(see Point Style below)**

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Forecast Graphs - Swell Period

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=swell-period&days=1
```

```json
{
	"location": {},
	"forecastGraphs": {
		"swell-period": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "swell-period",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": true,
						"pointRenderer": "CirclePointRenderer",
						"pointFormatter": "SwellPeriodPointFormatter",
						"pointStyle": {
							"fill": "#FFFFFF",
							"stroke": "#003355"
						}
					},
					"yAxisDataMin": 6.57,
					"yAxisDataMax": 7.06,
					"yAxisMin": 0,
					"yAxisMax": 25,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"y": 6.57
								},
								{
									"x": 1395885600,
									"y": 6.76
								},
								{
									"x": 1395896400,
									"y": 6.81
								}
							]
						}
					],
					"controlPoints": {
						"pre": {
							"x": 1395874800,
							"y": 6.57
						},
						"post": {
							"x": 1395972000,
							"y": 7.11
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"units": {
                "period": "sec"
            },
			"issueDateTime": "2014-03-26 16:23:30",
			"nextIssueDateTime": "2014-03-27 16:23:30"
		}
	}
}
```

Swell Period forecast graph series, can be plotted on the same graph as a <a href="#forecast-graphs-swell-height">Swell Height</a> graph.

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | |  **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `swell-period` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `true` | whether to show data points or just display a line
pointRenderer | string | `CirclePointRenderer` |
pointFormatter | string | `SwellPeriodPointFormatter` |
pointStyle | object | | **(see Point Style below)**

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | period in seconds

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Forecast Graphs - Temperature

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=temperature&days=1
```

```json
{
	"location": {},
	"forecastGraphs": {
		"temperature": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "temperature",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "TemperaturePointFormatter"
					},
					"yAxisDataMin": 19.2,
					"yAxisDataMax": 24,
					"yAxisMin": 12,
					"yAxisMax": 32,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"y": 21.4
								},
								{
									"x": 1395882000,
									"y": 21.6
								},
								{
									"x": 1395885600,
									"y": 21.7
								}
							]
						}
					],
					"controlPoints": {
						"pre": {
							"x": 1395874800,
							"y": 21.4
						},
						"post": {
							"x": 1395964800,
							"y": 21.1
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"units": {
                "temperature": "c"
            },
			"issueDateTime": "2014-03-27 08:35:13",
			"nextIssueDateTime": "2014-03-27 09:35:13"
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double| | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Point below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `temperature` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `TemperaturePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | temperature

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Forecast Graphs - Tides

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=tides&days=1
```

> Example Response

```json
{
	"location": {},
	"forecastGraphs": {
		"tides": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "tides",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": true,
						"lineRenderer": "CurvedLineRenderer",
						"showPoints": false,
						"pointFormatter": "TidePointFormatter"
					},
					"yAxisDataMin": 0,
					"yAxisDataMax": 6.86,
					"yAxisMin": 0,
					"yAxisMax": 7.55,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"y": 1.77,
									"description": "Low",
									"interpolated": false
								},
								{
									"x": 1395879276,
									"y": 1.86,
									"description": "",
									"interpolated": true
								},
								{
									"x": 1395880434,
									"y": 1.98,
									"description": "",
									"interpolated": true
								},
								{
									"x": 1395881592,
									"y": 2.14,
									"description": "",
									"interpolated": true
								}
							],
							"overlays": [
								{
									"renderer": "MoonPhasesOverlayRenderer",
									"data": {
										"phase": "Waning Crescent",
										"phaseCode": "wanc",
										"percentageFull": 14,
										"hemisphere": "s"
									}
								}
							]
						}
					],
					"controlPoints": {
						"pre": {
							"x": 1395876960,
							"y": 1.77,
							"description": "Low",
							"interpolated": false
						},
						"post": {
							"x": 1395967200,
							"y": 1.44,
							"description": "Low",
							"interpolated": false
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"carousel": {
				"size": 9,
				"start": 3
			},
			"units": {
                "height": "ft"
            },
			"issueDateTime": "2014-03-27 00:53:19",
			"nextIssueDateTime": "2014-03-28 00:53:19"
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Point below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `tides` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `true` | whether the area under the graph should have a fill or not
lineRenderer | string | `CurvedLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `TidePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | tide height
description | string | `low`, `high` | empty string when not a low or high tide
interpolated | boolean | | `true` when not a low or high tide (data is generated parabolically from low and high points)

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast Graphs - UV

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=uv&days=1
```

> Example Response

```json
{
	"location": {},
	"forecastGraphs": {
		"uv": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "uv",
						"color": "#036036",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "UVPointFormatter"
					},
					"yAxisDataMin": 0,
					"yAxisDataMax": 2.1,
					"yAxisMin": 0,
					"yAxisMax": 20,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"y": 0,
									"description": "low"
								},
								{
									"x": 1395882000,
									"y": 0,
									"description": "low"
								},
								{
									"x": 1395885600,
									"y": 0,
									"description": "low"
								}
							]
						}
					],
					"controlPoints": {
						"post": {
							"x": 1395964800,
							"y": 0,
							"description": "low"
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"issueDateTime": "2014-03-27 05:02:10",
			"nextIssueDateTime": "2014-03-28 05:02:10"
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Point below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `uv` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `UVPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | <a href="https://en.wikipedia.org/wiki/Ultraviolet_index">uv index</a>
description | string | `low`, `moderate`, `high`, `very-high`, `extreme` | a name given to <a href="https://en.wikipedia.org/wiki/Ultraviolet_index">uv index</a> ranges

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Forecast Graphs - Wind

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=wind&days=1
```

> Example Response

```json
{
	"location": {},
	"forecastGraphs": {
		"wind": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "wind",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": true,
						"pointRenderer": "ArrowPointRenderer",
						"pointFormatter": "DirectionPointFormatter"
					},
					"yAxisDataMin": 3.6,
					"yAxisDataMax": 23.4,
					"yAxisMin": 0,
					"yAxisMax": 74.1,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395878400,
									"y": 22.3,
									"direction": 41,
									"directionText": "NE",
									"description": "moderate",
									"pointStyle": {
										"fill": "#48ad00",
										"stroke": "#2b6700"
									}
								},
								{
									"x": 1395882000,
									"y": 19.8,
									"direction": 39,
									"directionText": "NE",
									"description": "moderate",
									"pointStyle": {
										"fill": "#48ad00",
										"stroke": "#2b6700"
									}
								},
								{
									"x": 1395885600,
									"y": 16.9,
									"direction": 38,
									"directionText": "NE",
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								}
							]
						}
					],
					"controlPoints": {
						"pre": {
							"x": 1395874800,
							"y": 24.5,
							"direction": 42,
							"directionText": "NE",
							"description": "moderate",
							"pointStyle": {
								"fill": "#48ad00",
								"stroke": "#2b6700"
							}
						},
						"post": {
							"x": 1395964800,
							"y": 5,
							"direction": 328,
							"directionText": "NNW",
							"description": "light",
							"pointStyle": {
								"fill": "#d1ef51",
								"stroke": "#7d8f30"
							}
						}
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1395964799
			},
			"units": {
			    "speed": "km/h"
			},
			"issueDateTime": "2014-03-27 10:02:44",
			"nextIssueDateTime": "2014-03-27 22:02:44"
		}
	}
}
```

### Data Config
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | doube | | the largest y value
yAxisMin | doube | | the smallest y value with graph padding
yAxisMax | doube | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `wind` | |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `true` | whether to show data points or just display a line
pointRenderer | string | `ArrowPointRenderer` |
pointFormatter | string | `DirectionPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
x | int | | time value
y | double | | wind speed
direction | double | `0` - `360` | degrees, clockwise from North (0). describes the direction the wind originates from
directionText | string | `N`, `NNE`, `NE`, `ENE`, `E`, `ESE`, `SE`, `SSE`, `S`, `SSW`, `SW`, `WSW`, `W`, `WNW`, `NW`, `NNW` | cardinal direction text
description | string | `calm`, `light`, `gentle`, `moderate`, `fresh`, `strong`, `near-gale`, `gale`, `strong-gale`, `storm`, `violent`, `cyclone` |
pointStyle | object | | **(see Point Style below)**

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Observational Graphs - Dew Point

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=dew-point
```

> Example Response

```json
{
	"location": {},
	"observationalGraphs": {
		"dew-point": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "dew-point",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "DewPointPointFormatter"
					},
					"yAxisDataMin": 18.6,
					"yAxisDataMax": 19.4,
					"yAxisMin": 12,
					"yAxisMax": 32,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 18.7
								},
								{
									"x": 1395891000,
									"y": 19.4
								},
								{
									"x": 1395892800,
									"y": 19.2
								}
							]
						}
					],
					"controlPoints": {
				        "pre": {
				            "x": 1395878399,
				             "y": 18.7
				        },
                        "post": null
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "temperature": "c"
            },
			"provider": {
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			}
		}
	}
}
```

The temperature at which dew will form.

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Provider

The station used to gather this data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | double | |
lng | double | |
distance | double | | distance of provider from location
units | object | | includes unit of measurement for distance

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double| | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `dew-point` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` DewPointPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | dew point

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Observational Graphs - Pressure

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=pressure
```

> Example Response

```json
{
	"location": {},
	"observationalGraphs": {
		"pressure": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "pressure",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "PressurePointFormatter"
					},
					"yAxisDataMin": 1021,
					"yAxisDataMax": 1023,
					"yAxisMin": 850,
					"yAxisMax": 1100,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 1023
								},
								{
									"x": 1395891000,
									"y": 1021
								},
								{
									"x": 1395892800,
									"y": 1021
								}
							]
						}
					],
					"controlPoints": {
					    "pre": {
					        "x": 1395878399,
                            "y": 1023
					    }, 
					    "post": null
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "pressure": "hPa"
            },
			"provider": {
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			}
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Provider

The station used to gather this data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | double | |
lng | double | |
distance | double | | distance of provider from location
units | object | | includes unit of measurement for distance

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `pressure` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `PressurePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
x | int | | time value
y | double | | pressure

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Observational Graphs - Rainfall

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=rainfall
```

> Example Response

```json
{
	"location": {},
	"observationalGraphs": {
		"rainfall": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "rainfall",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "BarLineRenderer",
						"showPoints": false,
						"pointFormatter": "RainfallPointFormatter"
					},
					"yAxisDataMin": 2.6,
					"yAxisDataMax": 14,
					"yAxisMin": 0,
					"yAxisMax": 15.4,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 11.8
								},
								{
									"x": 1395891000,
									"y": 11.8
								},
								{
									"x": 1395892800,
									"y": 12
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878399,
                            "y": 11.7
                        }, 
                        "post": null
                    }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "amount": "mm"
            },
			"provider": {
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			}
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Provider

The station used to gather this data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | double | |
lng | double | |
distance | double | | distance of provider from location
units | object | | includes unit of measurement for distance

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `rainfall` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `BarLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `RainfallPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------

points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
x | int | | time value
y | double | | amount

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Observational Graphs - Temperature

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=temperature
```

> Example Response

```json
{
	"location": {},
	"observationalGraphs": {
		"temperature": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "temperature",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "TemperaturePointFormatter"
					},
					"yAxisDataMin": 21,
					"yAxisDataMax": 22.6,
					"yAxisMin": 12,
					"yAxisMax": 32,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 21.3
								},
								{
									"x": 1395891000,
									"y": 22.5
								},
								{
									"x": 1395892800,
									"y": 22.6
								}
							]
						}
					],
					"controlPoints": {
                    "pre": {
                        "x": 1395878399,
                        "y": 21
                    }, 
                    "post": null
                }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "temperature": "c"
            },
			"provider": {
				"id": 733,
				"name": "Wedding Cake West",
				"lat": -33.84,
				"lng": 151.26,
				"distance": 3.6,
				"units": {
					"distance": "miles"
				}
			}
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Provider

The station used to gather this data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | double | |
lng | double | |
distance | double | | distance of provider from location
units | object | | includes unit of measurement for distance

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `temperature` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `TemperaturePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
x | int | | time value
y | double | | temperature

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Observational Graphs - Wind

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=wind
```

> Example Response

```json
{
	"location": {},
	"observationalGraphs": {
		"wind": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "wind",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": true,
						"pointRenderer": "ArrowPointRenderer",
						"pointFormatter": "DirectionPointFormatter"
					},
					"yAxisDataMin": 7.4,
					"yAxisDataMax": 20.4,
					"yAxisMin": 0,
					"yAxisMax": 74.1,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 18.5,
									"direction": 10,
									"directionText": "N",
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								},
								{
									"x": 1395891000,
									"y": 13,
									"direction": 30,
									"directionText": "NNE",
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								},
								{
									"x": 1395892800,
									"y": 16.7,
									"direction": 10,
									"directionText": "N",
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878399,
                            "y": 16.7,
                            "direction": 10,
                            "directionText": "N",
                            "description": "gentle",
                            "pointStyle": {
                                "fill": "#a5de37",
                                "stroke": "#638521"
                            }
                        }, 
                        "post": null
                    }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
			    "speed": "km/h"
			},
			"provider": {
				"id": 733,
				"name": "Wedding Cake West",
				"lat": -33.84,
				"lng": 151.26,
				"distance": 3.6,
				"units": {
					"distance": "miles"
				}
			}
		}
	}
}
```

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object | | **(see Series below)**
xAxisMin | int | | start time of the graph period
xAxisMax | int| | end time of the graph period

### Provider

The station used to gather this data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | double | |
lng | double | |
distance | double | | distance of provider from location
units | object | | includes unit of measurement for distance

### Series

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
config | object | | **(see Config below)**
yAxisDataMin | double | | the smallest y value
yAxisDataMax | double | | the largest y value
yAxisMin | double | | the smallest y value with graph padding
yAxisMax | double | | the largest y value with graph padding
groups | object | | **(see Groups below)**
controlPoints | object | | **(see Control Points below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `wind` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `true` | whether to show data points or just display a line
pointRenderer | string | `ArrowPointRenderer` |
pointFormatter | string | `DirectionPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | speed
direction | double | `0` - `360` | degrees, clockwise from North (0). describes the direction the wind originates from
directionText | string | `N`, `NNE`, `NE`, `ENE`, `E`, `ESE`, `SE`, `SSE`, `S`, `SSW`, `SW`, `WSW`, `W`, `WNW`, `NW`, `NNW` | cardinal direction text
description | string | `calm`, `light`, `gentle`, `moderate`, `fresh`, `strong`, `near-gale`, `gale`, `strong-gale`, `storm`, `violent`, `cyclone`
pointStyle | object | | **(see Point Style below)**

### Point Style

Colour descriptions

Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
fill | string | | hexadecimal colour code
stroke | string | | hexadecimal colour code

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

## Observational

Observational provides real time data from one or more weather stations.

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observational=true
```

> Example Response

```json
{
	"location": {},
    "observational": {
        "observations": {
            "temperature": {
                "temperature": 19.3,
                "apparentTemperature": 12.3,
                "trend": 1
            },
            "humidity": {
                "percentage": 26
            },
            "dewPoint": {
                "temperature": -0.6,
                "trend": -1
            },
            "pressure": {
                "pressure": 1007.2,
                "trend": 1
            },
            "wind": {
                "speed": 27.8,
                "gustSpeed": 42.6,
                "trend": 1,
                "direction": 292.5,
                "directionText": "WNW"
            },
            "rainfall": {
                "lastHourAmount": 0,
                "todayAmount": 0,
                "since9AMAmount": 0
            }
        },
        "stations": {
            "temperature": {
                "id": 733,
                "name": "Wedding Cake West",
                "lat": -33.84,
                "lng": 151.26,
                "distance": 5.8
            },
            "humidity": {
                "id": 349,
                "name": "Sydney (Observatory Hill)",
                "lat": -33.86,
                "lng": 151.21,
                "distance": 6.9
           },
           "dewPoint": {
                "id": 349,
                "name": "Sydney (Observatory Hill)",
                "lat": -33.86,
                "lng": 151.21,
                "distance": 6.9
          },
          "pressure": {
                "id": 349,
                "name": "Sydney (Observatory Hill)",
                "lat": -33.86,
                "lng": 151.21,
                "distance": 6.9
          },
          "wind": {
                "id": 733,
                "name": "Wedding Cake West",
                "lat": -33.84,
                "lng": 151.26,
                "distance": 5.8
          },
          "rainfall": {
                "id": 349,
                "name": "Sydney (Observatory Hill)",
                "lat": -33.86,
                "lng": 151.21,
                "distance": 6.9
            }        
        },
        "issueDateTime": "2016-07-12 14:10:00",
        "units": {
            "temperature": "c",
            "amount": "mm",
            "speed": "km/h",
            "distance": "km",
            "pressure": "hPa"
        }
    }
}
```

### Observations
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | object | |
humidity | object | |
dewPoint | object | |
wind | object | |
rainfall | object | |

### Temperature
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | double | |
apparentTemperature | double |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising

### Humidity
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
percentage | double | |

### Dew Point
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | double | |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising

### Pressure
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
pressure | double | |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising

### Wind
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
speed | double | |
gustSpeed | double | |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising
direction | double | `0` - `360` | degrees, clockwise from North (0). describes the direction the wind originates from
directionText | string | `N`, `NNE`, `NE`, `ENE`, `E`, `ESE`, `SE`, `SSE`, `S`, `SSW`, `SW`, `WSW`, `W`, `WNW`, `NW`, `NNW` | cardinal direction text

### Rainfall
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
lastHourAmount | double | |
todayAmount | double | |
since9AMAmount | double | |

### Stations

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | object | | **(see Station below)**
pressure | object | | **(see Station below)**
wind | object | | **(see Station below)**
rainfall | object | | **(see Station below)**

### Station

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
name | int | |
distance | string | | the distance from the Station to the Location

## Region Precis

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?regionPrecis=true&days=1
```

> Example Response

```json
{
	"location": {},
	"regionPrecis": {
		"days": [
			{
				"dateTime": "2014-03-27 00:00:00",
				"entries": [
					{
						"dateTime": "2014-03-27 00:00:00",
						"precis": "Cloudy. Areas of rain. Winds northeasterly 15 to 25 km/h."
					}
				]
			}
		],
		"issueDateTime": "2014-03-27 08:48:11",
		"name": "Sydney Area"
	}
}
```

### Days

An array of forecast days.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | **(see Entries below)**

### Entries

Each day can contain a maximum of two entries.
A single entry signifies the forecast is for the whole day. Multiple entries signify a forecast for the morning and a forecast for the eventing.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
precis | string | | long text weather description

## Summaries

> Example Request

```shell
https://api.willyweather.com.au/v2/{api key}/weather/summaries.json?ids=16
```

> Example Response

```json
[
	{
		"location": {
			"id": 16,
			"name": "Canberra",
			"region": "Canberra",
			"state": "ACT",
			"postcode": "2600",
			"timeZone": "Australia/Sydney",
			"lat": -35.28204,
			"lng": 149.12858,
			"typeId": 22
		},
		"forecasts": {
			"weather": {
				"days": [
					{
						"dateTime": "2016-03-27 00:00:00",
						"entries": [
							{
								"dateTime": "2016-06-27 00:00:00",
								"precisCode": "chance-shower-fine",
								"precis": "Rain easing",
								"precisOverlayCode": null,
								"night": false,
								"min": 15,
								"max": 19
							}
						]
					}
				],
				"units": {
					"temperature": "c"
				}
			}
		},
		"observational": {
		    "observations": {
			    "temperature": 17.5
            },
			"units": {
				"temperature": "c"
			}
		}
	}
]
```

Returns an array of brief weather and observational summaries, each being for a location provided in request. Each summary contains a weather forecast ()precis, min temperature, max temperature) as well as observational temperature.

### Request

`GET api.willyweather.com.au/v2/{api key}/weather/summaries.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
ids | csv |  | list of location ids | true
units | csv | See <a href="/#units">Units</a>. | | true


### Response

### Summary

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
location | object | | 
forecasts | object | | 
observational | object | |

### Location

See <a href="#locations">Locations</a> for description a of Location response.

### Forecasts

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
weather | object | |

### Weather

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
days | array | | 
units | object | |

### Days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
entries | array | | 

### Entries

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
precisCode | string | `fine`, `mostly-fine`, `high-cloud`, `partly-cloudy`, `mostly-cloudy`, `cloudy`, `overcast`, `shower-or-two`, `chance-shower-fine`, `chance-shower-cloud`, `drizzle`, `few-showers`, `showers-rain`, `heavy-showers-rain`, `chance-thunderstorm-fine`, `chance-thunderstorm-cloud`, `chance-thunderstorm-showers`, `thunderstorm`, `chance-snow-fine`, `chance-snow-cloud`, `snow-and-rain`, `light-snow`, `snow`, `heavy-snow`, `wind`, `frost`, `fog`, `hail`, `dust` |
precis | string | | formal, formatted name for precisCode (e.g. "Partly Cloudy")
precisOverlayCode | string | `wind`, `frost`, `fog`, `hail` | this gives a second level of detail to the precis, we use it to show a small icon on top of the existing image. can be `null`
night | boolean | | 
min | int | | daily temperature
max | int | | daily temperature

### Observational
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
observations | object | |

### Observations
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | object | |

### Temperature
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | double | |

# Units

> Example Request

```shell
?units=amount:mm,distance:km,speed:knots,swellHeight:ft,temperature:f,tideHeight:m
```

> Example Response

```json
{
    "units": {
        "amount": "mm",
        "distance": "km",
        "speed": "knots",
        "swellHeight": "ft",
        "temperature": "c",
        "tideHeight": "ft"
    }
}
```

The Units parameter allows the data to be converted to a specific unit. The format of the preferred units will form part of the query string.

### Request

`GET ?units=amount:mm,distance:km,speed:knots,swellHeight:ft,temperature:f,tideHeight:m`

### Response
Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
amount | string | `mm`, `pts`, `in` | millimetres, points, inches
distance | string | `km`, `miles` | kilometers, miles
speed | string | `km/h`, `mph`, `m/s`, `knots` | kilometers per hour, miles per hour, meters per second, knots
swellHeight | string | `m`, `ft` | meters, feet
temperature | string | `c`, `f` | celsius, fahrenheit
tideHeight | string | `m`, `ft` | meters, feet