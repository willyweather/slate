---
title: API Reference

toc_footers:
  - <a href='/info/api.html'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

## Notification - Forecast Debugger - Get Logs

> Example Request Header

```json
{}
```

> Example Response

```json
[{
	"notificationUID": "22222222-0f30-4ee7-97f8-29abeecd247a",
	"messageTypeId": 11,
	"messageTypeText": "Rain Cleared In X Minutes",
	"dateTime": "2023-08-24 17:39:00"
}, {
	"notificationUID": "22222222-0f30-4ee7-97f8-29abeecd247a",
	"messageTypeId": 10,
	"messageTypeText": "Rain Cleared",
	"dateTime": "2023-08-24 17:29:00"
}, {
	"notificationUID": "22222222-0f30-4ee7-97f8-29abeecd247a",
	"messageTypeId": 0,
	"messageTypeText": "",
	"dateTime": "2023-08-24 15:49:00"
}]
```

Get Forecast Radar Notification Debugger's logs.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/forecast-debugger/logs.json`

### Response - Log Object

Sorted by `datetime` **DESC**.

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
notificationUID | string | | 
messageTypeId | string | | **(See Message Type)**
messageTypeText | string | | **(See Message Type)**
dateTime | string | |

### Message Type

Id | Message Type Text
--------- | ---- 
1 | Heads Up
2 | Imminent With Prior Heads Up
3 | Imminent
4 | Cancellation
5 | Recap Message
6 | Rain Arrived With Prior Heads Up
7 | Rain Arrived
8 | Device Outside Radar
9 | Device Inside Radar
10 | Rain Cleared
11 | Rain Cleared In X Minutes


<aside class="notice">If <code>messageTypeId</code> is not in the list, <code>messageTypeText</code> is <code>""</code>.</aside>

## Notification - Forecast Debugger - Get Data
> Example Request Header

```json
{}
```

> Example Response

```json
{
	"mapBounds": {
		"sw": {
			"lat": -45,
			"lng": 110
		},
		"ne": {
			"lat": -8,
			"lng": 155
		}
	},
	"mapProviders": [{
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
		"overlayPath": "https:\/\/cdn2.willyweather.com.au",
		"overlays": [{
			"dateTime": "2023-08-23 05:04:00",
			"name": "radar\/71-202308230504.png"
		}],
		"classification": "radar",
		"mapLegend": {
			"keys": [{
				"colour": "#000012",
				"range": {
					"min": 17,
					"max": 18
				},
				"label": "label 9"
			}, {
				"colour": "#000013",
				"range": {
					"min": 19,
					"max": 20
				},
				"label": "label 10"
			}, {
				"colour": "#000014",
				"range": {
					"min": 21,
					"max": 22
				},
				"label": "label 11"
			}]
		},
		"status": {
			"code": "active",
			"description": [{
				"text": "Currently active.",
				"meta": "text"
			}]
		},
		"nextIssueDateTime": "2014-03-26 23:06:00"
	}, {
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
		"overlayPath": "https:\/\/cdn2.willyweather.com.au",
		"overlays": [{
			"dateTime": "2023-08-23 06:01:30",
			"name": "forecast-regional-radar\/img-2023-08-23-05-59-71_step_0_mid_1_denoised.png"
		}],
		"classification": "forecast-regional-radar",
		"mapLegend": {
			"keys": [{
				"colour": "#000012",
				"range": {
					"min": 17,
					"max": 18
				},
				"label": "label 9"
			}, {
				"colour": "#000013",
				"range": {
					"min": 19,
					"max": 20
				},
				"label": "label 10"
			}, {
				"colour": "#000014",
				"range": {
					"min": 21,
					"max": 22
				},
				"label": "label 11"
			}]
		},
		"status": {
			"code": "active",
			"description": [{
				"text": "Currently active.",
				"meta": "text"
			}]
		},
		"nextIssueDateTime": "2014-03-26 23:06:00"
	}],
	"logData": {
		"notification": 6,
		"uid": "22222222-0f30-4ee7-97f8-29abeecd247a",
		"timezone": "Australia\/Sydney",
		"maplocation": 71,
		"latitude": -33.8739,
		"longitude": 151.1927,
		"dataPointLogs": [{
			"radarDataPoint": {
				"mapDataPointId": 71,
				"dateTime": "2023-08-23 05:04:00",
				"intensity": {
					"intensityValue": 0,
					"startRange": 0,
					"endRange": 0.2,
					"classification": "NO_RAIN"
				},
				"blob": {
					"blobSize": 0,
					"blobClassification": "SMALL"
				},
				"type": "ACTUAL",
				"blobRainMass": "NONE",
				"bucket": "willyweather-overlays-au",
				"path": "radar\/71-202308230504.png",
				"createdAt": "2023-08-23 05:05:33",
				"width": 0,
				"height": 0
			},
			"localForecastDateTime": "2023-08-23 15:04:00",
			"metadata": "Frame 1, Timestamp: 2023-08-23 15:04:00, Intensity: no-rain(0), Blob: small-blob(0), Blob Rain Mass: none, Type: actual<br>Current Time : 2023-08-23 16:04:19",
			"messageTypeToRuleLogsMap": null,
			"isMatchingDataPoint": false,
			"timestamp": "1692767040000"
		}, {
			"radarDataPoint": {
				"mapDataPointId": 71,
				"dateTime": "2023-08-23 06:01:30",
				"intensity": {
					"intensityValue": 1,
					"startRange": 0.2,
					"endRange": 0.5,
					"classification": "LIGHT"
				},
				"blob": {
					"blobSize": 270,
					"blobClassification": "MEDIUM"
				},
				"type": "FORECAST",
				"blobRainMass": "MEDIUM",
				"bucket": "willyweather-overlays-au",
				"path": "forecast-regional-radar\/img-2023-08-23-05-59-71_step_0_mid_1_denoised.png",
				"createdAt": "2023-08-23 06:03:33",
				"width": 0,
				"height": 0
			},
			"localForecastDateTime": "2023-08-23 16:01:30",
			"metadata": "Frame 13, Timestamp: 2023-08-23 16:01:30, Intensity: light(1), Blob: medium-blob(270), Blob Rain Mass: medium, Type: forecast<br>Current Time : 2023-08-23 16:04:19",
			"messageTypeToRuleLogsMap": {
				"Imminent": [{
					"rule": "Notification Status Rule",
					"result": true,
					"description": "Notification status is enabled",
					"conditions": [{
						"left": {
							"name": "Actual Notification Status",
							"value": "enabled"
						},
						"operator": "EQUAL",
						"right": {
							"name": "Expected",
							"value": "enabled"
						},
						"result": true
					}]
				}],
				"Imminent With Prior Heads Up": [{
					"rule": "Notification Status Rule",
					"result": true,
					"description": "Notification status is enabled",
					"conditions": [{
						"left": {
							"name": "Actual Notification Status",
							"value": "enabled"
						},
						"operator": "EQUAL",
						"right": {
							"name": "Expected",
							"value": "enabled"
						},
						"result": true
					}]
				}],
				"Rain Cleared In X Minutes": [{
					"rule": "Current Rain Status Rule",
					"result": false,
					"conditions": [{
						"left": {
							"name": "Frame 8 intensity",
							"value": "no-rain"
						},
						"operator": "NOT_EQUAL",
						"right": {
							"name": "Intensity",
							"value": "no-rain"
						},
						"result": false
					}]
				}],
				"Heads Up": [{
					"rule": "Notification Status Rule",
					"result": true,
					"description": "Notification status is enabled",
					"conditions": [{
						"left": {
							"name": "Actual Notification Status",
							"value": "enabled"
						},
						"operator": "EQUAL",
						"right": {
							"name": "Expected",
							"value": "enabled"
						},
						"result": true
					}]
				}]
			},
			"isMatchingDataPoint": false,
			"timestamp": "1692770490000"
		}],
		"latency": "00:03:20",
		"location": "Blackwattle Bay Marina",
		"satelliteForecastTimestamps": ["2023-08-23 05:00:00", "2023-08-23 05:05:00"],
		"mock": false,
		"dateTime": "2023-08-23 15:59:00",
		"isPremium": false
	},
	"logInfo": {
		"notificationUid": "22222222-0f30-4ee7-97f8-29abeecd247a",
		"dateTime": "2023-08-24 17:39:00",
		"messageTypeId": 11
	}
}
```

Get Forecast Radar Notification Debugger's data.

### Request

`GET api.willyweather.com.au/v2/{api key}/accounts/{account uid}/forecast-debugger/data.json`

### Response - Map Bounds Object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
sw | object | | **(See Map Bounds - Lat Lng)**
ne | object | | **(See Map Bounds - Lat Lng)**

### Response - Map Bounds - Lat Lng Object

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
lat | float | |
lng | float | |

<aside class="notice">This is the map bounds of the country.</aside>

### Response - Map Provider

Response is an array of Map Providers. See <a href="#map-provider">Map Provider</a> for a description of a Map Provider response.

<aside class="notice">Note: The overlays matches the <strong>Log Data - Data Point Logs - timestamp</strong></aside>

### Response - Log Data

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
notification | int | | notification id
uid | string | | notification uid
timezone | string | <a href="http://php.net/manual/en/timezones.php">php timezone</a> | (e.g. Sydney/Australia)
maplocation | int | | map provider id
latitude | float | | map provider lat
longitude | float | | map provider lng
dataPointLogs | array | | array of Data Point Logs **(See Data Point Logs)**
latency | time | | `HH:MM:SS`
location | string | | map provider name
satelliteForecastTimestamps | array | | array of datetimes `YYYY-MM-DD HH:MM:SS`
mock | boolean | | if this is just a test data then `true`
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
isPremium | boolean | |

### Response - Data Point Logs

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
radarDataPoint | object | | **(See Data Point Logs - Radar Data Point)**
localForecastDateTime | string | | `YYYY-MM-DD HH:MM:SS`
metadata | string | | description
messageTypeToRuleLogsMap | object | | can be `null`. **(See Data Point Logs - Message Type To Rule Logs Map)**
isMatchingDataPoint | boolean | | 
timestamp | float | | This value is the same as **Map Provider - Overlay - datetime** but in timestamp

### Response - Data Point Logs - Radar Data Point

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
mapDataPointId | int | |
dateTime | string | | `YYYY-MM-DD HH:MM:SS`
intensity | object | | **(See Data Point Logs - Radar Data Point - Intensity)**
blob | object | | **(See Data Point Logs - Radar Data Point - Blob)**
type | string | `ACTUAL`, `FORECAST` |
blobRainMass | string | `NONE`, `SMALL`, `MEDIUM`|
bucket | string | | S3 Bucket
path | string | | Image path. This value is the same as **Map Provider - Overlay - name**
createdAt | string | | `YYYY-MM-DD HH:MM:SS`
width | int | |
height | int | |

### Response - Data Point Logs - Radar Data Point - Intensity

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
intensityValue | int | `0`, `1`, `2`, `3`, `4`| **(See Data Point Logs - Radar Data Point - Intensity - Value - Classification)**
startRange | float | |
endRange | float | |
classification | string | `NO_RAIN`, `LIGHT`, `MODERATE`, `HEAVY`, `VERY_HEAVY` | **(See Data Point Logs - Radar Data Point - Intensity - Value - Classification)**

### Response - Data Point Logs - Radar Data Point - Intensity - Value - Classification

Id | Classification
--------- | ---- 
0 | NO_RAIN
1 | LIGHT
2 | MODERATE
3 | HEAVY
4 | VERY_HEAVY

### Response - Data Point Logs - Radar Data Point - Blob

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
blobSize | int | |
blobClassification | string | `NONE`, `SMALL`, `MEDIUM`|

### Response - Data Point Logs - Message Type To Rule Logs Map

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
`Heads Up`, `Imminent With Prior Heads Up`, `Imminent`, `Cancellation`, `Recap Message`, `Rain Arrived With Prior Heads Up`, `Rain Arrived`, `Device Outside Radar`, `Device Inside Radar`, `Rain Cleared`, `Rain Cleared In X Minutes` | array | | array of Message Type To Rule Log. **(See Data Point Logs - Message Type To Rule Log)**

<aside class="notice">Attributes are <a href="#message-type">Message Type Texts</a>.</aside>

### Response - Data Point Logs - Message Type To Rule Log

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
rule | string | |
result | boolean | |
description | string | | Optional. Can be not included in the response.
conditions | array | | array of Conditions. **(See Data Point Logs - Message Type To Rule Log - Conditions)**

### Response - Data Point Logs - Message Type To Rule Log - Conditions

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
left | object | | **(See Data Point Logs - Message Type To Rule Log - Conditions - Left/Right)**
operator | string | |
right | object | | **(See Data Point Logs - Message Type To Rule Log - Conditions - Left/Right)**
result | boolean | |

### Response - Data Point Logs - Message Type To Rule Log - Conditions - Left/Right

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
name | string | | description
value | string | |

### Response - Log Info

This is the information about the log file

Attribute | Type | Values | Description
--------- | ---- | ------ | -----------
notificationUID | string | | 
dateTime | string | |
messageTypeId | string | | **(See Message Type)**

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
