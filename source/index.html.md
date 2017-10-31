---
title: API Reference for Libra Project

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

<!-- toc_footers: -->
  <!-- - <a href='#'>Sign Up for a Developer Key</a> -->
  <!-- - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a> -->

<!-- includes: -->
  <!-- - errors -->

search: true
---

# Introduction 簡介

**問題描述**
C&W的45Gbit/s高頻線纜在cabling製程中，因轉盤轉速、張力等差異，導致在某個時間點絞製出來的線品質不佳

**預期產出**
一個可以不間斷收集機台數據的簡易資料收集系統
1. 不間斷紀錄6~24小時的cable生產時的相關量測數據
2. 具有基本的資料庫系統
3. 做簡單的數據圖表檢視/時間篩選, 觀察與不良品之關聯性

**文檔**
本文檔主要即說明後端伺服器所提供之API，提供前端和Gateway使用


**資料庫Schema**
![image_schema](images/schema.png)

# Authentication

There's no authentication in this project.

# Raws

<!-- =============================================================== -->
## Get a Specific Raw

```shell
# using curl
curl http://path_to_host/api/raw/rid

# using httpie
http get http://path_to_host/api/raw/rid
```

```python
import requests
response = requests.get('http://path_to_host/api/raw/rid')
```

> The above command returns JSON structured like this:

```json
{
  "acx": 100,
  "acy": 100,
  "acz": 102,
  "event_id": 2,
  "grx": 90,
  "gry": 84,
  "grz": 80,
  "rid": 33,
  "sid": 3,
  "ts_ms": "2017-09-27T16:09:27.241892"
}
```

This endpoint retrieves a specific raw record.

### HTTP Request

`GET http://path_to_host/api/raw/rid`


<!-- =============================================================== -->
## Get Filtered Raws

```shell
# using curl
curl http://path_to_host/raw \
-H "Content-Type: application/json" \
-d '{"rc_id":1,
     "event_id":2, 
     "start_datetime":"2017-09-27 17:53:27.350000", 
     "end_datetime": "2017-09-27 17:53:27.400000"}'

# using httpie
http post http://path_to_host/raw \
  rc_id:=1 \
  event_id:=2 \
  start_datetime='2017-09-27 17:53:27.350000' \
  end_datetime='2017-09-27 17:59:27.400000'
```

```python
import requests
json_data = {'rc_id':1,
             'event_id':2, 
             'start_datetime': '2017-09-27 17:53:27.350000',
             'end_datetime': '2017-09-27 17:53:27.400000'}
response = requests.post('http://path_to_host/api/raw/rid', json=json_data)
```

> The above command returns JSON structured like this:

```json
[
    {
        "acx": 105,
        "acy": 103,
        "acz": 98,
        "event_id": 2,
        "grx": 95,
        "gry": 101,
        "grz": 92,
        "rid": 181,
        "sid": 1,
        "ts_ms": "2017-09-27T17:53:27.381438+00:00"
    },
    {
        "acx": 61,
        "acy": 49,
        "acz": 48,
        "event_id": 2,
        "grx": 29,
        "gry": 12,
        "grz": 13,
        "rid": 182,
        "sid": 2,
        "ts_ms": "2017-09-27T17:53:27.381438+00:00"
    },
    {
        "acx": -85,
        "acy": -72,
        "acz": -79,
        "event_id": 2,
        "grx": -77,
        "gry": -61,
        "grz": -49,
        "rid": 183,
        "sid": 3,
        "ts_ms": "2017-09-27T17:53:27.381438+00:00"
    }
]
```

This endpoint retrieves raws filtered by {rc_id, event_id, start_datetime, end_datetime}

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`POST http://path_to_host/raw`

### JSON Parameters

Parameter | Description
--------- | -----------
rc_id | The primary key of the recording_config to retrieve
event_id | The primary key of the event to retrieve
start_datetime | The start datetime of the raws
end_datetime | The end datetime of the raws


<!-- =============================================================== -->
## Delete a Specific Raw

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific raw.

### HTTP Request

`DELETE http://path_to_host/raw/<rid>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the raw to delete




# RecordingConfig

<!-- =============================================================== -->
## List All RecordingConfigs

```shell
# using curl
curl http://path_to_host/api/recording_config

# using httpie
http get http://path_to_host/api/recording_config
```

```python
import requests
response = requests.get('http://path_to_host/api/recording_config')
```

> The above command returns JSON structured like this:

```json
{
  "num_results": 2,
  "objects": [
    {
      "description": "test1",
      "events": [
        {
          "end_time": null,
          "event_id": 1,
          "rc_id": 1,
          "remark": "sent by simulator",
          "start_time": null
        }
      ],
      "name": "config: 3 sensors",
      "rc_id": 1,
      "sensors": [
        {
          "img_ref": null,
          "loc_orient": "loc1",
          "sid": 1
        },
        {
          "img_ref": null,
          "loc_orient": "loc2",
          "sid": 2
        },
        {
          "img_ref": null,
          "loc_orient": "loc3",
          "sid": 3
        }
      ]
    },
    {
      "description": "test2",
      "events": [],
      "name": "config: 2 sensors",
      "rc_id": 2,
      "sensors": [
        {
          "img_ref": null,
          "loc_orient": "loc3",
          "sid": 3
        },
        {
          "img_ref": null,
          "loc_orient": "loc5",
          "sid": 5
        }
      ]
    }
  ],
  "page": 1,
  "total_pages": 1
}
```

This endpoint lists all recording_config records.

### HTTP Request
`GET http://path_to_host/api/recording_config`

### HTTP Response
`HTTP/1.0 200 OK`



<!-- =============================================================== -->
## Insert a Recording Config Record with Sensors

```shell
# using curl
curl http://path_to_host/api/recording_config \
-H "Content-Type: application/json" \
-d '{"name": "rec_name",
     "description": "..."}'

# using httpie
http post http://path_to_host/api/recording_config \
          name='rec_name' \
          description='...' \
          sensors:='[{"sid": sid1}, {"sid": sid2}]'
```

```python
import requests
response = requests.post('http://path_to_host/api/recording_config', 
                         json={'name': 'rec_name', 
                               'description': '...', 
                               'sensors': [{'sid': sid1}, {'sid': sid2}]})
```

> The above command returns JSON structured like this:

```json
{
  "description": "test3",
  "events": [],
  "name": "config test1",
  "rc_id": 3,
  "sensors": [
    {
      "img_ref": null,
      "loc_orient": "loc1",
      "sid": 1
    },
    {
      "img_ref": null,
      "loc_orient": "loc2",
      "sid": 2
    }
  ]
}
```

This endpoint inserts a new recording_config record with sensors

### HTTP Request
`POST http://path_to_host/api/recording_config`

### JSON Parameters

Parameter | Description
--------- | -----------
rc_id | The primary key of the recording_config to insert (can be omitted)
name | The name of the recording_config
description | The description of the recording_config
sensors | Array of sensors

### HTTP Response
`HTTP/1.0 201 CREATED`




<!-- =============================================================== -->
## Update a Existed Recording Config Record

```shell
# using curl
curl http://path_to_host/api/recording_config/rc_id \
-H "Content-Type: application/json" \
-d '{"name": modified_name,
     "description": modified_description}'

# using httpie
http post http://path_to_host/api/recording_config/rc_id \
          name=modified_name \
          description=modified_description
```

```python
import requests
response = requests.post('http://path_to_host/api/recording_config/rc_id', 
                    json={'name': modified_name, 'description': modified_description)
```

> The above command returns JSON structured like this:

```json
{
  "description": modified_description,
  "events": [],
  "name": modified_name,
  "rc_id": rc_id_new,
  "sensors": []
}
```

This endpoint updates a existed recording_config record.

### HTTP Request
`POST http://path_to_host/api/recording_config/rc_id`

### JSON Parameters
Parameter | Description
--------- | -----------
name | The name of the recording_config
description | The description of the recording_config


<!-- =============================================================== -->
## Delete a Recording Config Record

```shell
# using curl
curl -X DELETE http://path_to_host/api/recording_config/3

# using httpie
http delete http://path_to_host/api/recording_config/3
```

```python
import requests
response = requests.delete('http://path_to_host/api/recording_config/3')
```

> The above command returns JSON structured like this:

```json
{}
```

This endpoint deletes a recording_config record.

### HTTP Request
`DELETE http://path_to_host/api/recording_config/3`

### URL Parameters

Parameter | Description
--------- | -----------
rc_id | The rc_id of the recording_config to delete


### HTTP Response
`HTTP/1.0 204 NO CONTENT`





# Sensor

<!-- =============================================================== -->
## List All Sensors

```shell
# using curl
curl http://path_to_host/api/sensor

# using httpie
http get http://path_to_host/api/sensor
```

```python
import requests
response = requests.get('http://path_to_host/api/sensor')
```

> The above command returns JSON structured like this:

```json
{
  "num_results": 3,
  "objects": [
    {
      "img_ref": null,
      "loc_orient": "loc1",
      "sid": 1
    },
    {
      "img_ref": null,
      "loc_orient": "loc2",
      "sid": 2
    },
    {
      "img_ref": null,
      "loc_orient": "loc3",
      "sid": 3
    }
  ],
  "page": 1,
  "total_pages": 1
}
```

This endpoint lists all sensor records.

### HTTP Request
`GET http://path_to_host/api/sensor`

### HTTP Response
`HTTP/1.0 200 OK`





<!-- =============================================================== -->
## Insert a Sensor Record

```shell
# using curl
curl http://path_to_host/api/sensor \
-H "Content-Type: application/json" \
-d '{"loc_orient": some_description,
     "img_ref": img_url_to_use}'

# using httpie
http post http://path_to_host/api/sensor \
          loc_orient=some_description \
          img_ref=img_url_to_use
```

```python
import requests
response = requests.post('http://path_to_host/api/sensor', 
                    json={'loc_orient': 'some_description', 'img_ref': img_url_to_use)
```

> The above command returns JSON structured like this:

```json
{
  "img_ref": null,
  "loc_orient": description,
  "sid": sid_new
}
```

This endpoint inserts a new sensor record.

### HTTP Request
`POST http://path_to_host/api/sensor`

### JSON Parameters

Parameter | Description
--------- | -----------
loc_orient | The description of the sensor
img_ref | The image url location

### HTTP Response
`HTTP/1.0 201 CREATED`



<!-- =============================================================== -->
## Delete a Specific Sensor

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific raw.

### HTTP Request

`DELETE http://path_to_host/sensor/<sid>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the sensor to delete



<!-- =============================================================== -->
## Update a Sensor Record

```shell
# using curl
curl http://path_to_host/api/sensor \
-H "Content-Type: application/json" \
-d '{"loc_orient": modified_description,
     "img_ref": modified_img_ref}'

# using httpie
http post http://path_to_host/api/sensor/sid \
          loc_orient=modified_description \
          img_ref=modified_imf_ref
```

```python
import requests
response = requests.post('http://path_to_host/api/sensor/sid', 
                    json={'loc_orient': modified_description, 'img_ref': modified_img_ref)
```

> The above command returns JSON structured like this:

```json
{
  "img_ref": modified_img_ref,
  "loc_orient": modified_description,
  "sid": sid_to_be_modified
}
```

This endpoint updates a existed sensor record.

### HTTP Request
`POST http://path_to_host/api/sensor/sid`

### JSON Parameters

Parameter | Description
--------- | -----------
loc_orient | The modified description of the sensor
img_ref | The modified image url location

### HTTP Response
`HTTP/1.0 201 CREATED`



# Simulator API

<!-- =============================================================== -->
## Start Simulator: Begin to Send Data

```shell
# using httpie
http post http://path_to_simulator_host/simu/start_event period:=1000 rc_id:=1
```

### HTTP Request
`POST http://path_to_simulator_host/simu/start_event`

### JSON Parameters

Parameter | Description
--------- | -----------
rc_id | The rc_id of the recording_config to delete
period | The update period of the simulated data (milli-second)


<!-- ### HTTP Response -->
<!-- `HTTP/1.0 204 NO CONTENT` -->
