---
title: API Reference AloHa

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

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

# Authentication

There's no authentication in this project.

# Raws

## Get a Specific Raw

```python
import requests

response = requests.get('http://path_to_host/api/raw/rid')
```

```shell
*using curl*
curl http://path_to_host/api/raw/rid

*using httpie*
http get http://path_to_host/api/raw/rid
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
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

<!-- ### Query Parameters -->

<!-- Parameter | Default | Description -->
<!-- --------- | ------- | ----------- -->
<!-- include_cats | false | If set to true, the result will also include cats. -->
<!-- available | true | If set to false, the result will include kittens that have already been adopted. -->

<!-- <aside class="success"> -->
<!-- Remember — a happy kitten is an authenticated kitten! -->
<!-- </aside> -->

## Get Filtered Raws

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

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint retrieves a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

