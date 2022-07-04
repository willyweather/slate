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

## Info - GET - Legal

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/info/legal.json?platform=web
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
    "HTTP_X_PAYLOAD": {
    	"platform" : "mobile"
    }
}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

## Info - GET - By Location Id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/{location id}/info.json?platform=iphone&weatherTypes=weather&mapTypes=radar-rainfall&forecastGraphTypes=wind&observationalGraphTypes=temperature&graphKeyTypes=wind
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"platform": "iphone",
         "weatherTypes": [
			{
				"code": "weather"
			}
		],
		"mapTypes": [
			{
				"code": "radar-rainfall"
			}
		],
		"graphKeyTypes": ["wind"],
		"forecastGraphTypes": ["precis"],
		"observationalGraphTypes": ["dew-point"]
	}
}
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
observationalGraphTypes | csv | `temperature`, `dew-point`, `pressure`, `rainfall`, `wind`, `apparent-temperature`, `cloud`, `delta-t`, `wind-gust`, `humidity` | option
graphKeyTypes | csv | `wind`, `swell-height` | option
units | csv | <a href="#units">Units</a> | false

<aside class="notice">
Either <code>weatherTypes</code>, <code>mapTypes</code>, <code>forecastGraphTypes</code> or <code>observationalGraphTypes</code> or <code>graphKeyTypes</code> is required.
</aside>

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

# Locations

## Location - GET - by Location id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/{location id}.json
```

> Example Request Header

```json
{}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
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

## Map Provider - GET - Get Map Providers

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/maps.json?mapTypes=forecast-regional-radar&lat=-25.97&lng=133.91&verbose=true&offset=-60&limit=30&units=distance:km
```
> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"lat"  : 23.455,
		"lng" : 150.67,
		"mapTypes" :[
			{
				"code" : "forecast-regional-radar"
			}
		],
		"offset" : -15 ,
		"limit" : 15,
		"verbose" : true,
		"units" : {
			"distance" : "km",
			"speed" : "knots"
		}
	}
}
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
		"typeId": 5,
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
        },
        "status": {
            "code": "active",
            "description": [{
                "text": "Currently active.",
                "meta": "text"
            }]
        },
        "nextIssueDateTime": "2016-03-26 22:48:00"
    }
]
```

Returns all map providers that can be filtered by `mapType`.

### Request

`GET api.willyweather.com.au/v2/{api key}/maps.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapTypes | csv | `regional-radar`, `radar`, `forecast-regional-radar`, `satellite`, `synoptic`, `temperature`, `wind`, `rainfall`, `swell`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `thunderstorms`, `lightning`, `fog`, `frost`, `mixing-height`, `drought-factor`, `cyclone` | **see <a href="/#map-types">Map Types</a> for conversion of typeId** | true
lat | double | | latitude | false
lng | double | | longitude | false
verbose | boolean | | include overlay images with the response | false
offset | int | | minutes that overlay images should start from | true
limit | int | | minutes that overlay images should end at | false
dataPoints | boolean | | used in conjunction with the lat and lng parameters and when set to true, will include raw data values in the response for the provided map type. | false
units | csv | See <a href="#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
<code>offset</code> is required if <code>verbose</code> is <code>true</code>, which is the default if not included in the request.
</aside>

<aside class="notice">
When <code>lat</code> and <code>lng</code> are provided the returned Map Providers will be the closest ones to these coordinates, that also support the specified <code>mapTypes</code>.
</aside>

<aside class="notice">
<code>dataPoints</code> is only supported by forecast-regional-radar and when used <code>mapTypes</code> must equal <code>forecast-regional-radar</code> and requires both <code>lat</code> and <code>lng</code> parameters.
</aside>

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
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
mapLegend | object | | The legend describing the values of the data **(see Map Legend)**
status | object | | the status of the map provider **(see Status)**
nextIssueDateTime | string | | the estimated time when new data will be made available (`YYYY-MM-DD HH:MM:SS`)

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

### Map Legend

The legend describing the values of the data.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
keys | array | | an array of key objects **(see Map Legend - Key)**

### Map Legend - Key

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
colour | string | | hexadecimal colour code
range | object | | **(see Map Legend - Range)**
label | string | | 

### Map Legend - Range

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
min | float | | min value of the range
max | float | | max value of the range
 
### Data Points

Data points will only be included when it is not empty.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | YYYY-MM-DD HH:MM:SS
lat | int | | latitude
lng | int | | longitude
amount | int | | value of the rainfall
intensity | int | | intensity of rainfall


### Map Types
* regional rain radar : 1
* national rain radar : 3
* forecast regional rain radar : 5
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

### Status

The map provider of the map provider with code and description.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
code | string | `active`, `inactive`, `redirected` | indicates the current status of the map provider. <p>`active` - The map provider has at least 3 map overlays within the last hour.</p> <p>`inactive` - The map provider has less than 3 map overlays for the last hour.</p> <div>`redirected` - The map provider does not have more than 3 map overlays within the last hour so a backup map provider is provided.</div>
description | array | | an array of description objects. **(see Status - Description)**

### Status - Description

A description object forms part of the overall description of the map providers current state.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
text | string | | the description text
meta | string | `name`, `text`, `date` | the meta value provides information which describes the data


## Map Provider - GET - Map Data By Provider

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/maps/4.json?mapTypes=regional-radar&offset=-60&limit=30&units=distance:km
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"mapTypes" :[
			{
				"code" : "regional-radar"
			}, {
				"code" : "synoptic"
			}
		],
		"offset" : -15 ,
		"limit" : 15,
		"verbose" : true,
		"units" : {
			"distance" : "km",
			"speed" : "knots"
		}
	}
}
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
    },
    "status": {
        "code": "inactive",
        "description": [{
            "text": "Newcastle",
            "meta": "name"
        }, {
            "text": " has been offline for ",
            "meta": "text"
        }, {
            "text": "2 days",
            "meta": "date"
        }, {
            "text": " due to scheduled maintenance, it is expected to be restored in ",
            "meta": "text"
        }, {
            "text": "4 hours",
            "meta": "date"
        }, {
            "text": ".",
            "meta": "text"
        }]
    },
    "nextIssueDateTime": "2014-03-26 22:48:00"
}
```

Get a map provider with overlay data.

### Request

`GET api.willyweather.com.au/v2/{api key}/maps/{provider id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapTypes | csv | `regional-radar`, `radar`, `forecast-regional-radar`, `satellite`, `synoptic`, `temperature`, `wind`, `rainfall`, `swell`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `thunderstorms`, `lightning`, `fog`, `frost`, `mixing-height`, `drought-factor`, `cyclone` | **<a href="/#map-provider-get-get-map-providers">Get Map Providers</a>** | true
offset | int | | minutes that overlay images should start from | true
limit | int | | minutes that overlay images should end at | false
units | csv | See <a href="#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is a Map Provider. See <a href="#map-provider-get-get-map-providers">Get Map Providers</a> for description of a Map Provider response.

## Map Provider - GET - Map Data By Location Id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/2919/maps.json?mapTypes=regional-radar&offset=-60&limit=30&units=distance:km
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"mapTypes" :[
			{
				"code" : "regional-radar"
			}, {
				"code" : "synoptic"
			}
		],
		"offset" : -15 ,
		"limit" : 15,
		"verbose" : true,
		"units" : {
			"distance" : "km",
			"speed" : "knots"
		}
	}
}
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
        },
        "status": {
            "code": "active",
            "description": [{
                "text": "Newcastle",
                "meta": "name"
            }, {
                "text": " is currently offline. ",
                "meta": "text"
            }, {
                "text": "Sydney (Terrey Hills)",
                "meta": "name"
            }, {
                "text": " is the backup radar.",
                "meta": "text"
            }]
        },
        "nextIssueDateTime": "2014-03-26 22:48:00"
    }
]
```

Get map providers linked to a location.

### Request

`GET api.willyweather.com.au/v2/locations/{location id}/maps.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapTypes | csv | `regional-radar`, `radar`, `forecast-regional-radar`, `satellite`, `synoptic`, `temperature`, `wind`, `rainfall`, `swell`, `uv`, `apparent-temperature`, `dew-point`, `relative-humidity`, `cloud-cover`, `thunderstorms`, `lightning`, `fog`, `frost`, `mixing-height`, `drought-factor`, `cyclone` | **<a href="/#get-map-providers">Get Map Providers</a>** | true
offset | int | | minutes that overlay images should start from | true
limit | int | | minutes that overlay images should end at | false
units | csv | See <a href="#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an array of Map Providers. See <a href="#map-provider-get-get-map-providers">Get Map Providers</a> for a description of a Map Provider response.

# Radar Stations

## Radar Station - GET - Get Radar Station

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/radar-stations/{id}.json
```

> Example Request Header

```json
{}
```

> Example Response

```json
{
    "radarStation": {
        "id": 58,
        "name": "South Doodlakine",
        "lat": -31.777,
        "lng": 117.953,
        "timeZone": "Australia/Perth"
    }
}
```

Returns the details of a Radar Station.

### Request

`GET api.willyweather.com.au/v2/{api key}/radar-stations/{id}.json`

### Response

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
radarStation | object | | **(see Radar Station)**


### Radar Station

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
timeZone | string | |


## Radar Station - GET - Get Closest Radar Station

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/radar-stations.json?lat=-32.25914030013808&lng=116.53787876954023&units=distance:miles
```

> Example Request Header

```json
{
    "CONTENT_TYPE": "application/json",
    "HTTP_X_PAYLOAD": {
        "lat": 32.25914030013808,
        "lng": 116.53787876954023,
        "units": {
          "distance": "miles"
        }
    }
}
```

> Example Response

```json
{
    "radarStation": {
        "id": 58,
        "name": "South Doodlakine",
        "lat": -31.777,
        "lng": 117.953,
        "timeZone": "Australia/Perth",
        "distance": 89.4,
        "units": {
          "distance": "miles"
        }
    }
}
```

Returns the details of closest Radar Station.

### Request

`GET api.willyweather.com.au/v2/{api key}/radar-stations.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
lat | float | | | true
lng | float | | | true
units | csv | See <a href="#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
radarStation | object | | **(see Radar Station)**


### Radar Station

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
timeZone | string | |
distance | double | | distance of radar station from the provided lat and lng
units | object | | includes unit of measurement for distance


## Graph - GET - Forecast Graphs - Rainfall: Specific radar station data source

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/radar-stations/{id}.json?forecastGraphs=rainfall&lat=-32.25914030013808&lng=116.53787876954023&units=distance:miles
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["rainfall"],
		"lat": 32.25914030013808,
		"lng": 116.53787876954023,
		"units": {
			"distance" : "miles"
		}
	}
}
```

> Example Response

```json
{
  "radarStation": {
    "id": 58,
    "name": "South Doodlakine",
    "lat": -31.777,
    "lng": 117.953,
    "timeZone": "Australia/Perth",
    "distance": 89.4,
    "units": {
      "distance": "miles"
    }
  },
  "forecastGraphs": {
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
          "yAxisDataMin": 2,
          "yAxisDataMax": 6,
          "yAxisMin": 1,
          "yAxisMax": 7,
          "groups": [{
            "dateTime": 1604188800,
            "points": [{
              "x": 1604211480,
              "y": 4
            }, {
              "x": 1604211840,
              "y": 3
            }, {
              "x": 1604212200,
              "y": 4
            }, {
              "x": 1604212560,
              "y": 4
            }, {
              "x": 1604212920,
              "y": 4
            }, {
              "x": 1604213280,
              "y": 5
            }, {
              "x": 1604213640,
              "y": 4
            }, {
              "x": 1604214000,
              "y": 4
            }, {
              "x": 1604214360,
              "y": 3
            }, {
              "x": 1604214720,
              "y": 4
            }, {
              "x": 1604215080,
              "y": 4
            }, {
              "x": 1604215440,
              "y": 4
            }, {
              "x": 1604215800,
              "y": 3
            }, {
              "x": 1604216160,
              "y": 2
            }, {
              "x": 1604216520,
              "y": 3
            }, {
              "x": 1604216880,
              "y": 5
            }, {
              "x": 1604217240,
              "y": 5
            }, {
              "x": 1604217600,
              "y": 6
            }, {
              "x": 1604217960,
              "y": 5
            }, {
              "x": 1604218320,
              "y": 4
            }, {
              "x": 1604218680,
              "y": 3
            }, {
              "x": 1604219040,
              "y": 3
            }, {
              "x": 1604219400,
              "y": 2
            }]
          }],
          "controlPoints": {
            "pre": null,
            "post": null
          },
          "controlPoint": null
        },
        "xAxisMin": 1604211480,
        "xAxisMax": 1604219400
      },
      "carousel": {
        "size": 1,
        "start": 1
      },
      "issueDateTime": "2020-11-01 00:30:00",
      "provider": {
        "id": 58,
        "name": "South Doodlakine",
        "lat": -31.777,
        "lng": 117.953,
        "distance": 89.4,
        "units": {
          "distance": "miles"
        }
      }
    }
  }
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/radar-stations/{id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
lat | float | | | true
lng | float | | | true
forecastGraphs | array | Only `rainfall` can be specified | | false
units | csv | See <a href="/#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
radarStation | object | | **(see Radar Station below)**
forecastGraphs | array | | **(see Forecast Graph below)**

### Radar Station

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
timeZone | string | |
distance | double | | distance of radar station from the provided lat and lng
units | object | | includes unit of measurement for distance

### Forecast Graph

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dataConfig | object | | **(see Data Config below)**
carousel | object | | **(see Carousel below)**
issueDateTime | object | | `YYYY-MM-DD HH:MM:SS`
provider | object | | **(see Provider)**

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

### Provider

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
distance | double | | distance of radar station from the provided lat and lng
units | object | | includes unit of measurement for distance

## Graph - GET - Forecast Graphs - Rainfall: Closest radar station data source

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/radar-stations.json?forecastGraphs=rainfall&lat=-32.25914030013808&lng=116.53787876954023&units=distance:miles
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["rainfall"],
		"lat": 32.25914030013808,
		"lng": 116.53787876954023,
		"units": {
			"distance" : "miles"
		}
	}
}
```

> Example Response

```json
{
  "radarStation": {
    "id": 58,
    "name": "South Doodlakine",
    "lat": -31.777,
    "lng": 117.953,
    "timeZone": "Australia/Perth",
    "distance": 89.4,
    "units": {
      "distance": "miles"
    }
  },
  "forecastGraphs": {
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
          "yAxisDataMin": 2,
          "yAxisDataMax": 6,
          "yAxisMin": 1,
          "yAxisMax": 7,
          "groups": [{
            "dateTime": 1604188800,
            "points": [{
              "x": 1604211480,
              "y": 4
            }, {
              "x": 1604211840,
              "y": 3
            }, {
              "x": 1604212200,
              "y": 4
            }, {
              "x": 1604212560,
              "y": 4
            }, {
              "x": 1604212920,
              "y": 4
            }, {
              "x": 1604213280,
              "y": 5
            }, {
              "x": 1604213640,
              "y": 4
            }, {
              "x": 1604214000,
              "y": 4
            }, {
              "x": 1604214360,
              "y": 3
            }, {
              "x": 1604214720,
              "y": 4
            }, {
              "x": 1604215080,
              "y": 4
            }, {
              "x": 1604215440,
              "y": 4
            }, {
              "x": 1604215800,
              "y": 3
            }, {
              "x": 1604216160,
              "y": 2
            }, {
              "x": 1604216520,
              "y": 3
            }, {
              "x": 1604216880,
              "y": 5
            }, {
              "x": 1604217240,
              "y": 5
            }, {
              "x": 1604217600,
              "y": 6
            }, {
              "x": 1604217960,
              "y": 5
            }, {
              "x": 1604218320,
              "y": 4
            }, {
              "x": 1604218680,
              "y": 3
            }, {
              "x": 1604219040,
              "y": 3
            }, {
              "x": 1604219400,
              "y": 2
            }]
          }],
          "controlPoints": {
            "pre": null,
            "post": null
          },
          "controlPoint": null
        },
        "xAxisMin": 1604211480,
        "xAxisMax": 1604219400
      },
      "carousel": {
        "size": 1,
        "start": 1
      },
      "issueDateTime": "2020-11-01 00:30:00",
      "provider": {
        "id": 58,
        "name": "South Doodlakine",
        "lat": -31.777,
        "lng": 117.953,
        "distance": 89.4,
        "units": {
          "distance": "miles"
        }
      }
    }
  }
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/radar-stations.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
lat | float | | | true
lng | float | | | true
forecastGraphs | array | Only `rainfall` can be specified | | false
units | csv | See <a href="/#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is a Radar Station and an array of Forecast graphs. See <a href="#graph-get-forecast-graphs-rainfall-specific-radar-station-data-source">Forecast Graphs - Rainfall: Specific radar station data source</a> for response.

## Graph - GET - Observational Graphs - Rainfall: Specific radar station data source

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/radar-stations/{id}.json?observationalGraphs=rainfall&lat=-32.25914030013808&lng=116.53787876954023&units=distance:miles
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["rainfall"],
		"lat": 32.25914030013808,
		"lng": 116.53787876954023,
		"units": {
			"distance" : "miles"
		}
	}
}
```

> Example Response

```json
{
  "radarStation": {
    "id": 58,
    "name": "South Doodlakine",
    "lat": -31.777,
    "lng": 117.953,
    "timeZone": "Australia/Perth",
    "distance": 89.4,
    "units": {
      "distance": "miles"
    }
  },
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
          "yAxisDataMin": 2,
          "yAxisDataMax": 6,
          "yAxisMin": 1,
          "yAxisMax": 7,
          "groups": [{
            "dateTime": 1604188800,
            "points": [{
              "x": 1604211480,
              "y": 4
            }, {
              "x": 1604211840,
              "y": 3
            }, {
              "x": 1604212200,
              "y": 4
            }, {
              "x": 1604212560,
              "y": 4
            }, {
              "x": 1604212920,
              "y": 4
            }, {
              "x": 1604213280,
              "y": 5
            }, {
              "x": 1604213640,
              "y": 4
            }, {
              "x": 1604214000,
              "y": 4
            }, {
              "x": 1604214360,
              "y": 3
            }, {
              "x": 1604214720,
              "y": 4
            }, {
              "x": 1604215080,
              "y": 4
            }, {
              "x": 1604215440,
              "y": 4
            }, {
              "x": 1604215800,
              "y": 3
            }, {
              "x": 1604216160,
              "y": 2
            }, {
              "x": 1604216520,
              "y": 3
            }, {
              "x": 1604216880,
              "y": 5
            }, {
              "x": 1604217240,
              "y": 5
            }, {
              "x": 1604217600,
              "y": 6
            }, {
              "x": 1604217960,
              "y": 5
            }, {
              "x": 1604218320,
              "y": 4
            }, {
              "x": 1604218680,
              "y": 3
            }, {
              "x": 1604219040,
              "y": 3
            }, {
              "x": 1604219400,
              "y": 2
            }]
          }],
          "controlPoints": {
            "pre": null,
            "post": null
          },
          "controlPoint": null
        },
        "xAxisMin": 1604211480,
        "xAxisMax": 1604219400
      },
      "carousel": {
        "size": 1,
        "start": 1
      },
      "issueDateTime": "2020-11-01 00:30:00",
      "provider": {
        "id": 58,
        "name": "South Doodlakine",
        "lat": -31.777,
        "lng": 117.953,
        "distance": 89.4,
        "units": {
          "distance": "miles"
        }
      }
    }
  }
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/radar-stations/{id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
lat | float | | | true
lng | float | | | true
observationalGraphs | array | Only `rainfall` can be specified | | false
units | csv | See <a href="/#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
radarStation | object | | **(see Radar Station below)**
observationGraphs | array | | **(see Observational Graph below)**

### Radar Station

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
timeZone | string | |
distance | double | | distance of radar station from the provided lat and lng
units | object | | includes unit of measurement for distance

### Observational Graph

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dataConfig | object | | **(see Data Config below)**
carousel | object | | **(see Carousel below)**
issueDateTime | object | | `YYYY-MM-DD HH:MM:SS`
provider | object | | **(see Provider)**

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

### Provider

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
distance | double | | distance of radar station from the provided lat and lng
units | object | | includes unit of measurement for distance

## Graph - GET - Observational Graphs - Rainfall: Closest radar station data source

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/radar-stations.json?observationalGraphs=rainfall&lat=-32.25914030013808&lng=116.53787876954023&units=distance:miles
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["rainfall"],
		"lat": 32.25914030013808,
		"lng": 116.53787876954023,
		"units": {
			"distance" : "miles"
		}
	}
}
```

> Example Response

```json
{
  "radarStation": {
    "id": 58,
    "name": "South Doodlakine",
    "lat": -31.777,
    "lng": 117.953,
    "timeZone": "Australia/Perth",
    "distance": 89.4,
    "units": {
      "distance": "miles"
    }
  },
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
          "yAxisDataMin": 2,
          "yAxisDataMax": 6,
          "yAxisMin": 1,
          "yAxisMax": 7,
          "groups": [{
            "dateTime": 1604188800,
            "points": [{
              "x": 1604211480,
              "y": 4
            }, {
              "x": 1604211840,
              "y": 3
            }, {
              "x": 1604212200,
              "y": 4
            }, {
              "x": 1604212560,
              "y": 4
            }, {
              "x": 1604212920,
              "y": 4
            }, {
              "x": 1604213280,
              "y": 5
            }, {
              "x": 1604213640,
              "y": 4
            }, {
              "x": 1604214000,
              "y": 4
            }, {
              "x": 1604214360,
              "y": 3
            }, {
              "x": 1604214720,
              "y": 4
            }, {
              "x": 1604215080,
              "y": 4
            }, {
              "x": 1604215440,
              "y": 4
            }, {
              "x": 1604215800,
              "y": 3
            }, {
              "x": 1604216160,
              "y": 2
            }, {
              "x": 1604216520,
              "y": 3
            }, {
              "x": 1604216880,
              "y": 5
            }, {
              "x": 1604217240,
              "y": 5
            }, {
              "x": 1604217600,
              "y": 6
            }, {
              "x": 1604217960,
              "y": 5
            }, {
              "x": 1604218320,
              "y": 4
            }, {
              "x": 1604218680,
              "y": 3
            }, {
              "x": 1604219040,
              "y": 3
            }, {
              "x": 1604219400,
              "y": 2
            }]
          }],
          "controlPoints": {
            "pre": null,
            "post": null
          },
          "controlPoint": null
        },
        "xAxisMin": 1604211480,
        "xAxisMax": 1604219400
      },
      "carousel": {
        "size": 1,
        "start": 1
      },
      "issueDateTime": "2020-11-01 00:30:00",
      "provider": {
        "id": 58,
        "name": "South Doodlakine",
        "lat": -31.777,
        "lng": 117.953,
        "distance": 89.4,
        "units": {
          "distance": "miles"
        }
      }
    }
  }
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/radar-stations.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
lat | float | | | true
lng | float | | | true
observationalGraphs | array | Only `rainfall` can be specified | | false
units | csv | See <a href="/#units">Units</a>. Only distance can be specified | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is a Radar Station and an array of Observational graphs. See <a href="#graph-get-observational-graphs-rainfall-specific-radar-station-data-source">Observational Graphs - Rainfall: Specific radar station data source</a> for response.

# Regions

## Region - GET - by Region id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/regions/{region id}.json
```

> Example Request Header

```json
{}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

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


## Region - GET - All Regions

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/regions.json
```

> Example Request Header

```json
{}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response
Response is an array of Regions. See <a href="#region-get-by-region-id">Get Region</a> for a description of a Region response.

## Region - GET - by State id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/states/{state id}/regions.json
```

> Example Request Header

```json
{}
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
Response is an array of Regions. See <a href="#region-get-by-region-id">Get Region</a> for a description of a Region response.

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

# Search

## Location - GET - Search By Query

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/search.json?query=beach&limit=2
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"query": "bateman",
		"limit": 1
	}
}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an array of Locations. See <a href="#location-get-by-location-id">Locations</a> for description of a Location response.

## Location - GET - Search By Coordinates

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/search.json?lat=-33.89&lng=151.27&range=5&units=distance:km
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"lat": -33.8996141,
		"lng": 151.272962,
        "range": 5,
		"units": {
			"distance": "km"
		}
	}
}
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
range | integer | | restrict the search to a specific radius. | false
units | csv | See <a href="/#units">Units</a>. Only distance can be specified | | true

<aside class="notice">
    Parameter <code>range</code> unit conversion is based on the distance unit specified in the <code>units</code> parameter</strong>.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

A single Location and units. See <a href="#location-get-by-location-id">Location</a> for a description of a Location response. See <a href="#units">Units</a> for a description of a Units response.

## Location - GET - Get Closest Locations

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/search/closest.json?id=4988&weatherTypes=swell,tides,general&units=distance:km
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"location" : {
			"id" : 1
		},
		"weatherTypes" : [
			{
				"code" : "general"
			}, {
				"code" : "tides"
			}
		],
		"units" : {
			"distance" : "km"
		}
	}
}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

An array of location objects, broken up into their weather types. See <a href="#location-get-by-location-id">Locations</a> for a description of a Location response.

# States

##  State - GET - by State id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/states/{state id}.json
```

> Example Request Header

```json
{}
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
`GET api.willyweather.com.au/api/v2/states/{state id}.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

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

## State - GET - All States

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/states.json
```

> Example Request Header

```json
{}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response
Response is an array of States. See <a href="#state-get-by-state-id">Get State</a> for description of a State response.

# Warnings

## Warning - GET - All Warnings

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/warnings.json?classifications=storm
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"classifications": ["storm", "flood"],
		"verbose": true
	}
}
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
			"classification": "storm",
			"warningSeverityLevels": [
				{
					"id": 1,
					"code": "yellow"
				},
				{
					"id": 2,
					"code": "amber"
				},
				{
					"id": 3,
					"code": "red"
				}
			]
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
			"classification": "storm",
			"warningSeverityLevels": [
				{
					"id": 1,
					"code": "yellow"
				},
				{
					"id": 2,
					"code": "amber"
				},
				{
					"id": 3,
					"code": "red"
				}
			]
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

An array of Warning objects, see <a href="#warning-get-by-warning-code">Warning</a> for a description of a Warning response.

## Warning - GET - by Warning code

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/warnings/IDQ20870.json
```

> Example Request Header

```json
{}
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
		"classification": "flood",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

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
warningSeverityLevels | array | | **(see Warning Severity Level object)**

### Warning Severity Level

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1` for yellow, `2` for amber, `3` for red |
code | string | `yellow`, `amber`, `red`|

### Content

Warnings will occasionally come pre-formatted in html, as well as in plain text

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
text | string | |
html | string | |

## Warning - GET - All Warnings By Area

> Example Query String Request 

```
https://api.willyweather.com.au/v2/{api key}/locations/5381/warnings.json?classifications=storm,flood&area=location&verbose=false
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"classifications": ["storm", "flood"],
		"area": "location",
		"verbose": true
	}
}
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
			"classification": "flood",
			"warningSeverityLevels": [
				{
					"id": 1,
					"code": "yellow"
				},
				{
					"id": 2,
					"code": "amber"
				},
				{
					"id": 3,
					"code": "red"
				}
			]
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
			"classification": "flood",
			"warningSeverityLevels": [
				{
					"id": 1,
					"code": "yellow"
				},
				{
					"id": 2,
					"code": "amber"
				},
				{
					"id": 3,
					"code": "red"
				}
			]
		}
	}
]
```
There are three ways to request Warnings by area.

* **Location**
* **Region**
* **State**

<aside class="notice">
Get <a href="#locations">Locations</a>, <a href="#states">States</a> and <a href="#regions">Regions</a>
</aside>

These will all return an array of Warnings, filtered by classification.

### Requests

`GET api.willyweather.com.au/v2/{api key}/locations/{location id}/warnings.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
classifications | csv | `avalanche`, `blizzard`, `closed-water`, `cold`, `cold-rain`, `dust-smoke-pollution`, `earthquake`, `farming`, `fire`, `flood`, `fog`, `frost`, `fruit-disease`, `general`, `hazmat`, `heat`, `hiking`, `hurricane`, `leaf-disease`, `marine`, `road`, `sheep`, `snow`, `storm`, `strong-wind`, `surf`, `tornado`, `tsunami`, `typhoon`, `volcano`, `wind-chill` | the classifications are a fixed list and all new warnings fit an existing classification | false
verbose | boolean |  | include the content attribute with the response | false
area | string | `location`, `region`, `state` | | false

<aside class="notice">
    <code>area = location</code> by default.
</aside>

### Response

An array of Warning objects, see <a href="#warning-get-by-warning-code">Warning</a> for a description of a Warning response.

## Warning Type - GET - All Warning Types

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/warning-types.json
```

> Example Request Header

```json
{}
```

> Example Response

```json
[
	{
		"id": 1,
		"code": "severe-weather",
		"name": "Severe Weather",
		"classification": "storm",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 2,
		"code": "strong-winds",
		"name": "Strong Wind",
		"classification": "strong-wind",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 3,
		"code": "thunderstorms",
		"name": "Thunderstorm",
		"classification": "storm",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 4,
		"code": "floods",
		"name": "Flood",
		"classification": "flood",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 5,
		"code": "roads",
		"name": "Road",
		"classification": "road",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 6,
		"code": "fire",
		"name": "Fire",
		"classification": "fire",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 7,
		"code": "heat",
		"name": "Heat",
		"classification": "heat",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 8,
		"code": "frosts",
		"name": "Frost",
		"classification": "frost",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 9,
		"code": "cyclones",
		"name": "Cyclone",
		"classification": "hurricane",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 10,
		"code": "tsunamis",
		"name": "Tsunami",
		"classification": "tsunami",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 11,
		"code": "storm-tide",
		"name": "Storm tide",
		"classification": "tsunami",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 12,
		"code": "earthquakes",
		"name": "Earthquake",
		"classification": "earthquake",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 13,
		"code": "bushwalking",
		"name": "Bushwalking",
		"classification": "hiking",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 14,
		"code": "haze",
		"name": "Haze",
		"classification": "dust-smoke-pollution",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 15,
		"code": "sheep-grazier",
		"name": "Sheep Grazier",
		"classification": "sheep",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	}, 
	{
		"id": 16,
		"code": "brown-rot",
		"name": "Brown Rot",
		"classification": "fruit-disease",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 17,
		"code": "downy-mildew",
		"name": "Downy Mildew",
		"classification": "leaf-disease",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 18,
		"code": "farmers-graziers",
		"name": "Farmer\/Grazier",
		"classification": "farming",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 19,
		"code": "announcements",
		"name": "Important Announcement",
		"classification": "general",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 20,
		"code": "small-boat",
		"name": "Small Boat",
		"classification": "marine",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 21,
		"code": "closed-waters",
		"name": "Closed Water",
		"classification": "closed-water",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 22,
		"code": "flood-summary",
		"name": "Flood Summary (state wide)",
		"classification": "flood",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 23,
		"code": "rain",
		"name": "Rain",
		"classification": "cold-rain",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 24,
		"code": "snow",
		"name": "Snow",
		"classification": "snow",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 25,
		"code": "fog",
		"name": "Fog",
		"classification": "fog",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	},
	{
		"id": 26,
		"code": "ice",
		"name": "Ice",
		"classification": "frost",
		"warningSeverityLevels": [
			{
				"id": 1,
				"code": "yellow"
			},
			{
				"id": 2,
				"code": "amber"
			},
			{
				"id": 3,
				"code": "red"
			}
		]
	}
]
```

Returns all warnings types.

### Request

`GET api.willyweather.com.au/v2/{api key}/warning-types.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

An array of Warning Type objects, see <a href="#warning-types">Warning Types</a> for a description of a Warning Type response.

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
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 7</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>


### Forecast Response

A forecast response consists of an object for each `weatherType` requested. Each will contain the array `days`, which contains the forecast data (usually a `dateTime` and `entries`, the latter consisting of an array of forecast days). See each specific section for details on the response information unique to each weather type.

> Example Large Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=weather,wind&forecastGraphs=temperature,precis&observationalGraphs=pressure,temperature&observational=true&regionPrecis=true&days=1&startDate=2016-03-26
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

## Forecast - GET - Moon Phases

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=moonphases&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["moonphases"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			],
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - Precis

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=precis&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["precis"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
Unlike <a href="#forecast-get-weather">Weather</a>, a Precis forecast can provide multiple values throughout a day (e.g. every 3 hours).

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
precisCode | string | `fine`, `mostly-fine`, `high-cloud`, `partly-cloudy`, `mostly-cloudy`, `cloudy`, `overcast`, `shower-or-two`, `chance-shower-fine`, `chance-shower-cloud`, `drizzle`, `few-showers`, `showers-rain`, `heavy-showers-rain`, `chance-thunderstorm-fine`, `chance-thunderstorm-cloud`, `chance-thunderstorm-showers`, `thunderstorm`, `chance-snow-fine`, `chance-snow-cloud`, `snow-and-rain`, `light-snow`, `snow`, `heavy-snow`, `wind`, `frost`, `fog`, `hail`, `dust` |
precis | string | | formal, formatted name for precisCode (e.g. "Partly Cloudy")
precisOverlayCode | string | `wind`, `frost`, `fog`, `hail` | this gives a second level of detail to the precis, we use it to show a small icon on top of the existing image. can be `null`
night | boolean | | 

## Forecast - GET - Rainfall

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=rainfall&days=1&startDate=2014-03-27
```

> Example Request Body

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["rainfall"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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

A Rainfall forecast contains a daily summary of rain, in contrast to a <a href="#forecast-get-rainfall-probability">Rainfall Probability</a> forecast which will give periodic probability values throughout each day.

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
The daily probabilty for rainfall will not align with the probability included in <a href="#forecast-get-rainfall-probability">Rainfall Probability</a>
</aside>

## Forecast - GET - Rainfall Probability

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=rainfallprobability&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{	"CONTENT_TYPE": "application/json",
 	"HTTP_X_PAYLOAD": {
		"forecasts": ["rainfallprobability"],
		"days": 1,
		"startDate": "2014-03-27"
 	}
}
```

> Example Response

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
    },
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
			"issueDateTime": "2014-03-27 08:55:30",
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

A Rainfall Probability forecast contains periodic probability forecasts throughout a day, in contrast to a <a href="#forecast-get-rainfall">Rainfall</a> forecast which returns a summary for an entire day.

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - Sunrise/Sunset

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=sunrisesunset&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["sunrisesunset"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```
> Example Response

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
    },
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
			],
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - Swell

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecasts=swell&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["swell"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"issueDateTime": "2014-03-26 16:23:30",
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - Temperature

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=temperature&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["temperature"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"issueDateTime": "2014-03-27 08:35:13",
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

A Temperature forecast contains periodic temperature forecasts throughout a day, in contrast to <a href="#forecast-get-weather">Weather</a> which provides a daily summary with min/max values.

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - Tides

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecasts=tides&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["tides"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"issueDateTime": "2014-03-27 00:53:19",
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - UV

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=uv&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["uv"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"issueDateTime": "2014-03-27 05:02:10",
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Forecast - GET - Weather

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=weather&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["weather"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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

## Forecast - GET - Wind

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/1215/weather.json?forecasts=wind&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecasts": ["wind"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"issueDateTime": "2014-03-27 10:02:44",
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Precis

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=precis&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["precis"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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

## Graph - GET - Forecast Graphs - Rainfall Probability

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=rainfallprobability&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["rainfallprobability"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"nextIssueDateTime": "2014-03-27 22:55:30",
			"carousel": {
				"size": 1,
				"start": 1
			}
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Sunrise/Sunset

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=sunrisesunset&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["sunrisesunset"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			},
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Swell Height

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=swell-height&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["swell-height"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"nextIssueDateTime": "2014-03-27 16:23:30",
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

Swell Height forecast graph series, can be plotted on the same graph as a <a href="#graph-get-forecast-graphs-swell-period">Swell Period</a> graph.

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Swell Period

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=swell-period&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["swell-period"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"nextIssueDateTime": "2014-03-27 16:23:30",
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

Swell Period forecast graph series, can be plotted on the same graph as a <a href="#graph-get-forecast-graphs-swell-height">Swell Height</a> graph.

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Temperature

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=temperature&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["temperature"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"nextIssueDateTime": "2014-03-27 09:35:13",
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Tides

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=tides&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["tides"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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

## Graph - GET - Forecast Graphs - UV

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=uv&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["uv"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"nextIssueDateTime": "2014-03-28 05:02:10",
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Forecast Graphs - Wind

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?forecastGraphs=wind&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"forecastGraphs": ["wind"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			"nextIssueDateTime": "2014-03-27 22:02:44",
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current forecast

## Graph - GET - Observational Graphs - Apparent Temperature

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=apparent-temperature&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["apparent-temperature"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
	"observationalGraphs": {
		"apparent-temperature": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "apparent-temperature",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "AppareTemperaturePointFormatter"
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
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

The temperature equivalent perceived by humans, caused by the combined effects of air temperature, relative humidity and wind speed.

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
id | string | `apparent-temperature` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `ApparentTemperaturePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
x | int | | time value
y | double | | apparent temperature

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Cloud

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=cloud&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["cloud"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
	"observationalGraphs": {
		"cloud": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "cloud",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "CloudPointFormatter"
					},
					"yAxisDataMin": 1,
					"yAxisDataMax": 8,
					"yAxisMin": 0,
					"yAxisMax": 10,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 1
								},
								{
									"x": 1395891000,
									"y": 4
								},
								{
									"x": 1395892800,
									"y": 8
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878399,
                            "y": 1
                        }, 
                        "post": null
                    }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
			    "cloud": "oktas"
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
			},
			"carousel": {
				"size": 1,
				"start": 1
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
id | string | `cloud` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `CloudPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | int | | cloud

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Delta T

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=delta-t&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["delta-t"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
	"observationalGraphs": {
		"delta-t": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "delta-t",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "DeltaTPointFormatter"
					},
					"yAxisDataMin": 12.3,
					"yAxisDataMax": 32.4,
					"yAxisMin": 0,
					"yAxisMax": 40,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 32.4
								},
								{
									"x": 1395891000,
									"y": 12.3
								},
								{
									"x": 1395892800,
									"y": 25.89
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878399,
                            "y": 16.7
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
			},
			"carousel": {
				"size": 1,
				"start": 1
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
id | string | `delta-t` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `DeltaTPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | delta t

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Dew Point

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=dew-point&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["dew-point"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
									"y": 18.6
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
				"id": 733,
				"name": "Wedding Cake West",
				"lat": -33.84,
				"lng": 151.26,
				"distance": 3.6,
				"units": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Humidity

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=humidity&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["humidity"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
	"observationalGraphs": {
		"humidity": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "humidity",
						"color": "#FFF",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "HumidityPointFormatter"
					},
					"yAxisDataMin": 20,
					"yAxisDataMax": 80,
					"yAxisMin": 0,
					"yAxisMax": 100,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 20
								},
								{
									"x": 1395891000,
									"y": 50
								},
								{
									"x": 1395892800,
									"y": 80
								}
							]
						}
					],
					"controlPoints": {
				        "pre": {
				            "x": 1395878399,
				            "y": 30
				        },
                        "post": null
					}
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "percentage": "%"
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
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

The amount of water vapor in the atmosphere or a gas.

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
id | string | `humidity` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` HumidityPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | int | | time value
points | array | | array of `point` objects **(see Point below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | humidity

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Pressure

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=pressure&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["pressure"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			},
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Rainfall

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=rainfall&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["rainfall"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
			},
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Temperature

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=temperature&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["temperature"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Wind

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=wind&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["wind"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Wind Gust

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observationalGraphs=wind-gust&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["wind-gust"],
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
	"observationalGraphs": {
		"wind-gust": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "wind-gust",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "WindGustPointFormatter"
					},
					"yAxisDataMin": 10,
					"yAxisDataMax": 75,
					"yAxisMin": 0,
					"yAxisMax": 80,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 10,
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								},
								{
									"x": 1395891000,
									"y": 40,
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								},
								{
									"x": 1395892800,
									"y": 75,
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
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"units": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
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
id | string | `wind-gust` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string | `WindGustPointFormatter` |

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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Observational - GET - by Location id

Observational provides real time data from one or more weather stations.

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?observational=true&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observational": true,
	    "days": 1,
	    "startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
    "observational": {
        "observations": {
            "temperature": {
                "temperature": 19.3,
                "apparentTemperature": 12.3,
                "trend": 1
            },
            "delta-t": {
                "temperature": 28,
                "trend": 0
            },
            "cloud": {
                "oktas": 3,
                "trend": 0
            },
            "humidity": {
                "percentage": 26,
                "trend": -1
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
			"deltaT": {
				"id": 377,
				"name": "Sydney (Observatory Hill)",
                "lat": -33.86,
                "lng": 151.21,
                "distance": 6.9
			},
			"cloud": {
				"id": 377,
				"name": "Sydney (Observatory Hill)",
                "lat": -33.86,
                "lng": 151.21,
                "distance": 6.9
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
delta-t | object | |
cloud | object | |
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

### Delta T
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | double |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising

### Cloud
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
oktas | int | |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising

### Humidity
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
percentage | double | |
trend | int | `-1`, `0`, `1` | -1 is falling. 0 is steady. 1 is rising

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

## Region Precis - GET - by Location id

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/locations/4988/weather.json?regionPrecis=true&days=1&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"regionPrecis": true,
		"days": 1,
		"startDate": "2014-03-27"
	}
}
```

> Example Response

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
    },
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

## Summary - GET - by Location ids

> Example Query String Request

```shell
https://api.willyweather.com.au/v2/{api key}/weather/summaries.json?ids=16
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"locations" : [
			{
				"id" : 1
			}, {
				"id" : 2
			}
		],
		"units" : {
			"temperature" : "c"
		}
	}
}
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

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

### Summary

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
location | object | | 
forecasts | object | | 
observational | object | |

### Location

See <a href="#location-get-by-location-id">Locations</a> for description a of Location response.

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

# Weather Stations

## Overview

Weather Station's weather data consists of many parts that can all be requested simultaneously, but must be requested by `weather station id`. We've broken them up into separate sections so it is easier to read.

<aside class="notice">
	All Graph <code>dateTime</code> values are in <strong>local time</strong> and are formatted as a <strong>Unix time stamp</strong>.
</aside>

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

## Graph - GET - Observational Graphs - Apparent Temperature

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=apparent-temperature&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["apparent-temperature"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
	"observationalGraphs": {
		"apparent-temperature": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "apparent-temperature",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "ApparentTemperaturePointFormatter"
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
									"y": 18.6
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
                            "x": 1395878100,
                            "y": 45
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
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

The temperature equivalent perceived by humans, caused by the combined effects of air temperature, relative humidity and wind speed.

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `apparent-temperature` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` ApparentTemperaturePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | apparent temperature

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Cloud

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=cloud&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["cloud"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
	"observationalGraphs": {
		"cloud": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "cloud",
						"color": "#FFF",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "CloudPointFormatter"
					},
					"yAxisDataMin": 1,
					"yAxisDataMax": 8,
					"yAxisMin": 1,
					"yAxisMax": 10,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 1
								},
								{
									"x": 1395891000,
									"y": 4
								},
								{
									"x": 1395892800,
									"y": 8
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878100,
                            "y": 1
                        },
                        "post": null
                    }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "cloud": "oktas"
            },
			"provider": {
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `cloud` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` CloudPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | int | | cloud

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Delta T

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=delta-t&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["delta-t"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
	"observationalGraphs": {
		"delta-t": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "delta-t",
						"color": "#003355",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "DeltaTPointFormatter"
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
									"y": 18.6
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
                            "x": 1395878100,
                            "y": 45
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
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `delta-t` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` DeltaTPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | delta t

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Dew Point

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=dew-point&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["dew-point"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
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
									"y": 18.6
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
                            "x": 1395878100,
                            "y": 45
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
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

The temperature at which dew will form.

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

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
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | dew point

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET -  Observational Graphs - Humidity

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=humidity&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["humidity"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
	"observationalGraphs": {
		"humidity": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "humidity",
						"color": "#FFF",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": false,
						"pointFormatter": "HumidityPointFormatter"
					},
					"yAxisDataMin": 20,
					"yAxisDataMax": 80,
					"yAxisMin": 0,
					"yAxisMax": 100,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 20
								},
								{
									"x": 1395891000,
									"y": 50
								},
								{
									"x": 1395892800,
									"y": 80
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878100,
                            "y": 30
                        },
                        "post": null
                    }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "percentage": "%"
            },
			"provider": {
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

The amount of water vapor in the atmosphere or a gas.

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `humidity` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` HumidityPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | humidity

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Pressure

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=pressure&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["pressure"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
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
									"y": 18.6
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
                            "x": 1395878100,
                            "y": 45
                        },
                        "post": null
                    }
				},
				"xAxisMin": 1395878400,
				"xAxisMax": 1396051199
			},
			"units": {
                "pressure": "hpa"
            },
			"provider": {
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `pressure` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` PressurePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | pressure

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Rainfall

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=rainfall&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["rainfall"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
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
					"yAxisDataMin": 20.4,
					"yAxisDataMax": 32.8,
					"yAxisMin": 0,
					"yAxisMax": 40,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 20.4
								},
								{
									"x": 1395891000,
									"y": 26.4
								},
								{
									"x": 1395892800,
									"y": 32.8
								}
							]
						}
					],
					"controlPoints": {
                        "pre": {
                            "x": 1395878100,
                            "y": 25.2
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
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `rainfall` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `BarLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` RainfallPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | rainfall

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Temperature

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=temperature&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["temperature"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
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
									"y": 18.6
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
                            "x": 1395878100,
                            "y": 45
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
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `temperature` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointFormatter | string |` TemperaturePointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | temperature

### Control Points

Control points sit before and after the graph to allow you to plot the lines right to the edge of the graph (using the control points as references outside the view area). They are identical to a Point.

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Wind

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=wind&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["wind"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
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
					"yAxisDataMax": 16.7,
					"yAxisMin": 0,
					"yAxisMax": 80,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 7.4,
									"direction": 120,
									"directionText": "ESE",
									"description": "light",
									"pointStyle": {
										"fill": "#d1ef51",
										"stroke": "#7d8f30"
									}
								},
								{
									"x": 1395891000,
									"y": 13,
									"direction": 110,
									"directionText": "ESE",
									"description": "gentle",
									"pointStyle": {
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								},
								{
									"x": 1395892800,
									"y": 16.7,
									"direction": 110,
									"directionText": "ESE",
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
                            "x": 1395878100,
                            "y": 45
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
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `wind` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointRenderer | string |` ArrowPointRenderer` |
pointFormatter | string |` DirectionPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | speed
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Graph - GET - Observational Graphs - Wind Gust

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/weather-stations/257.json?observationalGraphs=wind-gust&startDate=2014-03-27
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"observationalGraphs": ["wind-gust"],
		"startDate": "2010-10-10"
	}
}
```

> Example Response

```json
{
	"observationalGraphs": {
		"wind-gust": {
			"dataConfig": {
				"series": {
					"config": {
						"id": "wind-gust",
						"color": "#0094F8",
						"lineWidth": 2,
						"lineFill": false,
						"lineRenderer": "StraightLineRenderer",
						"showPoints": true,
						"pointRenderer": "ArrowPointRenderer"
					},
					"yAxisDataMin": 9.3,
					"yAxisDataMax": 22.2,
					"yAxisMin": 0,
					"yAxisMax": 80,
					"groups": [
						{
							"dateTime": 1395878400,
							"points": [
								{
									"x": 1395880200,
									"y": 9.3,
									"description": "light",
									"pointStyle": {
										"fill": "#d1ef51",
										"stroke": "#7d8f30"
									}
								},
								{
									"x": 1395891000,
									"y": 16.7,
									"description": "gentle",
									"pointStyle": {	
										"fill": "#a5de37",
										"stroke": "#638521"
									}
								},
								{
									"x": 1395892800,
									"y": 22.2,
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
                            "x": 1395878100,
                            "y": 20.2
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
				"id": 349,
				"name": "Sydney (Observatory Hill)",
				"lat": -33.86,
				"lng": 151.21,
				"distance": 4.3,
				"unit": {
					"distance": "miles"
				}
			},
			"carousel": {
				"size": 1,
				"start": 1
			}
		}
	}
}
```

### Request

`GET api.willyweather.com.au/v2/{api key}/weather-stations/{weather station id}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
days | int |  | max days returned | false
observationalGraphs | csv |  |  | true
units | string | See <a href="#units">Units</a>. |  | false
startDate | string |  | This is used with conjunction with the `days` parameter, when both are added the result will be the _end date_, the startdate and the _end date_ will be the range to filter out the entries. | false

<aside class="notice">
    <code>days = 1</code> by default.
</aside>
<aside class="notice">
    <code>startDate</code> is current date by default.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Data Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
series | object ||  **(see below)**
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
config | object | | **(see below)**
yAxisDataMin | int | | the smallest y value
yAxisDataMax | int | | the largest y value
yAxisMin | int | | the smallest y value with graph padding
yAxisMax | int | | the largest y value with graph padding
groups | object | | **(see below)**
controlPoints | object | | **(see below)**

### Config

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | string | `wind-gust` |
color | string | | hexadecimal colour code
lineWidth | int | | recommended line width in points
lineFill | boolean | `false` | whether the area under the graph should have a fill or not
lineRenderer | string | `StraightLineRenderer` |
showPoints | boolean | `false` | whether to show data points or just display a line
pointRenderer | string |` ArrowPointRenderer` |
pointFormatter | string |` DirectionPointFormatter` |

### Groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
points | array | | array of `point` objects **(see below)**

### Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
x | int | | time value
y | double | | speed
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

### Carousel

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
size | int | | The total number of available days of data
start | int | | The index of the start of the current observational graph

## Weather Station - GET - by Location id

> Example Query String Request

```shell
api.willyweather.com.au/v2/{api key}/locations/{location id}/weather-stations.json?units=distance:miles
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"units" :  {
			"distance": "km"
		}
	}
}
```

> Example Response

```json
{
	"pressure": [
		{
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
           }
		}
	],
	"rainfall": [
		{
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
            }
        }
	],
    "temperature": [
    	{
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
            	"distance": "miles"
			}
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
            	"distance": "miles"
            }
        }
    ],
    "wind": [
    	{
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
            	"distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
            	"distance": "miles"
            }
        }
    ],
    "humidity": [
    	{
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
            	"distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
            	"distance": "miles"
            }
        }
    ],
    "dewpoint": [
        {
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
            }
        }
    ],
    "apparent-temperature": [
        {
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
            }
        }
    ],
    "cloud": [
        {
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
            }
        }
    ],
    "delta-t": [
        {
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
            }
        }
    ],
    "wind-gust": [
        {
            "id": 116,
            "name": "Borroloola",
            "lat": -16.08,
            "lng": 136.3,
            "distance": 1.8,
            "units": {
                "distance": "miles"
            }
        },
        {
            "id": 115,
            "name": "Mcarthur River Mine",
            "lat": -16.44,
            "lng": 136.08,
            "distance": 29.7,
            "units": {
                "distance": "miles"
            }
        }
    ]
}
```

Returns the list of Weather Stations that are linked to the given location and are currently reporting data. For each data parameter, a location is linked up to 4 weather stations.

### Request
`GET api.willyweather.com.au/v2/{api key}/locations/{location id}/weather-stations.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
distance | string | `km`, `miles `| unit of measurement | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

An array of climate with its provider's data. 

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
entries | array | `pressure`, `rainfall`, `temperature`, `wind`, `humidity`, `dewpoint`, `apparent-temperature`, `cloud`, `delta-t`, `wind-gust` | key for the list of Provider

### Provider

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int |  | 
name | string |  | 
lat | double |  | 
lng | double |  | 
distance | double |  | distance of provider from location 
units | object |  | includes unit of measurement for distance 

# Accounts

## Overview

Account related data can be accessed and modified using different endpoints that starts with <code>/accounts/</code>.

### Response - Account object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
uid | string | |
firstName | string | |
lastName | string | |
email | string | |
credentials | array | | Account's accessibility
accountFeatures | array | **see (Account Feature) below** | An array of account features.
locations | array | See <a href="#location-get-by-location-id">Locations</a>. | An array of account's favorite locations.
units | object | See <a href="#units">Units</a>. | Account's prefered units.
warningFilters | array | See `classifications` in <a href="#warning-types">Warning Types</a>  | An array of warning filters
createdDateTime | string | | Created date time (YYYY-MM-DD HH:MM:SS`)
loggedInDateTime | string | | Last logged in date time (`YYYY-MM-DD HH:MM:SS`). Can be null.

### Account Feature

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`, `10` | `1` for "SMS"<br/>`2` for "Ad Removal"<br/>`3` for "1 Notification"<br/>`4` for "5 Notifications"<br/>`5` for "10 Notifications"<br/>`6` for "25 Notifications"<br/>`7` for "1 Contact"<br/>`8` for "20 Contacts"<br/>`9` for "100 Contacts"<br/>`10` for Webhook
code | string | `sms`, `ads`, `notifications1`, `notifications5`, `notifications10`, `notifications25`, `contacts1`, `contacts20`, `contacts100`, `webhook` |
name | string | "SMS", "Ad Removal", "1 Notification", "5 Notifications", "10 Notifications", "25 Notifications", "1 Contact", "20 Contacts", "100 Contacts", "Webhook" |
monthlyCost | int | | In cents
annualCost | int | |

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

## Account - POST - Register

> Example Request Body

```json
{
    "email": "jupiterjones60ae13a7a8592@willyweather.com",
    "password": "secret1234",
    "fullName": "Jupiter Jones"
}
```

> Example Response

```json
{
    "uid": "80ca55b0-32f0-4c51-8701-c5c779c8f47d",
    "firstName": "Jupiter",
    "lastName": "Jones",
    "email": "jupiterjones60ae164860728@willyweather.com",
    "credentials": [],
    "accountFeatures": [
        {
            "id": 3,
            "code": "notifications1",
            "name": "1 Notification",
            "monthlyCost": 0,
            "annualCost": 0
        },
        {
            "id": 7,
            "code": "contacts1",
            "name": "1 Contact",
            "monthlyCost": 0,
            "annualCost": 0
        }
    ],
    "locations": [
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
    ],
    "units": {
        "temperature": "c",
        "tideHeight": "m",
        "swellHeight": "m",
        "speed": "km\/h",
        "amount": "mm",
        "distance": "miles",
        "hour": "g",
        "pressure": "hpa"
    },
    "warningFilters": ["flood", "strong-wind"],
    "createdDateTime": "2021-05-26 17:35:06",
    "loggedInDateTime": null
}
```

Registers an account.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/register.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
email | string | | | true
password | string | | | true
fullName | string | | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when using an endpoint via <strong>Request Body</strong>.
</aside>

### Response

Response is an Account. See <a href="#accounts">Accounts</a> for a description of an Account response.

## Account - GET - Login

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"email": "bartsimpson@willyweather.com",
		"password": "secret1234"
	}
}
```

> Example Response

```json
{
    "uid": "4b26915f-ffef-4b3c-b5f1-4f172c07bb8f",
    "firstName": "Bart",
    "lastName": "Simpson",
    "email": "bartsimpson@willyweather.com",
    "credentials": [],
    "accountFeatures": [
        {
            "id": 3,
            "code": "notifications1",
            "name": "1 Notification",
            "monthlyCost": 0,
            "annualCost": 0
        }, 
        {
            "id": 7,
            "code": "contacts1",
            "name": "1 Contact",
            "monthlyCost": 0,
            "annualCost": 0
        }
    ],
    "locations": [
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
    ],
    "units": {
        "temperature": "f",
        "tideHeight": "ft",
        "swellHeight": "m",
        "speed": "km\/h",
        "amount": "mm",
        "distance": "miles",
        "hour": "g",
        "pressure": "hpa"
    },
    "warningFilters": ["flood", "strong-wind"],
    "createdDateTime": "2016-01-01 00:00:00",
    "loggedInDateTime": "2016-01-01 00:00:00"
}
```

Login an account.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/login.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
email | string | | | true
password | string | | | true

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an Account. See <a href="#accounts">Accounts</a> for a description of an Account response.

## Account - GET - Update Heartbeat

> Example Request Header

```json
{}
```

> Example Response

```json
[]
```

Updates a account's _loggedInDateTime_.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/heartbeat.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when using an endpoint via <strong>Request Body</strong>.
</aside>

### Response

Response is an empty array.

## Contact - GET - by Contact uid

> Example Request Header

```json
{}
```

> Example Response

```json
{
	"uid": "882dbe71-f933-4a8e-9597-83e4f0e1ac4a",
	"firstName": "Homer",
	"lastName": "Simpson",
	"email": "homersimpson@willyweather.com",
	"isEmailVerified": true,
	"phone": "0411222333",
	"isPhoneVerified": true,
	"webhook": "http://www.homerswebpage.com",
	"isDefault": true
}
```

Returns the details of a single contact

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts/{contact uid}.json`

### Response - Contact Object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
uid | string | | 
firstName | string | |
lastName | string | |
email | string | |
isEmailVerified | boolean | | Determines if email address is already verified
phone | string | |
isPhoneVerified | boolean | | Determines if phone is already verified
webhook | string | |
isDefault | boolean | | Determines whether the contact is the default of the account

## Contact - GET - by Account uid

> Example Request Header

```json
{}
```

> Example Response

```json
[
	{
		"uid": "882dbe71-f933-4a8e-9597-83e4f0e1ac4a",
		"firstName": "Homer",
		"lastName": "Simpson",
		"email": "homersimpson@willyweather.com",
		"isEmailVerified": true,
		"phone": "0411222333",
		"isPhoneVerified": true,
		"webhook": "http://www.homerswebpage.com",
		"isDefault": true
	}
]
```

Returns all contacts associated with the account

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts.json`

### Response - Contact Object
Response is an array of Contacts. See <a href="#contact-get-by-contact-uid">Get by uid</a> for description of a Contact response.

## Contact - POST - Create

> Example Request Body

```json
{
    "contact": 	{
		"firstName": "Homer",
		"lastName": "Simpson",
		"email": "homersimpson@willyweather.com",
		"phone": "0411222333",
		"automatedCall": "0411222333",
		"webhook": "http://www.homerswebpage.com",
		"isDefault": true
	}
}
```

> Example Response

```json
{}
```

Creates a Contact.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts.json`

### Contact

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
firstName | string | | | true
lastName | string | | | true
email | string | | | true
phone | string | | | true
automatedCall | string | | | true
webhook | string | | | true
isDefault | boolean | | | true

### Response

Response is empty object.

## Contact - PUT - Update

> Example Request Body

```json
{
    "contact": 	{
		"firstName": "Homer",
		"lastName": "Simpson",
		"email": "homersimpson@willyweather.com",
		"phone": "0411222333",
		"automatedCall": "0411222333",
		"webhook": "http://www.homerswebpage.com",
		"isDefault": true
	}
}
```

> Example Response

```json
{}
```

Updates a Contact.

### Request

`PUT api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts/{contact uid}.json`

### Contact

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
firstName | string | | | true
lastName | string | | | true
email | string | | | true
phone | string | | | true
automatedCall | string | | | true
webhook | string | | | true
isDefault | boolean | | | true

### Response

Response is empty object.

## Contact  - DELETE - Delete

> Example Request Body

```json
{}
```

> Example Response

```json
[]
```

Deletes a Contact.

### Request

`DELETE api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts/{contact uid}.json`

### Response

Response is an empty object.

## Contact - POST - Send Verification code

> Example Request Body

```json
{
	"contactVerification": {
		"transporterType": 1
	}
}
```

> Example Response

```json
[]
```

Sends a verification code to a Contact.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts/{contact uid}/send-verification.json`

### Contacts Verification

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
transporterType | int | 1 is **Email**. 2 is **SMS** | | true

### Response

Response is an empty object.

## Contact - POST - Verify

> Example Request Body

```json
{
	"verificationCode": "123ABCD",
	"contactVerification": {
		"transporterType": 1
	}
}
```

> Example Response

```json
[]
```

Verify contact's SMS or Email

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/contacts/{contact uid}/verify.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
verificationCode | string | | Verification code sent to Email or SMS | true

### Contacts Verification

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
transporterType | int | 1 is **Email**. 2 is **SMS** | | true

### Response

Response is an empty object.

## Device - GET - All Devices by Account uid

> Example Request Header

```json
{}
```

> Example Response

```json
[
    {
        "uid": "882dbe71-f933-4a8e-9597-83e4f0e1ac4a",
        "token": "<device token>",
        "endpointArn": "arn:aws:...",
        "lat": -33.869,
        "lng": 151.226,
        "deviceType": {
            "id": 1,
            "name": "ios"
        }
    },
    {
        "uid": "99d81062-ac72-48ad-b69f-66d96e5d1c46",
        "token": "<device token>",
        "endpointArn": "arn:aws:...",
        "lat": 14.21,
        "lng": 121.992,
        "deviceType": {
            "id": 2,
            "name": "android"
        }
    }
]
```

Returns the list of account's devices

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/devices.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response - Device Object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
uid | string | | 
token | string | | a unique key for the app-device combination for push notification gateways
endpointArn | string | | **Amazon Resource Names**
lat | float | | the exact coordinates of the device. Can be null.
lng | float | | the exact coordinates of the device. Can be null.
deviceType | object | | **(see Device Type object)** 

### Device Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1` for ios, `2` for android |
name | string | `ios`, `android` | Mobile type name

## Device - POST - Create

> Example Request Body

```json
{
    "token": "<device token>",
    "lat": -33.333,
    "lng": 105.666,
    "deviceType": {
        "id": 2
    }
}
```

> Example Response

```json
{
    "uid": "be1a2610-2ace-4d9c-a7da-2af1ec8acd35",
    "token": "<device token>",
    "endpointArn": "arn:aws:...",
    "lat": -33.333,
    "lng": 105.666,
    "deviceType": {
        "id": 2,
        "name": "android"
    }
}
```

Creates a device.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/devices.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
deviceType | object | see <a href="#device-type">Device Type</a> | | true
token | string | | a unique key for the app-device combination for push notification gateways | false
lat | float | | the exact coordinates of a device | false
lng | float | | the exact coordinates of a device | false

<aside class="notice">
    Providing <code>lat</code> requires <code>lng</code> parameter and vice versa.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is Device. See <a href="#device-get-all-devices-by-account-uid">Device</a> for description of a Device response.

## Device - PUT - Update

> Example Request Body

```json
{
    "token": "<device token>",
    "lat": -32.86901,
    "lng": 151.2261
}
```

> Example Response

```json
{
    "uid": "738f8f27-2818-4786-8525-94d443a1a5c8",
    "token": "<device token>",
    "endpointArn": "arn:aws:...",
    "lat": -32.869,
    "lng": 151.226,
    "deviceType": {
        "id": 1,
        "name": "ios"
    }
}
```

Updates a device.

### Request

`PUT api.willyweather.com.au/v2/{api key}/accounts/{account uid}/devices/{device uid}.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
token | string | | a unique key for the app-device combination for push notification gateways | false
lat | float | | the exact coordinates of a device | false
lng | float | | the exact coordinates of a device | false

<aside class="notice">
    Providing <code>lat</code> requires <code>lng</code> parameter and vice versa.
</aside>
<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is Device. See <a href="#device-get-all-devices-by-account-uid">Device</a> for description of a Device response.

## Location - GET - All Locations by Account uid

> Example Request Header

```json
{}
```

> Example Response

```json
[
    {
        "id": 16,
        "name": "Canberra",
        "region": "Canberra",
        "state": "ACT",
        "postcode": "2600",
        "timeZone": "Australia\/Sydney",
        "lat": -35.282,
        "lng": 149.1286,
        "typeId": 22
    },
    {
        "id": 54,
        "name": "Gordon",
        "region": "Canberra",
        "state": "ACT",
        "postcode": "2906",
        "timeZone": "Australia\/Sydney",
        "lat": -35.4506,
        "lng": 149.0835,
        "typeId": 22
    }
]
```

Returns the list of account's locations.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/locations.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an array of Locations. See <a href="#location-get-by-location-id">Locations</a> for description of a Location response.

## Location - POST - Update

> Example Request Body

```json
[
    {
        "id": 16
    },
    {
        "id": 54
    },
    {
        "id": 221
    }
]
```

> Example Response

```json
[]
```

Update account's locations.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/locations.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
 | array | **(See Location below)** | An array of locations | true

### Location

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | | | true

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an empty array.

## Notification - GET - All Notifications

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"notificationTypes": [
			{
				"id": 1
			},
            {
                "id": 2
            },
			{
				"id": 3
			}
		],
		"notificationTransporterTypes": [
            {
                "id": 1
            },
			{
				"id": 2
			},
			{
				"id": 3
			},
            {
                "id": 5
            }
		]
	}
}
```

> Example Response

```json
[{
  "uid": "7d0e6509-f9c1-431d-a0b0-2619d27ed68f",
  "name": "Account 4 Alert 1",
  "enabled": true,
  "notificationType": {
    "id": 1,
    "name": "alert"
  },
  "notificationContacts": [{
    "id": 1,
    "contact": {
      "id": 7,
      "firstName": "Krusty D",
      "lastName": "Clown",
      "email": "krusty@willyweather.com",
      "phone": "4013444",
      "webhook": "http://krusty.com",
      "isDefault": true
    },
    "notificationTransporterType": {
      "id": 1,
      "name": "email"
    }
  },
    {
      "id": 6,
      "contact": {
        "id": 7,
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "phone": "4013444",
        "webhook": "http://krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 2,
        "name": "sms"
      }
    },
    {
      "id": 13,
      "contact": {
        "id": 7,
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "phone": "4013444",
        "webhook": "http://krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 3,
        "name": "ios"
      }
    },
    {
      "id": 7,
      "contact": {
        "id": 7,
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "phone": "4013444",
        "webhook": "http://krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 5,
        "name": "webhook"
      }
    }
  ],
  "followMe": false,
  "location": {
    "id": 4988,
    "name": "Bondi Beach",
    "region": "Sydney",
    "state": "NSW",
    "postcode": "2026",
    "timeZone": "Australia/Sydney",
    "lat": -33.8905,
    "lng": 151.2749,
    "typeId": 2
  },
  "createdDateTime": "2016-01-01 00:00:00",
  "notifyMeOffset": "0",
  "notificationTime": {
    "mon": true,
    "tue": true,
    "wed": false,
    "thu": false,
    "fri": false,
    "sat": false,
    "sun": false,
    "startTime": 0,
    "endTime": 1140
  },
  "notificationAlertConditions": [{
    "id": 1,
    "notificationAlertConditionType": {
      "id": 2,
      "code": "forecast-max-temp"
    },
    "tempRangeStart": 290,
    "tempRangeEnd": 310
  }]
},
  {
    "uid": "6469b8e4-5bcb-4a40-804a-f1c136338f61",
    "name": "Account 4 Report 1",
    "enabled": true,
    "notificationType": {
      "id": 2,
      "name": "report"
    },
    "notificationContacts": [{
      "id": 2,
      "contact": {
        "uid": "2c1b8e02-4872-4452-a989-2343e10135e1",
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "isEmailVerified": true,
        "phone": "4013444",
        "isPhoneVerified": true,
        "webhook": "http:\/\/krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 1,
        "name": "email"
      }
    }, {
      "id": 8,
      "contact": {
        "uid": "2c1b8e02-4872-4452-a989-2343e10135e1",
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "isEmailVerified": true,
        "phone": "4013444",
        "isPhoneVerified": true,
        "webhook": "http:\/\/krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 2,
        "name": "sms"
      }
    }],
    "followMe": false,
    "location": {
      "id": 4988,
      "name": "Bondi Beach",
      "region": "Sydney",
      "state": "NSW",
      "postcode": "2026",
      "timeZone": "Australia\/Sydney",
      "lat": -33.8905,
      "lng": 151.2749,
      "typeId": 2
    },
    "createdDateTime": "2016-01-01 00:00:00",
    "notifyMeOffset": "0",
    "notificationDays": {
      "mon": true,
      "tue": true,
      "wed": false,
      "thu": false,
      "fri": false,
      "sat": false,
      "sun": false
    },
    "notificationTimeRanges": [{
      "startTime": 60,
      "endTime": 120
    }],
    "weatherTypes": [{
      "id": 2,
      "code": "wind",
      "name": "wind",
      "displayName": "wind",
      "marineBased": false
    }]
  },
  {
    "uid": "7ca1f593-2b89-4b78-9d84-51ed895f7948",
    "name": "Account 4 Warning 1",
    "enabled": true,
    "notificationType": {
      "id": 3,
      "name": "warning"
    },
    "notificationContacts": [{
      "id": 3,
      "contact": {
        "uid": "2c1b8e02-4872-4452-a989-2343e10135e1",
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "isEmailVerified": true,
        "phone": "4013444",
        "isPhoneVerified": true,
        "webhook": "http:\/\/krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 1,
        "name": "email"
      }
    }, {
      "id": 10,
      "contact": {
        "uid": "2c1b8e02-4872-4452-a989-2343e10135e1",
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "isEmailVerified": true,
        "phone": "4013444",
        "isPhoneVerified": true,
        "webhook": "http:\/\/krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 2,
        "name": "sms"
      }
    }, {
      "id": 11,
      "contact": {
        "uid": "2c1b8e02-4872-4452-a989-2343e10135e1",
        "firstName": "Krusty D",
        "lastName": "Clown",
        "email": "krusty@willyweather.com",
        "isEmailVerified": true,
        "phone": "4013444",
        "isPhoneVerified": true,
        "webhook": "http:\/\/krusty.com",
        "isDefault": true
      },
      "notificationTransporterType": {
        "id": 3,
        "name": "ios"
      }
    }],
    "followMe": true,
    "location": {
      "id": 4988,
      "name": "Bondi Beach",
      "region": "Sydney",
      "state": "NSW",
      "postcode": "2026",
      "timeZone": "Australia\/Sydney",
      "lat": -33.8905,
      "lng": 151.2749,
      "typeId": 2
    },
    "createdDateTime": "2016-01-01 00:00:00",
    "warningType": {
      "id": 21,
      "code": "closed-waters",
      "name": "Closed Water",
      "classification": "closed-water",
      "warningSeverityLevels": null
    },
    "warningSeverityLevels": [{
      "id": 1,
      "code": "yellow"
    }]
  }
]
```

Returns the list of account's notifications units.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
notificationTypes | array | **(See Notification Type)** | | false
notificationTransporterTypes | array | **(See Notification Transporter Type)** | | false

### Notification Type

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | `1`, `2`, `3` | `1` for alert type <br/> `2` for report type <br/> `3` for warning type | true

### Notification Transporter Type

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | `1`, `2`, `3`, `4`, `5` | `1` for email <br/> `2` for sms <br/> `3` for ios <br/> `4` for android <br/> `5` for webhook | true

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response - Notification object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
uid | string | | 
name | string | | 
enabled | boolean | | 
notificationType | object | | **(see Notification Type object)**
notificationContacts | array | | an array of Notification Contact objects **(see Notification Contact)**
followMe | boolean | | 
location | string | |  **(See <a href="#location-get-by-location-id">Locations</a> object)**
createdDateTime | string | | Created date time (YYYY-MM-DD HH:MM:SS`)
notifyMeOffset | int | | In minutes (only exists if type is `alert`). Can be null.
notificationTime | object | | **(see Notification Time object)** (only exists if type is `alert`)
notificationAlertConditions | array | | an array of Notification Alert Conditions **(see Notification Alert Condition object)** In minutes (only exists if type is `alert`)
warningType | object | | **(See <a href="#warning-types">Warning Types</a> object)** (only exists if type is `warning`)

### Notification Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1`, `2`, `3` | `1` for alert <br/> `2` for report <br/> `3` for warning
name | string | `alert`, `report`, `warning` | 

### Notification Contact

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int |  | 
contact | object |  | **(see Contact object)**
notificationTransporterType | object |  | **(see Notification Transporter Type object)**

### Contact

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | | 
firstName | string | |
lastName | string | |
email | string | |
phone | string | |
webhook | string | |
isDefault | boolean | |

### Notification Transporter Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1` for email <br/> `2` for sms <br/> `3` for ios <br/> `4` for android <br/> `5` for webhook | 
name | string | `email`, `sms`, `ios`, `android`, `webhook` |

### Notification Time

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
mon | boolean | | Monday
tue | boolean | | Tuesday
wed | boolean | | Wednesday
thu | boolean | | Thursday
fri | boolean | | Friday
sat | boolean | | Saturday
sun | boolean | | Sunday
startTime | int | 0 - 1439 | Time when the notification starts (in minutes)
endTime | int | 0 - 1439 | Time when the notification ends (in minutes)

### Notification Alert Condition

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
notificationAlertConditionType | object | | **(see Notification Alert Condition Type object)**
tempRangeStart | float | | Only exists if alert condition type is `forecast-min-temp` or `forecast-max-temp`
tempRangeEnd | float | | Only exists if alert condition type is `forecast-min-temp` or `forecast-max-temp`
heightRangeStart | float | | Only exists if alert condition type is `forecast-swell`
heightRangeEnd | float | | Only exists if alert condition type is `forecast-swell`
periodRangeStart | float | | Only exists if alert condition type is `forecast-swell`
periodRangeEnd | float | | Only exists if alert condition type is `forecast-swell`
directionRangeStart | float | | Only exists if alert condition type is `forecast-swell`
directionRangeEnd | float | | Only exists if alert condition type is `forecast-swell`
speedRangeStart | float | | Only exists if alert condition type is `forecast-wind` or `current-wind`
speedRangeEnd | float | | Only exists if alert condition type is `forecast-wind` or `current-wind`
directionRangeStart | int | | Only exists if alert condition type is `forecast-wind` or `current-wind`
directionRangeEnd | int | | Only exists if alert condition type is `forecast-wind` or `current-wind`
precis | array | | Only exists if alert condition type is `forecast-weather` or `forecast-region-precis`
precis | string | | Only exists if alert condition type is `forecast-hourly-precis`
amount | int | | Only exists if alert condition type is `forecast-rainfall`
probability | int | | Only exists if alert condition type is `forecast-rainfall`
status | string | | Only exists if alert condition type is `forecast-tides`
buffer | string | | Only exists if alert condition type is `forecast-tides`
time | string | | Only exists if alert condition type is `forecast-sunrise-sunset`
indexRangeStart | float | | Only exists if alert condition type is `forecast-uv` or `forecast-daily-max-uv`
indexRangeEnd | float | | Only exists if alert condition type is `forecast-uv` or `forecast-daily-max-uv`
phase | string | | Only exists if alert condition type is `forecast-moonphase`
surroundingDays | string | | Only exists if alert condition type is `forecast-moonphase`
tempRangeStart | float | | Only exists if alert condition type is `current-temp`
tempRangeEnd | float | | Only exists if alert condition type is `current-temp`
trend | int | | Only exists if alert condition type is `current-temp`
apparentTempRangeStart | float | | Only exists if alert condition type is `current-apparent-temp`
apparentTempRangeEnd | float | | Only exists if alert condition type is `current-apparent-temp`
deltaTRangeStart | float | | Only exists if alert condition type is `current-delta-t`
deltaTRangeEnd | float | | Only exists if alert condition type is `current-delta-t`
trend | int | | Only exists if alert condition type is `current-delta-t`
amount | int | | Only exists if alert condition type is `current-rain-last-hour` or `current-rain-since-9am`
humidityRangeStart | float | | Only exists if alert condition type is `current-humidity`
humidityRangeEnd | float | | Only exists if alert condition type is `current-humidity`
dewpointRangeStart | float | | Only exists if alert condition type is `current-dewpoint`
dewpointRangeEnd | float | | Only exists if alert condition type is `current-dewpoint`
trend | int | | Only exists if alert condition type is `current-dewpoint`
pressureRangeStart | float | | Only exists if alert condition type is `current-pressure`
pressureRangeEnd | float | | Only exists if alert condition type is `current-pressure`
trend | int | | Only exists if alert condition type is `current-pressure`
forecastCode | string | | Only exists if alert condition type is `forecast-radar`
intensity | int | | Only exists if alert condition type is `forecast-radar`
minimumRainDuration | int | | Only exists if alert condition type is `forecast-radar`
mapLocationId | int | | Only exists if alert condition type is `forecast-radar`
gustSpeedRangeStart | int | | Only exists if alert condition type is `current-wind-gust`
gustSpeedRangeEnd | int | | Only exists if alert condition type is `current-wind-gust`
cloudRangeStart | int | | Only exists if alert condition type is `current-cloud`
cloudRangeEnd | int | | Only exists if alert condition type is `current-cloud`
trend | int | | Only exists if alert condition type is `current-cloud`

### Notification Alert Condition Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | | `1` for `forecast-min-temp`<br/> `2` for `forecast-max-temp`<br/> `3` for `forecast-swell`<br/> `4` for `forecast-wind`<br/> `5` for `forecast-weather`<br/> `6` for `forecast-rainfall`<br/> `7` for `forecast-tides`<br/> `8` for `forecast-sunrise-sunset`<br/> `9` for `forecast-uv`<br/> `10` for `forecast-moonphase`<br/> `11` for `forecast-radar`<br/> `12` for `forecast-hourly-precis`<br/> `13` for `forecast-region-precis`<br/> `14` for `forecast-daily-max-uv`<br/> `20` for `current-wind`<br/> `21` for `current-temp`<br/> `22` for `current-rain-last-hour`<br/> `23` for `current-rain-since-9am`<br/> `24` for `current-humidity`<br/> `25` for `current-dewpoint`<br/> `26` for `current-pressure`<br/> `27` for `current-delta-t`<br/> `28` for `current-apparent-temp`<br/> `29` for `current-wind-gust`<br/> `30` for `current-cloud`<br/> |
code | string | `forecast-min-temp`, `forecast-max-temp`, `forecast-swell`, `forecast-wind`, `forecast-weather`, `forecast-rainfall`, `forecast-tides`, `forecast-sunrise-sunset`, `forecast-uv`, `forecast-moonphase`, `forecast-radar`, `forecast-hourly-precis`, `forecast-region-precis`, `forecast-daily-max-uv`, `current-wind`, `current-temp`, `current-rain-last-hour`, `current-rain-since-9am`, `current-humidity`, `current-dewpoint`, `current-pressure`, `current-delta-t`, `current-apparent-temp`, `current-wind-gust`, `current-cloud` |

## Notification - GET - Alert Condition Types

> Example Request Header

```json
{}
```

> Example Response

```json
{
	"absolute": {
		"observational": [{
			"id": 30,
			"code": "currentCloud",
			"title": "Cloud Cover",
			"components": [{
				"type": "slider",
				"title": "Cloud Cover",
				"unit": "oktas",
				"min": {
					"key": "cloudRangeStart",
					"values": [{
						"value": 0,
						"unit": "oktas",
						"default": true
					}]
				},
				"max": {
					"key": "cloudRangeEnd",
					"values": [{
						"value": 8,
						"unit": "oktas",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 3,
						"unit": "oktas"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 5,
						"unit": "oktas"
					}]
				}
			}, {
				"type": "radiobutton",
				"title": "Trend:",
				"key": "trend",
				"groups": [{
					"inputs": [{
						"label": "Any",
						"value": "1",
						"default": true
					}, {
						"label": "Rising",
						"value": "2",
						"default": false
					}, {
						"label": "Falling",
						"value": "3",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 27,
			"code": "currentDeltaT",
			"title": "Delta T",
			"components": [{
				"type": "slider",
				"title": "Delta T",
				"unit": "temperature",
				"min": {
					"key": "deltaTRangeStart",
					"values": [{
						"value": 210,
						"unit": "k",
						"default": true
					}, {
						"value": -60,
						"unit": "c",
						"default": false
					}, {
						"value": -80,
						"unit": "f",
						"default": false
					}]
				},
				"max": {
					"key": "deltaTRangeEnd",
					"values": [{
						"value": 334,
						"unit": "k",
						"default": true
					}, {
						"value": 60,
						"unit": "c",
						"default": false
					}, {
						"value": 140,
						"unit": "f",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 278.15,
						"unit": "k"
					}, {
						"value": 5,
						"unit": "c"
					}, {
						"value": 41,
						"unit": "f"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 308.15,
						"unit": "k"
					}, {
						"value": 35,
						"unit": "c"
					}, {
						"value": 95,
						"unit": "f"
					}]
				}
			}, {
				"type": "radiobutton",
				"title": "Trend:",
				"key": "trend",
				"groups": [{
					"inputs": [{
						"label": "Any",
						"value": "1",
						"default": true
					}, {
						"label": "Rising",
						"value": "2",
						"default": false
					}, {
						"label": "Falling",
						"value": "3",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 25,
			"code": "currentDewpoint",
			"title": "Dewpoint",
			"components": [{
				"type": "slider",
				"title": "Dewpoint",
				"unit": "temperature",
				"min": {
					"key": "dewpointRangeStart",
					"values": [{
						"value": 210,
						"unit": "k",
						"default": true
					}, {
						"value": -60,
						"unit": "c",
						"default": false
					}, {
						"value": -80,
						"unit": "f",
						"default": false
					}]
				},
				"max": {
					"key": "dewpointRangeEnd",
					"values": [{
						"value": 334,
						"unit": "k",
						"default": true
					}, {
						"value": 60,
						"unit": "c",
						"default": false
					}, {
						"value": 140,
						"unit": "f",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 278.15,
						"unit": "k"
					}, {
						"value": 5,
						"unit": "c"
					}, {
						"value": 41,
						"unit": "f"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 308.15,
						"unit": "k"
					}, {
						"value": 35,
						"unit": "c"
					}, {
						"value": 95,
						"unit": "f"
					}]
				}
			}, {
				"type": "radiobutton",
				"title": "Trend:",
				"key": "trend",
				"groups": [{
					"inputs": [{
						"label": "Any",
						"value": "1",
						"default": true
					}, {
						"label": "Rising",
						"value": "2",
						"default": false
					}, {
						"label": "Falling",
						"value": "3",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 24,
			"code": "currentHumidity",
			"title": "Humidity",
			"components": [{
				"type": "slider",
				"title": "Humidity",
				"unit": "percentage",
				"min": {
					"key": "humidityRangeStart",
					"values": [{
						"value": 0,
						"unit": "%",
						"default": true
					}]
				},
				"max": {
					"key": "humidityRangeEnd",
					"values": [{
						"value": 100,
						"unit": "%",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 40,
						"unit": "%"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 60,
						"unit": "%"
					}]
				}
			}]
		}, {
			"id": 26,
			"code": "currentPressure",
			"title": "Pressure",
			"components": [{
				"type": "slider",
				"title": "Pressure",
				"unit": "pressure",
				"min": {
					"key": "pressureRangeStart",
					"values": [{
						"value": 860,
						"unit": "hpa",
						"default": true
					}, {
						"value": 26,
						"unit": "inhg",
						"default": false
					}, {
						"value": 12.5,
						"unit": "psi",
						"default": false
					}, {
						"value": 860,
						"unit": "millibars",
						"default": false
					}, {
						"value": 650,
						"unit": "mmhg",
						"default": false
					}]
				},
				"max": {
					"key": "pressureRangeEnd",
					"values": [{
						"value": 1100,
						"unit": "hpa",
						"default": true
					}, {
						"value": 32,
						"unit": "inhg",
						"default": false
					}, {
						"value": 15.9,
						"unit": "psi",
						"default": false
					}, {
						"value": 1100,
						"unit": "millibars",
						"default": false
					}, {
						"value": 820,
						"unit": "mmhg",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 946,
						"unit": "hpa"
					}, {
						"value": 27.9,
						"unit": "inhg",
						"default": false
					}, {
						"value": 13.7,
						"unit": "psi",
						"default": false
					}, {
						"value": 946,
						"unit": "millibars",
						"default": false
					}, {
						"value": 709.6,
						"unit": "mmhg",
						"default": false
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 1014,
						"unit": "hpa"
					}, {
						"value": 29.9,
						"unit": "inhg",
						"default": false
					}, {
						"value": 14.7,
						"unit": "psi",
						"default": false
					}, {
						"value": 1014,
						"unit": "millibars",
						"default": false
					}, {
						"value": 760.6,
						"unit": "mmhg",
						"default": false
					}]
				}
			}, {
				"type": "radiobutton",
				"title": "Trend:",
				"key": "trend",
				"groups": [{
					"inputs": [{
						"label": "Any",
						"value": "1",
						"default": true
					}, {
						"label": "Rising",
						"value": "2",
						"default": false
					}, {
						"label": "Falling",
						"value": "3",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 22,
			"code": "currentRainLastHour",
			"title": "Rain Last Hour",
			"components": [{
				"type": "enabler",
				"title": "No rain",
				"value": -1
			}, {
				"type": "slider",
				"title": "Rain Last Hour at least",
				"unit": "amount",
				"min": {
					"key": "amount",
					"values": [{
						"value": 0,
						"unit": "mm",
						"default": true
					}, {
						"value": 0,
						"unit": "in",
						"default": false
					}, {
						"value": 0,
						"unit": "pts",
						"default": false
					}]
				},
				"max": {
					"values": [{
						"value": 300,
						"unit": "mm",
						"default": true
					}, {
						"value": 10,
						"unit": "in",
						"default": false
					}, {
						"value": 800,
						"unit": "pts",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 150,
						"unit": "mm"
					}, {
						"value": 5.9,
						"unit": "in"
					}, {
						"value": 425,
						"unit": "pts"
					}]
				}
			}]
		}, {
			"id": 21,
			"code": "currentTemp",
			"title": "Temperature",
			"components": [{
				"type": "slider",
				"title": "Temperature",
				"unit": "temperature",
				"min": {
					"key": "tempRangeStart",
					"values": [{
						"value": 210,
						"unit": "k",
						"default": true
					}, {
						"value": -60,
						"unit": "c",
						"default": false
					}, {
						"value": -80,
						"unit": "f",
						"default": false
					}]
				},
				"max": {
					"key": "tempRangeEnd",
					"values": [{
						"value": 334,
						"unit": "k",
						"default": true
					}, {
						"value": 60,
						"unit": "c",
						"default": false
					}, {
						"value": 140,
						"unit": "f",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 278.15,
						"unit": "k"
					}, {
						"value": 5,
						"unit": "c"
					}, {
						"value": 41,
						"unit": "f"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 308.15,
						"unit": "k"
					}, {
						"value": 35,
						"unit": "c"
					}, {
						"value": 95,
						"unit": "f"
					}]
				}
			}, {
				"type": "radiobutton",
				"title": "Trend:",
				"key": "trend",
				"groups": [{
					"inputs": [{
						"label": "Any",
						"value": "1",
						"default": true
					}, {
						"label": "Rising",
						"value": "2",
						"default": false
					}, {
						"label": "Falling",
						"value": "3",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 28,
			"code": "currentApparentTemp",
			"title": "Temperature (feels like)",
			"components": [{
				"type": "slider",
				"title": "Apparent Temperature",
				"unit": "temperature",
				"min": {
					"key": "apparentTempRangeStart",
					"values": [{
						"value": 210,
						"unit": "k",
						"default": true
					}, {
						"value": -60,
						"unit": "c",
						"default": false
					}, {
						"value": -80,
						"unit": "f",
						"default": false
					}]
				},
				"max": {
					"key": "apparentTempRangeEnd",
					"values": [{
						"value": 334,
						"unit": "k",
						"default": true
					}, {
						"value": 60,
						"unit": "c",
						"default": false
					}, {
						"value": 140,
						"unit": "f",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 278.15,
						"unit": "k"
					}, {
						"value": 5,
						"unit": "c"
					}, {
						"value": 41,
						"unit": "f"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 308.15,
						"unit": "k"
					}, {
						"value": 35,
						"unit": "c"
					}, {
						"value": 95,
						"unit": "f"
					}]
				}
			}]
		}, {
			"id": 20,
			"code": "currentWind",
			"title": "Wind",
			"components": [{
				"type": "slider",
				"title": "Wind Speed",
				"step": 0.1,
				"unit": "speed",
				"min": {
					"key": "speedRangeStart",
					"values": [{
						"value": 0,
						"unit": "m\/s",
						"default": true
					}, {
						"value": 0,
						"unit": "km\/h",
						"default": false
					}, {
						"value": 0,
						"unit": "mph",
						"default": false
					}, {
						"value": 0,
						"unit": "knots",
						"default": false
					}]
				},
				"max": {
					"key": "speedRangeEnd",
					"values": [{
						"value": 40,
						"unit": "m\/s",
						"default": true
					}, {
						"value": 140,
						"unit": "km\/h",
						"default": false
					}, {
						"value": 80,
						"unit": "mph",
						"default": false
					}, {
						"value": 70,
						"unit": "knots",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 5.5,
						"unit": "m\/s"
					}, {
						"value": 19.8,
						"unit": "km\/h"
					}, {
						"value": 12.3,
						"unit": "mph"
					}, {
						"value": 10.7,
						"unit": "knots"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 40,
						"unit": "m\/s"
					}, {
						"value": 140,
						"unit": "km\/h"
					}, {
						"value": 80,
						"unit": "mph"
					}, {
						"value": 70,
						"unit": "knots"
					}]
				}
			}, {
				"type": "compass",
				"title": "Direction",
				"min": {
					"key": "directionRangeStart",
					"values": [{
						"value": 0,
						"unit": "°"
					}]
				},
				"max": {
					"key": "directionRangeEnd",
					"values": [{
						"value": 360,
						"unit": "°"
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 0,
						"unit": "°"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 360,
						"unit": "°"
					}]
				}
			}, {
				"type": "button",
				"title": "Any Direction",
				"values": {
					"min": {
						"value": 0,
						"unit": "°"
					},
					"max": {
						"value": 360,
						"unit": "°"
					}
				}
			}]
		}, {
			"id": 29,
			"code": "currentWindGust",
			"title": "Wind Gust",
			"components": [{
				"type": "slider",
				"title": "Wind Gust",
				"step": 0.1,
				"unit": "speed",
				"min": {
					"key": "gustSpeedRangeStart",
					"values": [{
						"value": 0,
						"unit": "m\/s",
						"default": true
					}, {
						"value": 0,
						"unit": "km\/h",
						"default": false
					}, {
						"value": 0,
						"unit": "mph",
						"default": false
					}, {
						"value": 0,
						"unit": "knots",
						"default": false
					}]
				},
				"max": {
					"key": "gustSpeedRangeEnd",
					"values": [{
						"value": 40,
						"unit": "m\/s",
						"default": true
					}, {
						"value": 140,
						"unit": "km\/h",
						"default": false
					}, {
						"value": 80,
						"unit": "mph",
						"default": false
					}, {
						"value": 70,
						"unit": "knots",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 5.5,
						"unit": "m\/s"
					}, {
						"value": 19.8,
						"unit": "km\/h"
					}, {
						"value": 12.3,
						"unit": "mph"
					}, {
						"value": 10.7,
						"unit": "knots"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 40,
						"unit": "m\/s"
					}, {
						"value": 140,
						"unit": "km\/h"
					}, {
						"value": 80,
						"unit": "mph"
					}, {
						"value": 70,
						"unit": "knots"
					}]
				}
			}]
		}],
		"forecast": [{
			"id": 10,
			"code": "forecastMoonphase",
			"title": "Moon",
			"components": [{
				"type": "slider",
				"title": "+ \/ -",
				"min": {
					"key": "surroundingDays",
					"values": [{
						"value": 0,
						"unit": "days",
						"default": true
					}]
				},
				"max": {
					"values": [{
						"value": 4,
						"unit": "days",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 1,
						"unit": "days"
					}]
				}
			}, {
				"type": "radiobutton",
				"title": "Moon Phase",
				"key": "phase",
				"groups": [{
					"inputs": [{
						"label": "Full",
						"value": "1",
						"default": true
					}, {
						"label": "Last Quarter",
						"value": "2",
						"default": false
					}, {
						"label": "New",
						"value": "3",
						"default": false
					}, {
						"label": "First Quarter",
						"value": "4",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 6,
			"code": "forecastRainfall",
			"title": "Rain (daily)",
			"components": [{
				"type": "enabler",
				"title": "No rain",
				"value": -1
			}, {
				"type": "slider",
				"title": "Rain Forecast at Least",
				"unit": "amount",
				"min": {
					"key": "amount",
					"values": [{
						"value": 0,
						"unit": "mm",
						"default": true
					}, {
						"value": 0,
						"unit": "in",
						"default": false
					}, {
						"value": 0,
						"unit": "pts",
						"default": false
					}]
				},
				"max": {
					"values": [{
						"value": 300,
						"unit": "mm",
						"default": true
					}, {
						"value": 10,
						"unit": "in",
						"default": false
					}, {
						"value": 800,
						"unit": "pts",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 150,
						"unit": "mm"
					}, {
						"value": 5.9,
						"unit": "in"
					}, {
						"value": 425,
						"unit": "pts"
					}]
				}
			}, {
				"type": "slider",
				"title": "Chance of Rain at Least",
				"unit": "percentage",
				"min": {
					"key": "probability",
					"values": [{
						"value": 0,
						"unit": "%",
						"default": true
					}]
				},
				"max": {
					"values": [{
						"value": 100,
						"unit": "%",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 50,
						"unit": "%"
					}]
				}
			}]
		}, {
			"id": 15,
			"code": "forecastHourlyRainfall",
			"title": "Rain (hourly)",
			"components": [{
				"type": "slider",
				"title": "Chance of Rain at Least",
				"unit": "percentage",
				"min": {
					"key": "probability",
					"values": [{
						"value": 0,
						"unit": "%",
						"default": true
					}]
				},
				"max": {
					"values": [{
						"value": 100,
						"unit": "%",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 50,
						"unit": "%"
					}]
				}
			}]
		}, {
			"id": 13,
			"code": "forecastRegionPrecis",
			"title": "Region Precis",
			"components": [{
				"type": "checkbox",
				"key": "precis",
				"title": "The region precis is a daily description of the weather for a particular region, written by the government authority. If one of the words below appears in the text, then the alert will trigger.",
				"groups": [{
					"title": "Cloud Cover",
					"inputs": [{
						"label": "Sunny",
						"value": "cloud-cover:sunny",
						"default": false
					}, {
						"label": "Mostly Sunny",
						"value": "cloud-cover:mostly-sunny",
						"default": false
					}, {
						"label": "Partly Cloudy",
						"value": "cloud-cover:partly-cloudy",
						"default": false
					}, {
						"label": "Mostly Cloudy",
						"value": "cloud-cover:mostly-cloudy",
						"default": false
					}, {
						"label": "Cloudy",
						"value": "cloud-cover:cloudy",
						"default": false
					}, {
						"label": "Overcast",
						"value": "cloud-cover:overcast",
						"default": false
					}]
				}, {
					"title": "Heavy Rain",
					"inputs": [{
						"label": "Heavy Rain",
						"value": "heavy-rain:heavy-rain",
						"default": false
					}]
				}, {
					"title": "Chance of Rain\/Showers",
					"inputs": [{
						"label": "Very High",
						"value": "chance-of-rain:very-high",
						"default": false
					}, {
						"label": "High",
						"value": "chance-of-rain:high",
						"default": false
					}, {
						"label": "Medium",
						"value": "chance-of-rain:medium",
						"default": false
					}, {
						"label": "Slight",
						"value": "chance-of-rain:slight",
						"default": false
					}]
				}, {
					"title": "Chance of Thunderstorms",
					"inputs": [{
						"label": "Chance of Thunderstorms",
						"value": "chance-of-thunderstorms:chance-of-thunderstorms",
						"default": false
					}, {
						"label": "Thunderstorms",
						"value": "thunderstorms:thunderstorms",
						"default": false
					}]
				}, {
					"title": "Snow",
					"inputs": [{
						"label": "Snow",
						"value": "snow:snow",
						"default": false
					}]
				}, {
					"title": "Chance of Snow\/Snow Showers",
					"inputs": [{
						"label": "Very High",
						"value": "chance-of-snow:very-high",
						"default": false
					}, {
						"label": "High",
						"value": "chance-of-snow:high",
						"default": false
					}, {
						"label": "Medium",
						"value": "chance-of-snow:medium",
						"default": false
					}, {
						"label": "Slight",
						"value": "chance-of-snow:slight",
						"default": false
					}]
				}, {
					"title": "Fog",
					"inputs": [{
						"label": "Fog",
						"value": "fog:fog",
						"default": false
					}]
				}, {
					"title": "Frost",
					"inputs": [{
						"label": "Frost",
						"value": "frost:frost",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 8,
			"code": "forecastSunriseSunset",
			"title": "Sun",
			"components": [{
				"type": "radiobutton",
				"title": "Sun",
				"key": "time",
				"groups": [{
					"inputs": [{
						"label": "Dawn - The time between first light and sunrise",
						"value": "1",
						"default": false
					}, {
						"label": "Dusk - The time between sunset and last light",
						"value": "3",
						"default": false
					}, {
						"label": "Dawn or Dusk",
						"value": "9",
						"default": false
					}, {
						"label": "Daytime - The time between sunrise and sunset",
						"value": "2",
						"default": false
					}, {
						"label": "Nighttime - The time between last light and first light",
						"value": "4",
						"default": false
					}, {
						"label": "First Light",
						"value": "5",
						"default": false
					}, {
						"label": "Sunrise",
						"value": "6",
						"default": true
					}, {
						"label": "Sunset",
						"value": "7",
						"default": false
					}, {
						"label": "Last Light",
						"value": "8",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 3,
			"code": "forecastSwell",
			"title": "Swell",
			"components": [{
				"type": "slider",
				"title": "Swell Height Range",
				"step": 0.1,
				"unit": "height",
				"min": {
					"key": "heightRangeStart",
					"values": [{
						"value": 0,
						"unit": "m",
						"default": true
					}, {
						"value": 0,
						"unit": "ft",
						"default": false
					}]
				},
				"max": {
					"key": "heightRangeEnd",
					"values": [{
						"value": 15,
						"unit": "m",
						"default": true
					}, {
						"value": 40,
						"unit": "ft",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 2,
						"unit": "m"
					}, {
						"value": 6.6,
						"unit": "ft"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 10,
						"unit": "m"
					}, {
						"value": 32.8,
						"unit": "ft"
					}]
				}
			}, {
				"type": "slider",
				"title": "Swell Period Range",
				"min": {
					"key": "periodRangeStart",
					"values": [{
						"value": 0,
						"unit": "seconds",
						"default": true
					}]
				},
				"max": {
					"key": "periodRangeEnd",
					"values": [{
						"value": 30,
						"unit": "seconds",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 2,
						"unit": "seconds"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 10,
						"unit": "seconds"
					}]
				}
			}, {
				"type": "compass",
				"title": "Direction",
				"min": {
					"key": "directionRangeStart",
					"values": [{
						"value": 0,
						"unit": "°"
					}]
				},
				"max": {
					"key": "directionRangeEnd",
					"values": [{
						"value": 360,
						"unit": "°"
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 0,
						"unit": "°"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 360,
						"unit": "°"
					}]
				}
			}, {
				"type": "button",
				"title": "Any Direction",
				"values": {
					"min": {
						"value": 0,
						"unit": "°"
					},
					"max": {
						"value": 360,
						"unit": "°"
					}
				}
			}]
		}, {
			"id": 2,
			"code": "forecastMaxTemp",
			"title": "Temp (daily max)",
			"components": [{
				"type": "slider",
				"title": "Max Temperature Forecast",
				"unit": "temperature",
				"min": {
					"key": "tempRangeStart",
					"values": [{
						"value": 210,
						"unit": "k",
						"default": true
					}, {
						"value": -60,
						"unit": "c",
						"default": false
					}, {
						"value": -80,
						"unit": "f",
						"default": false
					}]
				},
				"max": {
					"key": "tempRangeEnd",
					"values": [{
						"value": 334,
						"unit": "k",
						"default": true
					}, {
						"value": 60,
						"unit": "c",
						"default": false
					}, {
						"value": 140,
						"unit": "f",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 278.15,
						"unit": "k"
					}, {
						"value": 5,
						"unit": "c"
					}, {
						"value": 41,
						"unit": "f"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 308.15,
						"unit": "k"
					}, {
						"value": 35,
						"unit": "c"
					}, {
						"value": 95,
						"unit": "f"
					}]
				}
			}]
		}, {
			"id": 1,
			"code": "forecastMinTemp",
			"title": "Temp (daily min)",
			"components": [{
				"type": "slider",
				"title": "Min Temperature Forecast",
				"unit": "temperature",
				"min": {
					"key": "tempRangeStart",
					"values": [{
						"value": 210,
						"unit": "k",
						"default": true
					}, {
						"value": -60,
						"unit": "c",
						"default": false
					}, {
						"value": -80,
						"unit": "f",
						"default": false
					}]
				},
				"max": {
					"key": "tempRangeEnd",
					"values": [{
						"value": 334,
						"unit": "k",
						"default": true
					}, {
						"value": 60,
						"unit": "c",
						"default": false
					}, {
						"value": 140,
						"unit": "f",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 278.15,
						"unit": "k"
					}, {
						"value": 5,
						"unit": "c"
					}, {
						"value": 41,
						"unit": "f"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 308.15,
						"unit": "k"
					}, {
						"value": 35,
						"unit": "c"
					}, {
						"value": 95,
						"unit": "f"
					}]
				}
			}]
		}, {
			"id": 7,
			"code": "forecastTides",
			"title": "Tides",
			"components": [{
				"type": "radiobutton",
				"title": "Tide Status:",
				"key": "status",
				"groups": [{
					"inputs": [{
						"label": "High Tide",
						"value": "1",
						"default": true
					}, {
						"label": "Low Tide",
						"value": "2",
						"default": false
					}, {
						"label": "Half-Tide (Rising)",
						"value": "3",
						"default": false
					}, {
						"label": "Half-Tide (Falling)",
						"value": "4",
						"default": false
					}, {
						"label": "High Tide or Low Tide",
						"value": "5",
						"default": false
					}]
				}]
			}, {
				"type": "slider",
				"title": "+ \/ -",
				"step": 3,
				"min": {
					"key": "buffer",
					"values": [{
						"value": 0,
						"unit": "minutes",
						"default": true
					}]
				},
				"max": {
					"values": [{
						"value": 180,
						"unit": "minutes",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 30,
						"unit": "minutes"
					}]
				}
			}]
		}, {
			"id": 14,
			"code": "forecastDailyMaxUV",
			"title": "UV (daily max)",
			"components": [{
				"type": "slider",
				"title": "UV Forecast Index Range",
				"min": {
					"key": "indexRangeStart",
					"values": [{
						"value": 0,
						"unit": "index",
						"default": true
					}]
				},
				"max": {
					"key": "indexRangeEnd",
					"values": [{
						"value": 20,
						"unit": "index",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 3,
						"unit": "index"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 20,
						"unit": "index"
					}]
				}
			}]
		}, {
			"id": 9,
			"code": "forecastUV",
			"title": "UV (hourly)",
			"components": [{
				"type": "slider",
				"title": "UV Forecast Index Range",
				"min": {
					"key": "indexRangeStart",
					"values": [{
						"value": 0,
						"unit": "index",
						"default": true
					}]
				},
				"max": {
					"key": "indexRangeEnd",
					"values": [{
						"value": 20,
						"unit": "index",
						"default": true
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 3,
						"unit": "index"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 20,
						"unit": "index"
					}]
				}
			}]
		}, {
			"id": 5,
			"code": "forecastWeather",
			"title": "Weather (daily)",
			"components": [{
				"type": "radiobutton",
				"key": "precis",
				"title": "Daily Weather Conditions:",
				"groups": [{
					"inputs": [{
						"label": "Fine",
						"value": "fine",
						"default": true
					}, {
						"label": "Cloudy",
						"value": "cloudy",
						"default": false
					}, {
						"label": "Rain\/Showers",
						"value": "rain-showers",
						"default": false
					}, {
						"label": "Thunderstorms",
						"value": "thunderstorms",
						"default": false
					}, {
						"label": "Snow",
						"value": "snow",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 12,
			"code": "forecastHourlyPrecis",
			"title": "Weather (hourly)",
			"components": [{
				"type": "radiobutton",
				"key": "precis",
				"title": "Hourly Weather Conditions:",
				"groups": [{
					"inputs": [{
						"label": "Fine",
						"value": "fine",
						"default": true
					}, {
						"label": "Cloudy",
						"value": "cloudy",
						"default": false
					}, {
						"label": "Rain\/Showers",
						"value": "rain-showers",
						"default": false
					}, {
						"label": "Thunderstorms",
						"value": "thunderstorms",
						"default": false
					}, {
						"label": "Snow",
						"value": "snow",
						"default": false
					}]
				}]
			}]
		}, {
			"id": 4,
			"code": "forecastWind",
			"title": "Wind",
			"components": [{
				"type": "slider",
				"title": "Wind Forecast Speed",
				"step": 0.1,
				"unit": "speed",
				"min": {
					"key": "speedRangeStart",
					"values": [{
						"value": 0,
						"unit": "m\/s",
						"default": true
					}, {
						"value": 0,
						"unit": "km\/h",
						"default": false
					}, {
						"value": 0,
						"unit": "mph",
						"default": false
					}, {
						"value": 0,
						"unit": "knots",
						"default": false
					}]
				},
				"max": {
					"key": "speedRangeEnd",
					"values": [{
						"value": 40,
						"unit": "m\/s",
						"default": true
					}, {
						"value": 140,
						"unit": "km\/h",
						"default": false
					}, {
						"value": 80,
						"unit": "mph",
						"default": false
					}, {
						"value": 70,
						"unit": "knots",
						"default": false
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 5.5,
						"unit": "m\/s"
					}, {
						"value": 19.8,
						"unit": "km\/h"
					}, {
						"value": 12.3,
						"unit": "mph"
					}, {
						"value": 10.7,
						"unit": "knots"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 40,
						"unit": "m\/s"
					}, {
						"value": 140,
						"unit": "km\/h"
					}, {
						"value": 80,
						"unit": "mph"
					}, {
						"value": 70,
						"unit": "knots"
					}]
				}
			}, {
				"type": "compass",
				"title": "Direction",
				"min": {
					"key": "directionRangeStart",
					"values": [{
						"value": 0,
						"unit": "°"
					}]
				},
				"max": {
					"key": "directionRangeEnd",
					"values": [{
						"value": 360,
						"unit": "°"
					}]
				},
				"defaultMin": {
					"values": [{
						"value": 0,
						"unit": "°"
					}]
				},
				"defaultMax": {
					"values": [{
						"value": 360,
						"unit": "°"
					}]
				}
			}, {
				"type": "button",
				"title": "Any Direction",
				"values": {
					"min": {
						"value": 0,
						"unit": "°"
					},
					"max": {
						"value": 360,
						"unit": "°"
					}
				}
			}]
		}]
	}
}
```

Returns the list of all notification alert condition types and their form configurations.

### Request

`GET api.willyweather.com.au/v2/{api key}/notifications/alertconditions/config.json `

### Response

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
absolute | object | | contains grouped notification alert condition types by condition type **(see Response - Condition Type object)**

### Response - Condition Type object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
observational | array | | list of obervational notification alert condition types **(see Response - Notification Alert Condition Type object)**
forecast | array | | list of forecast notification alert condition types **(see Response - Notification Alert Condition Type object)**

### Response - Notification Alert Condition Type object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | | alert condition type id
code | string | | alert condition type code
title | string | | alert condition type name
components | object | | list of forms **(see Components object)**

### Components - Slider

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
type | string | `slider` | slider type
title | string | | Form's title
unit | string | `temperature`, `pressure`, `amount`, `speed`, `height` | if does not exist or not in any of the options, no other unit measurements are available
min | object | | **(see Components - Slider/Compass - min/max)**
max | object | | **(see Components - Slider/Compass - min/max)**
defaultMin | object | | **(see Components - Slider/Compass - defaultMin/defaultMax)**
defaultMax | object | | **(see Components - Slider/Compass - defaultMin/defaultMax)**

### Components - Radiobutton/Checkbox

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
type | string | `radiobutton` | radiobutton or checkbox type inputs
title | string | | Form's title
key | string | | the parameter key when you pass it in the request
groups | array | | **(see Components - Radiobutton/Checkbox - groups)**

### Components - Compass

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
type | string | `compass` | circular slider used in `directions`
title | string | | Form's title
value | string | | value of other inputs when `on`

### Components - Button

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
type | string | `button` | button that sets `direction` values to default
title | string | | Form's title
values | array | | value of other inputs when clicked

### Components - Enabler

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
type | string | `enabler` | checkbox that disables other inputs when `on`
title | string | | Form's title
value | string | | value of other inputs when `on`

### Components - Slider/Compass - min/max

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
key | string | | the parameter key when you pass it in the request
values | array | | **(see Components - Slider/Compass - min/max - values)**

### Components - Slider/Compass - min/max - values

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
value | string | | slider value
unit | string | | unit value
default | boolean | | default unit measurement

### Components - Slider/Compass - defaultMin/defaultMax

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
values | array | | **(see Components - Slider/Compass - defaultMin/defaultMax - values)**

### Components - Slider/Compass - defaultMin/defaultMax - values

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
value | string | | slider value
unit | string | | unit value

### Components - Radiobutton/Checkbox - groups

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
title | string | | title of this group
inputs | array | | **(see Components - Radiobutton/Checkbox - groups - inputs)**

### Components - Radiobutton/Checkbox - groups - inputs

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
label | string | | label for this input
value | string | | value of this input
default | boolean | | input is default value

### Components - Button - values

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
min | object | | **(see Components - Button - values - min/max)**
max | object | | **(see Components - Button - values - min/max)**

### Components - Button - values - min/max

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
value | int | | value to set
unit | string | | unit


## Notification - POST - Create

> Example Request Body

```json
{
    "name": "Notification name",
    "location": {
        "id": 4988
    },
    "enabled": false,
    "followMe": false,
    "notificationType": {
        "id": 3
    },
    "warningClassification": "storm",
    "notificationContacts": [
        {
            "contact": {
                "uid": "47d34901-1f7e-40ee-a973-035802054343"
            },
            "notificationTransporterType": {
                "id": 3
            }
        }
    ]
}
```

> Example Response

```json
[
    {
        "uid": "baf25b09-6723-4b73-b07c-f8e138a83405",
        "name": "Notification name",
        "enabled": true,
        "notificationType": {
            "id": 1,
            "name": "alert"
        },
        "notificationContacts": [
            {
                "id": null,
                "contact": {
                    "id": 9,
                    "firstName": "John",
                    "lastName": "Doe",
                    "email": "johndoe@willyweather.com",
                    "phone": "",
                    "webhook": "",
                    "isDefault": true
                },
                "notificationTransporterType": {
                    "id": 3,
                    "name": "ios"
                }
            }
        ],
        "followMe": false,
        "location": {
            "id": 2212,
            "name": "Collaroy Beach",
            "region": "Sydney",
            "state": "NSW",
            "postcode": "2097",
            "timeZone": "Australia\/Sydney",
            "lat": -33.7321,
            "lng": 151.3012,
            "typeId": 2,
            "distance": 9.2
        },
        "createdDateTime": "2021-06-28 09:26:27",
        "warningType": {
            "id": 1,
            "code": "severe-weather",
            "name": "Severe Weather",
            "classification": "storm",
            "warningSeverityLevels": [
                {
                    "id": 1,
                    "code": "yellow"
                }, {
                    "id": 2,
                    "code": "amber"
                }, {
                    "id": 3,
                    "code": "red"
                }
            ]
        }
    }
]
```

Creates a notification.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
name | string | | | true
location | object | **(See Location)**  | The location this notification is for. | true
enabled | boolean | | | true
followMe | false | | Updates location of notification | true
notificationType | object | See <a href="#notification-type">Notification Type</a> | | true
notificationContacts | array | **(See Notification Contact)** | | true
lat | foat | | | false (required if for `alert`)
lng | float | | | false (required if for `alert`)
intensity | int | `1`, `2`, `3`, `4`, `5` | intensity levels | false (required if for `alert`)
duration | int | | | false (required if for `alert`)
notificationMinTime | int | 0 - 1439 | Time when the notification starts (in minutes) | false (required if for `alert`)
notificationMaxTime | int| 0 - 1439 | Time when the notification ends (in minutes) | false (required if for `alert`)
notifyMeOffset | int | | | false (required if for `alert`)
warningType | int | | | false (either `warningType` or `warningClassification` is required if for `warning`)
warningClassification | string | See `classifications` in <a href="#warning-types">Warning Types</a> | | false (either `warningType` or `warningClassification` is required if for `warning`)
warningSeverityLevels | csv | `1`, `2`, `3` |  | false

### Location

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | | | true

### Notification Contact

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
contact | object | **(See Contact)** | | true
notificationTransporterType | object | See <a href="#notification-transporter-type">Notification Transporter Type</a> | | true

### Contact

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
uid | string | | | true

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is a Notification object. See <a href="#notification-get-all-notifications">Notifications</a> for description of a Notification response.

## Notification - PUT - Update

> Example Request Body

```json
{
    "name": "Notification name",
    "enabled": false,
    "followMe": false,
    "notificationType": {
        "id": 1
    },
    "duration": 3,
    "notifyMeOffset": 1,
    "intensity": 3,
    "lat": -33.7321,
    "lng": 151.3012,
    "notificationTime": {
        "startTime": 75,
        "endTime": 100
    },
    "notificationContacts": [
        {
            "contact": {
                "uid": "2c1b8e02-4872-4452-a989-2343e10135e1"
            },
            "notificationTransporterType": {
                "id": 3
            }
        },
        {
            "contact": {
                "uid": "2c1b8e02-4872-4452-a989-2343e10135e1"
            },
            "notificationTransporterType": {
                "id": 4
            }
        }
    ],
    "notificationAlertConditions": [
    {
            "tempRangeStart": 290,
            "tempRangeEnd": 310,
            "notificationAlertConditionType": {
                "id": 2
            },
            "location": {
                "id": 4988
            },
            "group": 0
        }
    ]
}
```

> Example Response

```json
{
    "uid": "baf25b09-6723-4b73-b07c-f8e138a83405",
    "name": "Notification name",
    "enabled": true,
    "notificationType": {
        "id": 1,
        "name": "alert"
    },
    "notificationContacts": [
        {
            "id": null,
            "contact": {
                "id": 9,
                "firstName": "John",
                "lastName": "Doe",
                "email": "johndoe@willyweather.com",
                "phone": "",
                "webhook": "",
                "isDefault": true
            },
            "notificationTransporterType": {
                "id": 3,
                "name": "ios"
            }
        }
    ],
    "followMe": false,
    "location": {
        "id": 2212,
        "name": "Collaroy Beach",
        "region": "Sydney",
        "state": "NSW",
        "postcode": "2097",
        "timeZone": "Australia\/Sydney",
        "lat": -33.7321,
        "lng": 151.3012,
        "typeId": 2,
        "distance": 9.2
    },
    "createdDateTime": "2021-06-28 09:26:27",
    "notificationAlertConditions": [
        {
            "id": 1,
            "notificationAlertConditionType": {
                "id": 2,
                "code": "forecast-max-temp"
            },
            "tempRangeStart": 290,
            "tempRangeEnd": 310
        }
    ]
}
```

Updates a notification. Only for alert.

### Request

`PUT api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications/{notification uid}.json`

See <a href="#notification-post-create">Notifications Create</a> for Request Parameters.

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is a Notification object. See <a href="#notification-get-all-notifications">Notifications</a> for description of a Notification response.

## Notification - PUT - Update Enabled State

> Example Request Body

```json
{
    "notifications": [
        {
            "uid": "6469b8e4-5bcb-4a40-804a-f1c136338f61"
        },
        {
            "uid": "7d0e6509-f9c1-431d-a0b0-2619d27ed68f"
        }
    ],
    "enabled": false
}
```

> Example Response

```json
{}
```

Updates multiple notification's _enabled_ state.

### Request

`PUT api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
notifications | array | **(See Notifications below)** | | true
enabled | boolean | | | true
offset | int | | Number of minutes added to now to determine how long the notification will be disabled | true

<aside class="notice">
    Either <code>enabled</code> or <code>offset</code> must be defined in the request but not both.
</aside>

### Notifications

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
uid | string | | | true

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an empty object.

## Notification - DELETE - Delete

> Example Request Body

```json
{}
```

> Example Response

```json
[]
```

Deletes a notification.

### Request

`DELETE api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications/{uid}.json`

### Response

Response is an empty object.

## Notification - DELETE - Bulk Delete

> Example Request Body

```json
[
    {
        "uid": "117d017a-aa74-4413-9730-1424b0a2b427"
    },
    {
        "uid": "a5538ce8-b09f-4079-867a-d2df47cbdcc8"
    },
    {
        "uid": "5ea0b752-ead2-4367-9d62-ff9c5ad6d7ef"
    },
]
```

> Example Response

```json
[]
```

Delete multiple notifications.

### Request

`DELETE api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
 | array | **(See Notifications below)** | An array of notifications | true

### Notifications

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
uid | string | | | true

### Response

Response is an empty object.


## Notification - POST - Alert - Create

> Example Request Body

```json
{
  "name": "This is a new name",
  "notificationType": {
    "id": 1
  },
  "location": {
    "id": 4988
  },
  "enabled": false,
  "followMe": false,
  "notificationContacts": [{
    "contact": {
      "uid": "47d34901-1f7e-40ee-a973-035802054343"
    },
    "notificationTransporterType": {
      "id": 3
    }
  }],
  "notifyMeOffset": 0,
  "notificationDays": {
    "mon": true,
    "tue": true,
    "wed": true,
    "thu": false,
    "fri": true,
    "sat": true,
    "sun": true
  },
  "notificationMonths": {
    "jan": true,
    "feb": false,
    "mar": true,
    "apr": true,
    "may": true,
    "jun": false,
    "jul": true,
    "aug": true,
    "sep": true,
    "oct": true,
    "nov": true,
    "dec": true
  },
  "notificationTimeRanges": [{
    "startTime": 60,
    "endTime": 120
  }, {
    "startTime": 300,
    "endTime": 500
  }],
  "notificationDateRanges": [{
    "startDate": "2021-01-01",
    "endDate": "2021-03-31"
  }, {
    "startDate": "2021-05-01",
    "endDate": "2021-11-30"
  }],
  "notificationAlertConditions": [{
    "tempRangeStart": 295.0,
    "tempRangeEnd": 300.2,
    "location": {
      "id": 1
    },
    "notificationAlertConditionType": {
      "id": 1
    },
    "group": 0
  },
    {
      "precis": "fine",
      "location": {
        "id": 1
      },
      "notificationAlertConditionType": {
        "id": 12
      },
      "group": 1
    },
    {
      "tempRangeStart": 300.0,
      "tempRangeEnd": 301.2,
      "location": {
        "id": 1
      },
      "notificationAlertConditionType": {
        "id": 2
      },
      "group": 1
    }]
}
```

> Example Response

```json
[{
  "uid": "ea3ea8a0-527e-4fcc-bcf7-ac8fb479e962",
  "name": "This is a new name",
  "enabled": false,
  "notificationType": {
    "id": 1,
    "name": "alert"
  },
  "notificationContacts": [{
    "id": null,
    "contact": {
      "uid": "47d34901-1f7e-40ee-a973-035802054343",
      "firstName": "dashboard",
      "lastName": "",
      "email": "dash@willyweather.com",
      "isEmailVerified": false,
      "phone": "",
      "isPhoneVerified": false,
      "webhook": "",
      "isDefault": true
    },
    "notificationTransporterType": {
      "id": 3,
      "name": "ios"
    }
  }],
  "followMe": false,
  "location": {
    "id": 4988,
    "name": "Bondi Beach",
    "region": "Sydney",
    "state": "NSW",
    "postcode": "2026",
    "timeZone": "Australia\/Sydney",
    "lat": -33.8905,
    "lng": 151.2749,
    "typeId": 2
  },
  "createdDateTime": "2021-01-01 00:00:00",
  "notifyMeOffset": 0,
  "notificationDays": {
    "mon": true,
    "tue": true,
    "wed": true,
    "thu": false,
    "fri": true,
    "sat": true,
    "sun": true
  },
  "notificationMonths": {
    "jan": true,
    "feb": false,
    "mar": true,
    "apr": true,
    "may": true,
    "jun": false,
    "jul": true,
    "aug": true,
    "sep": true,
    "oct": true,
    "nov": true,
    "dec": true
  },
  "notificationTimeRanges": [{
    "startTime": 60,
    "endTime": 120
  }, {
    "startTime": 300,
    "endTime": 500
  }],
  "notificationDateRanges": [{
    "startDate": "2021-01-01",
    "endDate": "2021-03-31"
  }, {
    "startDate": "2021-05-01",
    "endDate": "2021-11-30"
  }],
  "notificationAlertConditions": [{
    "tempRangeStart": 295.0,
    "tempRangeEnd": 300.2,
    "location": {
      "id": 1
    },
    "notificationAlertConditionType": {
      "id": 1
    },
    "group": 0
  },
    {
      "precis": "fine",
      "location": {
        "id": 1
      },
      "notificationAlertConditionType": {
        "id": 12
      },
      "group": 1
    },
    {
      "tempRangeStart": 300.0,
      "tempRangeEnd": 301.2,
      "location": {
        "id": 1
      },
      "notificationAlertConditionType": {
        "id": 2
      },
      "group": 1
    }],
  "units": {
    "temperature": "k",
    "tideHeight": "m",
    "swellHeight": "m",
    "speed": "m\/s",
    "amount": "mm",
    "distance": "km",
    "pressure": "millibars",
    "cloud": "oktas"
  }
}]
```

Creates an alert notification.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
name | string | | | true
location | object | **(See Location)**  | The location this notification is for. | true
enabled | boolean | | | true
followMe | boolean | | Updates location of notification | true
notificationType | object | **(See Notification Type)** |  | true
notifyMeOffset | int | | | true
notificationContacts | array | **(See Notification Contact)** | | true
notificationDateRanges | array | **(Notification Date Range)** | date ranges that the notification is active | true*
notificationMonths | object | **(See Notification Months)** | months that the notification is active | true*
notificationDays | object | **(See Notification Days)** | days that the notification is active | true*
notificationTimeRanges | object | **(See Notification Time Range)** | time ranges that the notification is active | true
notificationAlertConditions | array | **(See Notification Alert Conditions)** | | true

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters. This endpoint only supports json requests.
</aside>
<aside class="notice">
    * At least one of the following: <code>notificationDateRanges</code>, <code>notificationDays</code>, or <code>notificationMonths</code> must be defined for the notification to be considered valid.
</aside>

### Location

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | | | true

### Notification Type

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | 1 | 1 for alert | true

### Notification Contact

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
contact | object | **(See Contact)** | | true
notificationTransporterType | object | See <a href="#notification-transporter-type">Notification Transporter Type</a> | | true

### Contact

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
uid | string | | | true

### Notification Date Range

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
startDate | string | | <code>YYYY-MM-DD</code> | true
endDate | string | | <code>YYYY-MM-DD</code> | true

### Notification Months

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
jan | boolean | | | true
feb | boolean | | | true
mar | boolean | | | true
apr | boolean | | | true
may | boolean | | | true
jun | boolean | | | true
jul | boolean | | | true
aug | boolean | | | true
sep | boolean | | | true
oct | boolean | | | true
nov | boolean | | | true
dec | boolean | | | true

### Notification Days

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mon | boolean | | | true
tue | boolean | | | true
wed | boolean | | | true
thu | boolean | | | true
fri | boolean | | | true
sat | boolean | | | true
sun | boolean | | | true

### Notification Days

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
startTime | int | 0 - 1439 | Time when the notification starts (in minutes) | true
endTime | int| 0 - 1439 | Time when the notification ends (in minutes) | true

### Notification Alert Conditions

<aside class="notice">
    * Each entry in the notificationAlertCondition must specify <code>notificationAlertConditionType</code>, <code>group</code> and a data source (<code>weatherStation</code> for current types, <code>location</code> for forecast types, unless specifically stated supporting other types)
</aside>
<aside class="notice">
    <code>group</code> attribute is used to denote groupings of alert conditions. For conditions that belong to the same group, the logical operator will be an AND, that means that the group evaluation will be true if all the conditions in the group returns true. If there are 2 or more groups, each group will be treated as an OR, that means that if one of the groups returns true the total result of the evaluation will be true.
</aside>

### Notification Alert Condition Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | | `1` for `forecast-min-temp`<br/> `2` for `forecast-max-temp`<br/> `3` for `forecast-swell`<br/> `4` for `forecast-wind`<br/> `5` for `forecast-weather`<br/> `6` for `forecast-rainfall`<br/> `7` for `forecast-tides`<br/> `8` for `forecast-sunrise-sunset`<br/> `9` for `forecast-uv`<br/> `10` for `forecast-moonphase`<br/> `11` for `forecast-radar`<br/> `12` for `forecast-hourly-precis`<br/> `13` for `forecast-region-precis`<br/> `14` for `forecast-daily-max-uv`<br/> `20` for `current-wind`<br/> `21` for `current-temp`<br/> `22` for `current-rain-last-hour`<br/> `24` for `current-humidity`<br/> `25` for `current-dewpoint`<br/> `26` for `current-pressure`<br/> `27` for `current-delta-t`<br/> `28` for `current-apparent-temp`<br/> `29` for `current-wind-gust`<br/> `30` for `current-cloud`<br/> 

### Location

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | | Location id which will be used as the datasource for the alert condition | true

### Weather Station

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | | Weather Station id which will be used as the datasource for the alert condition | true

### Forecast Daily Max UV

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
indexRangeStart | float | 0 - 20 | | true
indexRangeEnd | float | 0 - 20 | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Hourly Precis

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
precis | string | fine <br/> cloudy <br/ > rain-showers <br/> thunderstorms <br/> snow | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Max Temperature

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
tempRangeStart | float | 273.15 - 323.15 | value is in kelvin | true
tempRangeEnd | float | 273.15 - 323.15 | value is in kelvin | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Min Temperature

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
tempRangeStart | float | 253.15 - 303.15 | value is in kelvin | true
tempRangeEnd | float | 253.15 - 303.15 | value is in kelvin | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Moonphase

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
phase | int | `1` for `full`<br/> `2` for `last quarter`<br/> `3` for `new`<br/> `4` for `first quarter`<br/>  | | true
surroundingDays | int | 0 - 4 | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Radar

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
mapDataPoint | object | **(See Map Data Point)** | | false
minimumIntensity | int | 0 - 5 (no rain, light, moderate, heavy, very heavy) | | true
lat | float | | | true*
lng | float | | | true*
imminentMessageFalsePositiveReductionEnabled | boolean | | | true
rainAlertBoundaryMessageEnabled | boolean | | | true
headsUpMessageEnabled | boolean | | | true
headsUpMessageAdvancedWarningMinutes | int | | | true
headsUpMessageFalsePositiveReductionEnabled | boolean | | | true
imminentLowPredictabilityMinutes | int | | | true
imminentMediumPredictabilityMinutes | int | | | true
imminentHighPredictabilityMinutes | int | | | true
imminentMessageResetEnabled | boolean | | | true
imminentMessageResetMinutesForCloudy | int | | | true
briefShowerLengthEnabled | boolean | | | true
briefShowerLengthMinutes| int | | | true
clearingAfterBriefShowerMinutes | int | | | true
rainArrivedMessageEnabled | boolean | | | true
imminentMessageResetMinutesForPartlyCloudy | int | | | true
location | object | **(See Location)** | | true*
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | 0 | | | true

### Map Data Point

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
id | int | | | | true 

<aside class="notice">
    <code>notificationAlertConditions</code> for forecast radar can only contain 1 entry and that entry must have a group of <code>0</code>.
</aside>

<aside class="notice">
    *Either <code>lat</code> and <code>lng</code>  or <code>location</code> must be defined but not both.
</aside>

<aside class="notice">
    If <code>mapDataPoint</code> is not provided, closest radar station from <code>lat</code> and <code>lng</code> or <code>location</code> will be used.
</aside>

### Forecast Rainfall

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
probability | int | 1 - 100  | | true
amount | int | -1 - 100 | -1 (no rain)| true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Region Precis

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
cloudCover | string | sunny <br/> mostly-sunny <br/> partly-cloudy <br/> mostly-cloudy <br/> cloudy <br/> overcast | | false
heavyRain | string | heavy-rain | | false
chanceOfRain | string | slight <br/> medium <br/> high <br/> very-high | | false
chanceOfThunderstorms | string | chance-of-thunderstorms | | false
thunderstorms | string | thunderstorms | | false
snow | string | snow | | false
chanceOfSnow | string | slight <br/> medium <br/> high <br/> very-high | | false
fog | string | fog | | false
frost | string | frost | | false
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

<aside class="notice">
    * At least one of the following must be defined for region precis: <code>cloudCover</code>, <code>heavyRain</code>, <code>chanceOfThunderstorms</code>, <code>thunderstorms</code>, <code>snow</code>, <code>chanceOfSnow</code>, <code>fog</code>, <code>frost</code>.
</aside>

### Forecast Sunrise Sunset

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
time | int | `1` for `dawn`<br/> `2` for `daytime`<br/> `3` for `dusk`<br/> `4` for `night time`<br/>  `5` for `first light`<br/> `6` for `sunrise`<br/> `7` for `sunset`<br/> `8` for `last light`<br/> `9` for `dusk or dawn` | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Swell

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
heightRangeStart | float | 0 - 20 | value is in meters | true
heightRangeEnd | float | 0 - 20 | value is in meters | true
periodRangeStart | float | 0 - 20 | | true
periodRangeEnd | float | 0 - 20 | | true
directionRangeStart | float | 0 - 360 | | true
directionRangeEnd | float | 0 - 360 | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Tides

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
status | int | `1` for `high tide`<br/> `2` for `low tide`<br/> `3` for `half tide rising`<br/> `4` for `half tide falling`<br/>  `5` for `high or low tide` | | true
buffer | int | 0 - 180 | value must be divisible by 3 | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast UV

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
indexRangeStart | float | 0 - 20 | | true
indexRangeEnd | float | 0 - 20 | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Weather

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
precis | string | fine <br/> cloudy <br/ > rain-showers <br/> thunderstorms <br/> snow | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Forecast Wind

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
speedRangeStart | float | 0 - 41.7 | value is in meters per second | true
speedRangeEnd | float | 0 - 41.7 | value is in meters per second | true
directionRangeStart | float | 0 - 360 | | true
directionRangeEnd | float | 0 - 360 | | true
location | object | **(See Location)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Apparent Temperature

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
apparentTempRangeStart | float | 253.15 - 323.15 | value is in kelvin | true
apparentTempRangeEnd | float | 253.15 - 323.15 | value is in kelvin | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Cloud

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
cloudRangeStart | int | 0 - 8| oktas | true
cloudRangeEnd | int | 0 - 8 | oktas | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Delta T

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
deltaTRangeStart | float | 273.15 - 293.15 | value is in kelvin | true
deltaTRangeEnd | float | 273.15 - 293.15 | value is in kelvin | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Dewpoint

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
dewpointRangeStart | float | 273.15 - 303.15 | value is in kelvin | true
dewpointRangeEnd | float | 273.15 - 303.15 | value is in kelvin | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Humidity

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
humidityRangeStart | float | 0 - 100 | | true
humidityRangeEnd | float | 0 - 100 | | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Pressure

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
pressureRangeStart | float | 850 - 1100 | value is in millibars | true
pressureRangeStart | float | 850 - 1100 | value is in millibars | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Rainfall Last Hour

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
amount | int | -1 - 50 | -1 (no rain)| true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Temperature

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
tempRangeStart | float | 253.15 - 323.15 | value is in kelvin | true
tempRangeEnd | float | 253.15 - 323.15 | value is in kelvin | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Wind Gust

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
gustSpeedRangeStart | float | 0 - 41.7 | value is in meters per second | true
gustSpeedRangeStart | float | 0 - 41.7 | value is in meters per second | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Current Wind

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
speedRangeStart | float | 0 - 41.7 | value is in meters per second | true
speedRangeEnd | float | 0 - 41.7 | value is in meters per second | true
directionRangeStart | float | 0 - 360 | | true
directionRangeEnd | float | 0 - 360 | | true
weatherStation | object | **(See Weather Station)** | | true
notificationAlertConditionType | object | **(See Notification Alert Condition Type)** | | true
group | int | | | | true

### Response

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
uid | string | |
name | string | |
enabled | boolean | |
notificationType | object | | **(see Notification Type object)**
notificationContacts | array | | an array of Notification Contact objects **(see Notification Contact)**
followMe | boolean | |
location | string | |  **(See <a href="#location-get-by-location-id">Locations</a> object)**
createdDateTime | string | | Created date time (YYYY-MM-DD HH:MM:SS`)
notifyMeOffset | int | | In minutes. Can be null.
notificationDateRanges | array | **(Notification Date Range)** | date ranges that the notification is active
notificationMonths | object | **(See Notification Months)** | months that the notification is active
notificationDays | object | **(See Notification Days)** | days that the notification is active
notificationTimeRanges | object | **(See Notification Time Range)** | time ranges that the notification is active
notificationAlertConditions | array | **(See Notification Alert Conditions)** | conditions that make the notification valid

### Notification Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1`| `1` for alert
name | string | `alert` |

### Notification Contact

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int |  |
contact | object |  | **(See <a href="#contact-get-by-contact-uid">Contact</a> object)**|
notificationTransporterType | object |  | **(see Notification Transporter Type object)**

### Notification Transporter Type

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | `1` for email <br/> `2` for sms <br/> `3` for ios <br/> `4` for android <br/> `5` for webhook |
name | string | `email`, `sms`, `ios`, `android`, `webhook` |

### Notification Date Range

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
startDate | string | | <code>YYYY-MM-DD</code> | true
endDate | string | | <code>YYYY-MM-DD</code> | true

### Notification Months

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
jan | boolean | |
feb | boolean | |
mar | boolean | |
apr | boolean | |
may | boolean | |
jun | boolean | | 
jul | boolean | |
aug | boolean | |
sep | boolean | |
oct | boolean | |
nov | boolean | |
dec | boolean | |

### Notification Days

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
mon | boolean | |
tue | boolean | |
wed | boolean | |
thu | boolean | |
fri | boolean | |
sat | boolean | |
sun | boolean | |

### Notification Times

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
startTime | int | 0 - 1439 | Time when the notification starts (in minutes)
endTime | int| 0 - 1439 | Time when the notification ends (in minutes)

### Notification Alert Conditions

<aside class="notice">
    * An array of Notification Alert Conditions. See <a href="#notification-post-alert-create">Notification Alert Conditions Section</a> for the list of possible values.
</aside>


## Notification - PUT - Alert - Update

> Example Request Body

```json
{
  "name": "This a new name",
  "notificationType": {
    "id": 1
  },
  "location": {
    "id": 4988
  },
  "enabled": false,
  "followMe": false,
  "notificationContacts": [{
    "contact": {
      "uid": "47d34901-1f7e-40ee-a973-035802054343"
    },
    "notificationTransporterType": {
      "id": 3
    }
  }],
  "notifyMeOffset": 0,
  "notificationDays": {
    "mon": true,
    "tue": true,
    "wed": true,
    "thu": false,
    "fri": true,
    "sat": true,
    "sun": true
  },
  "notificationMonths": {
    "jan": true,
    "feb": false,
    "mar": true,
    "apr": true,
    "may": true,
    "jun": false,
    "jul": true,
    "aug": true,
    "sep": true,
    "oct": true,
    "nov": true,
    "dec": true
  },
  "notificationTimeRanges": [{
    "startTime": 60,
    "endTime": 120
  }, {
    "startTime": 300,
    "endTime": 500
  }],
  "notificationDateRanges": [{
    "startDate": "2021-01-01",
    "endDate": "2021-03-31"
  }, {
    "startDate": "2021-05-01",
    "endDate": "2021-11-30"
  }],
  "notificationAlertConditions": [{
    "tempRangeStart": 295.0,
    "tempRangeEnd": 300.2,
    "location": {
      "id": 1
    },
    "notificationAlertConditionType": {
      "id": 1
    },
    "group": 0
  },
    {
      "precis": "fine",
      "location": {
        "id": 1
      },
      "notificationAlertConditionType": {
        "id": 12
      },
      "group": 1
    },
    {
      "tempRangeStart": 300.0,
      "tempRangeEnd": 301.2,
      "location": {
        "id": 1
      },
      "notificationAlertConditionType": {
        "id": 2
      },
      "group": 1
    }]
}
```

> Example Response

```json
{}
```

Updates an alert notification.

### Request

`PUT api.willyweather.com.au/v2/{api key}/accounts/{account uid}/notifications/{notification uid}.json`

See <a href="#notification-post-alert-create">Notifications - Alert - Create</a> for Request Parameters.

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters. This endpoint only supports json requests.
</aside>

### Response

Response is an empty object.

## Notification - Push Notification Payload

### IOS

> Example IOS Payload

```json
{
	"APNS": {
		"aps": {
			"alert": {
				"title": "Observational with all conditions",
				"body": "Your alert conditions were met at 4:00am on Friday the 4th of March.",
				"category": "AlertNotification",
				"mutable-content": 1,
				"utcDeliveryDateTime": "2011-03-04 04:00:00"
			},
			"sound": "default"
		},
		"willyweather": {
			"redirect": {
				"screen": "alertNotification",
				"parameters": {
					"groupedNotificationAlertConditions": [{
						"occurrences": [{
							"dateTime": "2011-03-04 04:00:00",
							"criteria": [{
								"id": "currentWind",
								"rule": {
									"speed": {
										"rangeStart": 90,
										"rangeEnd": 144
									},
									"direction": {
										"rangeStart": 45,
										"rangeEnd": 45
									}
								},
								"match": {
									"speed": 129.6,
									"direction": 300,
									"directionText": "WNW"
								},
								"units": {
									"speed": "km/h"
								}
							}, {
								"id": "currentTemp",
								"rule": {
									"temperature": {
										"rangeStart": -3.15,
										"rangeEnd": 23.85
									},
									"trend": "any"
								},
								"match": {
									"temperature": 6.9,
									"trend": "falling"
								},
								"units": {
									"temperature": "c"
								}
							}, {
								"id": 22,
								"rule": {
									"amount": {
										"rangeStart": 1
									}
								},
								"match": {
									"amount": 5
								},
								"units": {
									"amount": "mm"
								}
							}, {
								"id": "currentRainSince9am",
								"rule": {
									"amount": {
										"rangeStart": 4
									}
								},
								"match": {
									"amount": 5.2
								},
								"units": {
									"amount": "mm"
								}
							}, {
								"id": 5,
								"type": "currentHumidity",
								"rule": {
									"humidity": {
										"rangeStart": 35,
										"rangeEnd": 90
									}
								},
								"match": {
									"humidity": 35
								},
								"units": {
									"humidity": "%"
								}
							}, {
								"id": "currentDewpoint",
								"rule": {
									"dewpoint": {
										"rangeStart": -13.15,
										"rangeEnd": 11.85
									},
									"trend": "any"
								},
								"match": {
									"dewpoint": -3.1,
									"trend": "falling"
								},
								"units": {
									"dewpoint": "c"
								}
							}, {
								"id": "currentPressure",
								"rule": {
									"pressure": {
										"rangeStart": 851,
										"rangeEnd": 1000
									},
									"trend": "any"
								},
								"match": {
									"pressure": 900.4,
									"trend": "falling"
								},
								"units": {
									"pressure": "hpa"
								}
							}, {
								"id": "currentApparentTemp",
								"rule": {
									"apparentTemperature": {
										"rangeStart": -3.15,
										"rangeEnd": 23.85
									}
								},
								"match": {
									"apparentTemperature": 6.9
								},
								"units": {
									"temperature": "c"
								}
							}, {
								"id": "currentCloud",
								"rule": {
									"cloud": {
										"rangeStart": 0,
										"rangeEnd": 8
									},
									"trend": "any"
								},
								"match": {
									"cloud": 5,
									"trend": "rising"
								},
								"units": {
									"cloud": "oktas"
								}
							}, {
								"id": "currentWindGust",
								"rule": {
									"speed": {
										"rangeStart": 0,
										"rangeEnd": 144
									}
								},
								"match": {
									"speed": 72
								},
								"units": {
									"speed": "km/h"
								}
							}, {
								"id": "currentDeltaT",
								"rule": {
									"deltaT": {
										"rangeStart": 0,
										"rangeEnd": 20
									},
									"trend": "any"
								},
								"match": {
									"deltaT": 10,
									"trend": "steady"
								},
								"units": {
									"temperature": "c"
								}
							}]
						}]
					}],
					"location": {
						"id": 1,
						"name": "Bondi",
						"displayName": "Bondi Display Name",
						"region": "Sydney",
						"state": "NSW",
						"postcode": "2026",
						"timeZone": "Australia/NSW",
						"lat": -33.8905,
						"lng": 151.2749,
						"url": "https%3A%2F%2Fwww.willyweather.com.au%2Fnsw%2Fsydney%2Fbondi.html"
					},
					"weatherStations": {
						"temperature": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"delta-t": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"cloud": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"humidity": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"dewPoint": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"pressure": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"wind": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"rainfall": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						}
					}
				}
			},
			"countryCode": "AU",
			"speech": "Observational with all conditions"
		}
	}
}
```
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
APNS | object | **(See APNS)** |

#### APNS

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
aps | object | **(See aps)** |
willyweather | object | **(See willyweather)** |

##### APS

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
alert | object | **(See alert)** |
sound | string | default | The sound that will be played for this notification

#### alert

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
title | string | | title of the notification
body | string | | body content of the notification
category | string | AlertNotification |
mutable-content | int | 1 |
utcDeliveryDateTime | string | |

#### willyweather

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
redirect | object | **(See redirect)** |
countryCode | string | AU, US, UK |
speech | string | |

#### redirect

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
screen  | string | |
parameters | object | **(See parameters)** | Contains the conditions for the notificatio and the matching rules and criteria

#### parameters

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
groupedNotificationAlertConditions  | array |  | Array of objects where each object has a single attribute named occurrences. **(See occurrences)**
location | object | **(See location)** | (optional) will be shown if there is a forecast type alert condition in the groupedNotificationAlertConditions
weatherStations | array | ***(See weatherStations)** | (optional) will be shown if there is a observational type alert condition in the groupedNotificationAlertConditions

#### occurrence

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | dateTime where criteria types was satisfied by matching data
criteria | array| **(See Grouped Notification Alert Conditions Criteria Types)** | Array of Criteria Types that matched the rule set in notification alert conditions.

#### location

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | | Location id
name | string | | Name of the location
displayName | string | | Display name of the location
region | string | | Region of the location
state | string | | State of the location
postalcode | string | |
timeZone| string | |
lat | float | |
lng | float | |
url | string  | | URL link in WillyWeather for the location

#### weatherStations

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | **(See WeatherStation below)** | | Weather Station for used as source for temperature data
delta-t | **(See WeatherStation below)** | | Weather Station for used as source for delta-t  data
cloud | **(See WeatherStation below)** | | Weather Station for used as source for cloud data
humidity | **(See WeatherStation below)** | | Weather Station for used as source for dewPoint data
pressure | **(See WeatherStation below)** | | Weather Station for used as source for pressure data
wind | **(See WeatherStation below)** | | Weather Station for used as source for wind data
rainfall | **(See WeatherStation below)** | | Weather Station for used as source for rainfall data

#### WeatherStations

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
distance | float | |

### Android

> Example Android Payload

```json
{
	"GCM": {
		"priority": "high",
		"data": {
			"category": "AlertNotification",
			"notification": {
				"title": "Observational with all conditions",
				"body": "Your alert conditions were met at 4:00am on Friday the 4th of March.",
				"sound": "default"
			},
			"redirect": {
				"screen": "AlertNotification",
				"parameters": {
					"groupedNotificationAlertConditions": [{
						"occurrences": [{
							"dateTime": "2011-03-04 04:00:00",
							"criteria": [{
								"id": "currentWind",
								"rule": {
									"speed": {
										"rangeStart": 90,
										"rangeEnd": 144
									},
									"direction": {
										"rangeStart": 45,
										"rangeEnd": 45
									}
								},
								"match": {
									"speed": 129.6,
									"direction": 300,
									"directionText": "WNW"
								},
								"units": {
									"speed": "km/h"
								}
							}, {
								"id": "currentTemp",
								"rule": {
									"temperature": {
										"rangeStart": -3.15,
										"rangeEnd": 23.85
									},
									"trend": "any"
								},
								"match": {
									"temperature": 6.9,
									"trend": "falling"
								},
								"units": {
									"temperature": "c"
								}
							}, {
								"id": 22,
								"rule": {
									"amount": {
										"rangeStart": 1
									}
								},
								"match": {
									"amount": 5
								},
								"units": {
									"amount": "mm"
								}
							}, {
								"id": "currentRainSince9am",
								"rule": {
									"amount": {
										"rangeStart": 4
									}
								},
								"match": {
									"amount": 5.2
								},
								"units": {
									"amount": "mm"
								}
							}, {
								"id": 5,
								"type": "currentHumidity",
								"rule": {
									"humidity": {
										"rangeStart": 35,
										"rangeEnd": 90
									}
								},
								"match": {
									"humidity": 35
								},
								"units": {
									"humidity": "%"
								}
							}, {
								"id": "currentDewpoint",
								"rule": {
									"dewpoint": {
										"rangeStart": -13.15,
										"rangeEnd": 11.85
									},
									"trend": "any"
								},
								"match": {
									"dewpoint": -3.1,
									"trend": "falling"
								},
								"units": {
									"dewpoint": "c"
								}
							}, {
								"id": "currentPressure",
								"rule": {
									"pressure": {
										"rangeStart": 851,
										"rangeEnd": 1000
									},
									"trend": "any"
								},
								"match": {
									"pressure": 900.4,
									"trend": "falling"
								},
								"units": {
									"pressure": "hpa"
								}
							}, {
								"id": "currentApparentTemp",
								"rule": {
									"apparentTemperature": {
										"rangeStart": -3.15,
										"rangeEnd": 23.85
									}
								},
								"match": {
									"apparentTemperature": 6.9
								},
								"units": {
									"temperature": "c"
								}
							}, {
								"id": "currentCloud",
								"rule": {
									"cloud": {
										"rangeStart": 0,
										"rangeEnd": 8
									},
									"trend": "any"
								},
								"match": {
									"cloud": 5,
									"trend": "rising"
								},
								"units": {
									"cloud": "oktas"
								}
							}, {
								"id": "currentWindGust",
								"rule": {
									"speed": {
										"rangeStart": 0,
										"rangeEnd": 144
									}
								},
								"match": {
									"speed": 72
								},
								"units": {
									"speed": "km/h"
								}
							}, {
								"id": "currentDeltaT",
								"rule": {
									"deltaT": {
										"rangeStart": 0,
										"rangeEnd": 20
									},
									"trend": "any"
								},
								"match": {
									"deltaT": 10,
									"trend": "steady"
								},
								"units": {
									"temperature": "c"
								}
							}]
						}]
					}],
					"location": {
						"id": 1,
						"name": "Bondi",
						"displayName": "Bondi Display Name",
						"region": "Sydney",
						"state": "NSW",
						"postcode": "2026",
						"timeZone": "Australia/NSW",
						"lat": -33.8905,
						"lng": 151.2749,
						"url": "https%3A%2F%2Fwww.willyweather.com.au%2Fnsw%2Fsydney%2Fbondi.html"
					},
					"weatherStations": {
						"temperature": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"delta-t": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"cloud": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"humidity": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"dewPoint": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"pressure": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"wind": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						},
						"rainfall": {
							"id": 1,
							"name": "station 1",
							"lat": 1,
							"lng": -1,
							"distance": 3
						}
					}
				}
			},
			"countryCode": "AU",
			"speech": "Observational with all conditions"
		}
	}
}
```
Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
GCM | object | **(See GCM)** |

#### GCM

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
priority | string | high |
data | object | **(See data)** | 

#### data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
category | string | AlertNotification | 
notification | object | **(See notification)** | 
redirect | object | **(See redirect)** |

#### notification

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
title | string | | title of the notification
body | string | | body content of the notification
sound | string | default | The sound that will be played for this notification

#### redirect

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
screen  | string | |
parameters | object | **(See parameters)** | Contains the conditions for the notificatio and the matching rules and criteria

#### parameters

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
groupedNotificationAlertConditions  | array |  | Array of objects where each object has a single attribute named occurrences. **(See occurrences)**
location | object | **(See location)** | (optional) will be shown if there is a forecast type alert condition in the groupedNotificationAlertConditions
weatherStations | array | ***(See weatherStations)** | (optional) will be shown if there is a observational type alert condition in the groupedNotificationAlertConditions

#### occurrence

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
dateTime | string | | dateTime where criteria types was satisfied by matching data
criteria | array| **(See Grouped Notification Alert Conditions Criteria Types)** | Array of Criteria Types that matched the rule set in notification alert conditions.

#### location

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | | Location id
name | string | | Name of the location
displayName | string | | Display name of the location
region | string | | Region of the location
state | string | | State of the location
postalcode | string | |
timeZone| string | |
lat | float | |
lng | float | |
url | string  | | URL link in WillyWeather for the location

#### weatherStations

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
temperature | **(See WeatherStation below)** | | Weather Station for used as source for temperature data
delta-t | **(See WeatherStation below)** | | Weather Station for used as source for delta-t  data
cloud | **(See WeatherStation below)** | | Weather Station for used as source for cloud data
humidity | **(See WeatherStation below)** | | Weather Station for used as source for dewPoint data
pressure | **(See WeatherStation below)** | | Weather Station for used as source for pressure data
wind | **(See WeatherStation below)** | | Weather Station for used as source for wind data
rainfall | **(See WeatherStation below)** | | Weather Station for used as source for rainfall data

#### WeatherStations

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
id | int | |
name | string | |
lat | float | |
lng | float | |
distance | float | |


## Grouped Notification Alert Conditions Criteria Types

### Forecast Daily Max UV

> Example Forecast Daily Max UV

```json
{
	"id": "forecastDailyMaxUV",
	"rule": {
		"index": {
			"rangeStart": 3,
			"rangeEnd": 12
		}
	},
	"match": {
		"index": 8.6,
		"scale": "very-high"
	}
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastDailyMaxUV |
rule | object | | The condition that the user has set
match | object | | The data that satisfied the rule


#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
indexRangeStart | float | 0 - 20 |
indexRangeEnd | float | 0 - 20 |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
index | int | 0 - 20 |
scale | float | Low <br/> Moderate <br/> High <br/> Very High <br/> Extreme |


### Forecast Hourly Precis

> Example Forecast Hourly Precis

```json
{
  "id": "forecastHourlyPrecis",
  "rule": {
    "precis": "fine"
  },
  "match": {
    "precis": "fine"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastHourlyPrecis |
rule | object | |
match | object | |


#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
precis | string | fine <br/> cloudy <br/ > rain-showers <br/> thunderstorms <br/> snow  |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
precis | string | fine <br/> cloudy <br/ > rain-showers <br/> thunderstorms <br/> snow  |


### Forecast Max Temperature

> Example Forecast Max Temperature

```json
{
  "id": "forecastMaxTemp",
  "rule": {
    "temperature": {
      "rangeStart": 10,
      "rangeEnd": 15
    }
  },
  "match": {
    "temperature": 12
  },
  "units": {
    "temperature": "c"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastMaxTemp |
rule | object | |
match | object | |
units | object | |


#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | object | | **(See temperature)**

#### temperature

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | float | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | string | c  <br/> f <br/> k <br/> |

### Forecast Min Temperature

> Example Forecast Min Temperature

```json
{
  "id": "forecastMinTemp",
  "rule": {
    "temperature": {
      "rangeStart": 10,
      "rangeEnd": 15
    }
  },
  "match": {
    "temperature": 12
  },
  "units": {
    "temperature": "c"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastMinTemp |
rule | object | |
match | object | |
units | object | |


#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | object | | **(See temperature)**

#### temperature

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | float | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | string | c  <br/> f <br/> k <br/> |

### Forecast Moonphase

> Example Forecast Moonphase

```json
{
  "id": "forecastMoonphase",
  "rule": {
    "phase": "new",
    "surroundingDays": 4
  },
  "match": {
    "phase": "new",
    "date": "2011-03-05"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastMoonphase |
rule | object | |
match | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
phase | string | | full <br/> last-quarter <br/> new <br/> first-quarter |
surroundingDays | int | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
phase | string | | full <br/> last-quarter <br/> new <br/> first-quarter |
date | string | |


### Forecast Rainfall

> Example Forecast Rainfall

```json
{
  "id": "forecastRainfall",
  "rule": {
    "probability": 19,
    "amount": 9
  },
  "match": {
    "probability": 20,
    "amount": 10
  },
  "units": {
    "amount": "mm"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastRainfall |
rule | object | |
match | object | |
units | object | | (optional) - Present if amount is set

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
probability | int | | |
amount | int | | (optional)

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
probability | int | | |
amount | int | | (optional)

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
amount | string | mm  <br/> pts <br/> in <br/> | (optional) - Present if amount is set

### Forecast Region Precis

> Example Forecast Region Precis

```json
{
  "id": "forecastRegionPrecis",
  "rule": {
    "precis": {
      "cloud-cover": ["sunny", "mostly-sunny", "partly-cloudy", "mostly-cloudy", "cloudy", "overcast"],
      "heavy-rain": ["heavy-rain"],
      "chance-of-rain": ["very-high", "high", "medium", "slight"],
      "chance-of-thunderstorms": ["chance-of-thunderstorms"],
      "thunderstorms": ["thunderstorms"],
      "snow": ["snow"],
      "chance-of-snow": ["very-high", "high", "medium", "slight"],
      "fog": ["fog"],
      "frost": ["frost"]
    }
  },
  "match": {
    "precis": {
      "cloud-cover": "sunny",
      "chance-of-rain": "very-high",
      "chance-of-thunderstorms": "chance-of-thunderstorms"
    }
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastRegionPrecis |
rule | object | |
match | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
precis | object | **(See Region Precis Criteria)** | Each criteria can have 1 more or values

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
precis | object | **(See Region Precis Criteria)** | Each criteria will have a single value which indicates the precis that satisfied the rule

### Region Precis Criteria

Parameter | Type | Options | Description |
--------- | ---- | ------- | ----------- |
cloudCover | string | sunny <br/> mostly-sunny <br/> partly-cloudy <br/> mostly-cloudy <br/> cloudy <br/> overcast |
heavyRain | string | heavy-rain |
chanceOfRain | string | slight <br/> medium <br/> high <br/> very-high |
chanceOfThunderstorms | string | chance-of-thunderstorms |
thunderstorms | string | thunderstorms |
snow | string | snow |
chanceOfSnow | string | slight <br/> medium <br/> high <br/> very-high |
fog | string | fog |
frost | string | frost |

<aside class="notice">
    * At least one of the following must be defined for region precis: <code>cloudCover</code>, <code>heavyRain</code>, <code>chanceOfThunderstorms</code>, <code>thunderstorms</code>, <code>snow</code>, <code>chanceOfSnow</code>, <code>fog</code>, <code>frost</code>.
</aside>

### Forecast Sunrise Sunset

> Example Forecast Sunrise Sunset

```json
{
  "id": "forecastSunriseSunset",
  "rule": {
    "startDateTime": "daytime"
  },
  "match": {
    "startDateTime": "2011-03-05 06:45:58",
    "endDateTime": "2011-03-05 19:26:54"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastSunriseSunset |
rule | object | |
match | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
startDateTime | string | dawn <br/> daytime <br/> dusk <br/> nighttime <br/>  first-light <br/> sunrise <br/> sunset<br/> last light <br/> dusk-or-dawn |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
startDateTime | string | (dateTime) |
endDateTime | string | (dateTime) | optional


### Forecast Swell

> Example Forecast Swell

```json
{
  "id": "forecastSwell",
  "rule": {
    "height": {
      "rangeStart": 5,
      "rangeEnd": 8
    },
    "period": {
      "rangeStart": 2,
      "rangeEnd": 5
    },
    "direction": {
      "rangeStart": 0,
      "rangeEnd": 360
    }
  },
  "match": {
    "height": 5,
    "period": 5,
    "direction": 5,
    "directionText": "N"
  },
  "units": {
    "height": "m",
    "period": "seconds"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastSwell |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
height | object | **(See height)** |
period | object | **(See period)** |
direction | object | **(See direction)** |

#### height

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float |  |
rangeEnd | float | |

#### period

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | int | 0 - 20 |
rangeEnd | int | 0 - 20 |

#### direction

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | 0 - 360 |
rangeEnd | float | 0 - 360 |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
height | float | |
period | float | |
direction | float | |
directionText | string | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
height | string | ft <br/> m <br/> |
period | string | seconds |

### Forecast Tides

> Example Forecast Tides

```json
{
  "id": "forecastTides",
  "rule": {
    "status": "high-tide"
  },
  "match": {
    "status": "high",
    "dateTime": "2011-03-04 15:23:00"
  },
  "units": {
    "height": "m"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastTides |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
status | string | high-tide <br/> low-tide <br/> half-tide-rising <br/> half-tide-falling <br/>  high-or-low-tide |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
status | string | high <br/> low <br/> half-tide-rising <br/> half-tide-falling |
dateTime | string | (dateTime) |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
height | string | ft <br/> m <br/> |

### Forecast UV

> Example Forecast UV

```json
{
  "id": "forecastUV",
  "rule": {
    "index": {
      "rangeStart": 3,
      "rangeEnd": 12
    }
  },
  "match": {
    "index": 8.6,
    "scale": "very-high"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastUV |
rule | object | |
match | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
index | object | **(See index)** |

#### index

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | 0 - 20 |
rangeEnd | float | 0 - 20 |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
index | int | |
scale | string | Low <br/> Moderate <br/> High <br/> Very High <br/> Extreme <br/> |


### Forecast Weather

> Example Forecast Weather

```json
{
  "id": "forecastWeather",
  "rule": {
    "precis": "fine"
  },
  "match": {
    "precis": "fine"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastWeather |
rule | object | |
match | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
precis | string | fine <br/> cloudy <br/ > rain-showers <br/> thunderstorms <br/> snow |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
precis | string |  fine <br/> cloudy <br/ > rain-showers <br/> thunderstorms <br/> snow |


### Forecast Wind

> Example Forecast Wind

```json
{
  "id": "forecastWind",
  "rule": {
    "speed": {
      "rangeStart": 19.8,
      "rangeEnd": 144
    },
    "direction": {
      "rangeStart": 0,
      "rangeEnd": 180
    }
  },
  "match": {
    "speed": 136.8,
    "direction": 110,
    "directionText": "ESE"
  },
  "units": {
    "speed": "km\/h"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | forecastWind |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | object | **(See speed)** |
direction | object | **(See direction)** |

#### speed

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float |  |
rangeEnd | float | |

#### direction

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | 0 - 360 |
rangeEnd | float | 0 - 360 |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | float | |
direction | float | |
directionText | string | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | string | knots <br/> m/s <br/> km/h <br/> mph |


### Current Apparent Temperature

> Example Current Apparent Temperature

```json
{
  "id": "currentApparentTemp",
  "rule": {
    "apparentTemperature": {
      "rangeStart": -3.15,
      "rangeEnd": 23.85
    }
  },
  "match": {
    "apparentTemperature": 6.9
  },
  "units": {
    "temperature": "c"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentApparentTemp |
rule | object | |
match | object | |
units | object | |


#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
apparentTemperature | object | **(See apparentTemperature)** |

#### apparentTemperature

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
apparentTemperature | float | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | string | c  <br/> f <br/> k <br/> |

### Current Cloud

> Example Current Cloud

```json
{
  "id": "currentCloud",
  "rule": {
    "cloud": {
      "rangeStart": 0,
      "rangeEnd": 8
    },
    "trend": "any"
  },
  "match": {
    "cloud": 5,
    "trend": "rising"
  },
  "units": {
    "cloud": "oktas"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentCloud |
rule | object | |
match | object | |
units | object | |


#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
cloud | object | **(See apparentTemperature)** |
trend | string | failing <br/> rising <br/> steady <br/> |

#### cloud

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
cloud | int | |
trend | string | failing <br/> rising <br/> steady <br/> |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
cloud | string | oktas |

### Current Delta T

> Example Current Delta T

```json
{
  "id": "currentDeltaT",
  "rule": {
    "deltaT": {
      "rangeStart": 0,
      "rangeEnd": 20
    },
    "trend": "any"
  },
  "match": {
    "deltaT": 10,
    "trend": "steady"
  },
  "units": {
    "temperature": "c"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentDeltaT |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
deltaT | object | **(See deltaT)** |
trend | string | failing <br/> rising <br/> steady <br/> |

#### deltaT

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
deltaT | float | |
trend | string | failing <br/> rising <br/> steady <br/>  |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | string | c  <br/> f <br/> k <br/> |

### Current Dewpoint

> Example Current Dewpoint

```json
{
  "id": "currentDewpoint",
  "rule": {
    "dewpoint": {
      "rangeStart": -13.15,
      "rangeEnd": 11.85
    },
    "trend": "any"
  },
  "match": {
    "dewpoint": -3.1,
    "trend": "falling"
  },
  "units": {
    "dewpoint": "c"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentDewpoint |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
dewpoint | object | **(See dewpoint)** |
trend | string | failing <br/> rising <br/> steady <br/> |

#### dewpoint

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
dewpoint | float | |
trend | string | failing <br/> rising <br/> steady |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | string | c  <br/> f <br/> k |

### Current Humidity

> Example Current Humidity

```json
{
  "id": 5,
  "type": "currentHumidity",
  "rule": {
    "humidity": {
      "rangeStart": 35,
      "rangeEnd": 90
    }
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentHumidity |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
humidity | object | **(See humidity)** |

#### humidity

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
humidity | float | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
humidity | string | % |

### Current Pressure

> Example Current Pressure

```json
{
  "id": "currentPressure",
  "rule": {
    "pressure": {
      "rangeStart": 851,
      "rangeEnd": 1000
    },
    "trend": "any"
  },
  "match": {
    "pressure": 900.4,
    "trend": "falling"
  },
  "units": {
    "pressure": "hpa"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentPressure |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
pressure | object | **(See pressure)** |
trend | string | failing <br/> rising <br/> steady |

#### pressure

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
pressure | float | |
trend | string | failing <br/> rising <br/> steady

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
pressure | string | mmhg  <br/> millibars <br/> inhg <br/> psi |

### Current Rainfall Last Hour

> Example Current Rainfall Last Hour

```json
{
  "id": "currentRainLastHour",
  "rule": {
    "amount": {
      "rangeStart": 4
    }
  },
  "match": {
    "amount": 5.2
  },
  "units": {
    "amount": "mm"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentRainLastHour |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
amount | object | | **(See amount)**

#### amount

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | string/float | No Rain/number

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
amount | float | |


#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
amount | string | mm  <br/> pts <br/> in <br/> |

### Current Temperature

> Example Current Temperature

```json
{
  "id": "currentTemp",
  "rule": {
    "temperature": {
      "rangeStart": -3.15,
      "rangeEnd": 23.85
    },
    "trend": "any"
  },
  "match": {
    "temperature": 6.9,
    "trend": "falling"
  },
  "units": {
    "temperature": "c"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentTemp |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | object | **(See temperature)** |
trend | string | failing <br/> rising <br/> steady <br/> |

#### temperature

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | float | |
trend | string | failing <br/> rising <br/> steady <br/>  |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
temperature | string | c  <br/> f <br/> k <br/> |

### Current Wind Gust

> Example Current Wind Gust

```json
{
  "id": "currentWindGust",
  "rule": {
    "speed": {
      "rangeStart": 0,
      "rangeEnd": 144
    }
  },
  "match": {
    "speed": 72
  },
  "units": {
    "speed": "km\/h"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentWindGust |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | object | **(See speed)** |

#### speed

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float |  |
rangeEnd | float | |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | float | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | string | knots <br/> m/s <br/> km/h <br/> mph |

### Current Wind

> Example Current Wind

```json
{
  "id": "currentWind",
  "rule": {
    "speed": {
      "rangeStart": 0,
      "rangeEnd": 144
    },
    "direction": {
      "rangeStart": 0,
      "rangeEnd": 0
    }
  },
  "match": {
    "speed": 129.6,
    "direction": 300,
    "directionText": "WNW"
  },
  "units": {
    "speed": "km\/h"
  }
}
```

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
id | string | currentWind |
rule | object | |
match | object | |
units | object | |

#### Rule

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | object | **(See speed)** |
direction | object | **(See direction)** |

#### speed

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float |  |
rangeEnd | float | |

#### direction

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
rangeStart | float | 0 - 360 |
rangeEnd | float | 0 - 360 |

#### Match

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | float | |
direction | float | |
directionText | string | |

#### Units

Parameter | Type | Options | Description
--------- | ---- | ------- | -----------
speed | string | knots <br/> m/s <br/> km/h <br/> mph |

## Subscription - POST - Validation - IOS

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"receiptData" : "{a really long string}"
	}
}
```

Calls https://buy.itunes.apple.com/verifyReceipt to validate subscription receipt.

### Request

`POST api.willyweather.com.au/v2/{api key}/subscription/validate.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
receiptData | string | | | true

### Response

<aside class="notice">
    Refer to https://developer.apple.com/documentation/appstorereceipts/responsebody
</aside>

## Units - GET - All units by Account uid

> Example Request Header

```json
{}
```

> Example Response

```json
{
    "temperature": "f",
    "tideHeight": "ft",
    "swellHeight": "m",
    "amount": "mm",
    "speed": "km\/h",
    "distance": "miles",
    "pressure": "hpa"
}
```

Returns the list of account's prefered units.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/units.json`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is units. See <a href="#units">Units</a> for a description of a Unit response.

## Units - POST - Update

> Example Request Body

```json
{
    "units": {
        "distance": "km",
        "temperature": "c",
        "speed": "knots",
        "amount": "in",
        "tideHeight": "m",
        "swellHeight": "ft",
        "pressure": "mmhg"
    }
}
```

> Example Response

```json
{}
```

Updates account's prefered units.

### Request

`POST api.willyweather.com.au/v2/{api key}/accounts/{account uid}/units.json`

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
units | object | **(See Units below)** | | true

### Units

Parameter | Type | Options | Description | Required
--------- | ---- | ------- | ----------- | --------
amount | string | `mm`, `pts`, `in` | | false
distance | string | `km`, `miles` | | false
speed | string | `km/h`, `mph`, `m/s, knots` | | false
swellHeight | string | `m`, `ft` | | false
temperature | string | `c`, `f` | | false
tideHeight | string | `m`, `ft` | | false
pressure | string | `hpa`, `mmhg`, `inhg`, `psi`, `millibars` | | false

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response

Response is an empty object.

# Units

> Example Query String Request

```shell
?units=amount:mm,distance:km,speed:knots,swellHeight:ft,temperature:f,tideHeight:m,pressure:hpa
```

> Example Request Header

```json
{
	"CONTENT_TYPE": "application/json",
	"HTTP_X_PAYLOAD": {
		"units": {
			"amount": "mm",
			"distance": "km",
			"speed": "knots",
			"swellHeight": "ft",
			"temperature": "c",
			"tideHeight": "ft",
			"pressure": "hpa"
		}
	}
}
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
        "tideHeight": "ft",
        "pressure": "hpa"
    }
}
```

The Units parameter allows the data to be converted to a specific unit. The format of the preferred units will form part of the query string.

### Request

`GET ?units=amount:mm,distance:km,speed:knots,swellHeight:ft,temperature:f,tideHeight:m,pressure:hpa`

<aside class="notice">
    Request header <code>Content-type: application/json</code> is required when passing parameters via <strong>Request Header</strong>.
</aside>

### Response
Attribute | Type | Values | Description
--------- | ---- | ------- | -----------
amount | string | `mm`, `pts`, `in` | millimetres, points, inches
distance | string | `km`, `miles` | kilometers, miles
speed | string | `km/h`, `mph`, `m/s`, `knots` | kilometers per hour, miles per hour, meters per second, knots
swellHeight | string | `m`, `ft` | meters, feet
temperature | string | `c`, `f` | celsius, fahrenheit
tideHeight | string | `m`, `ft` | meters, feet
pressure | string | `hpa`, `mmhg`, `inhg`, `psi`, `millibars` | hectopascal, millimeters of mercury, inch of mercury, pounds per square inch, millibars


<script>
const urlSearchParams = new URLSearchParams(window.location.search);
const params = Object.fromEntries(urlSearchParams.entries());

// remove sections if not dev
$(document).ready(function() {
	$(window).on('load', function() {
		if (params.dev !== "true") {
			// remove in left panel
			$('#tocify-header11').remove();

			// remove in middle panel
			var indexStart = $('.content [data-unique="accounts"]').index(); // start element to remove
			var indexEnd = $('.content [data-unique="units"]').index(); // end element remove (but do not remove this element)
			$(".content").children().slice(indexStart, indexEnd).remove();
		}
	});
});
</script>