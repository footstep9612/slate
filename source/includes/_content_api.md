# 静态接口（Content API）

分销商可用本章涵盖的接口来获取酒店的静态信息。例如酒店名字，地址，描述等等。

## 国家信息（Country list）

本接口用于获取国家信息。包括国家名称，id，国际区号，名字，及国家代码。

### Reuqest

`GET https://{endpoint}/content/public/..`

### Response

> Country List Response

```json
{
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"..": [
		{
			"country_id": "1",
			"country_code": "AD",
			"phonecode": "+376",
			"name": "Andorra",
			"name_zh": "安道尔",
			"continent": "Europe",
			"continent_zh": "欧洲"
		},
		{
			"country_id": "2",
			"country_code": "AE",
			"phonecode": "+971",
			"name": "United Arab Emirates",
			"name_zh": "阿拉伯联合酋长国",
			"continent": "Asia",
			"continent_zh": "亚洲"
		}
	]
}
```

| Parameter                 | Count | Type   | Description              |
| ------------------------- | ----- | ------ | ------------------------ |
| country_list              | 1     | Array  | 国家返回所有信息         |
| country_list.country_id   | 1     | String | Amerilink 唯一国家ID     |
| country_list.country_code | 1     | String | ISO Alpha-2 标准国家代码 |
| country_list.phonecode    | 1     | String | 国家电话国际区号         |
| country_list.name         | 1     | String | 国家英文名字             |
| country_list.name_zh      | 1     | String | 国家中文名字             |
| country_list.continent    | 1     | String | 大洲                     |
| country_list.continent_zh | 1     | String | 大洲                     |

## 城市信息（City List）
利用从`Country List` 接口中拿到的数据来获取城市信息。

### Request
`GET https://{Endpoint}/content/public/city_list./{country_id}`

### Response

> City List Response

```json
{
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"city_list": [
		{
			"city_id": "264",
			"name": "Allentown",
			"areaname": "Pennsylvania",
			"center_latitude": "40.602180",
			"center_longitude": "-75.471695"
		},
		{
			"city_id": "271",
			"name": "Albuquerque",
			"areaname": "New Mexico",
			"center_latitude": "35.083679",
			"center_longitude": "-106.644653"
		}
	]
}
```


| Parameter                  | Count | Type   | Description           |
| -------------------------- | ----- | ------ | --------------------- |
| city_list                  | 1     | Array  | 城市返回所有信息      |
| city_list.city_id          | 1     | String | Amerilink 唯一城市 Id |
| city_list.name             | 1     | String | 对应的城市名称        |
| city_list.areaname         | 1     | String | 城市所属的省/州名称   |
| city_list.center_latitude  | 1     | String | 城市中心经度          |
| city_list.center_longitude | 1     | String | 城市中心维度          |
<aside class="notice"><code>Latitude</code> 和 <code>Longitude</code> 是基于Google map的数据 </aside>
## 酒店信息Hotel List

利用从 `City List`接口中拿到的数据来获取城市信息。

<aside class="notice">可以用<code>locale</code>参数，以及值： <code>zh-CN</code>， <code>en_US</code> 来获取中文以及英文静态信息</aside>
### Request

`GET https:{EndPoint}/content/public/hotelInfo_list/{city_id}`

### Response

> Hotel List Response

```json
"result": {
  "return_status": {
    "success": "true",
    "exception": ""
  }
},
"hotelinfo_list": [{
    "hotel_id": 116228,
    "star": 4,
    "pic": "https://img.Amerilink hotels-content.com/public/hotels/1184/116228/supplier/4.jpg",
    "address": " 101 Harborside Drive Boston Massachusetts 02128",
    "currency": "USD",
    "country_short": "US",
    "country_id": 233,
    "city": "Boston",
    "city_id": "660",
    "state_name": "Massachusetts",
    "postcode": "02128",
    "phone": "+16175681234",
    "fax": "+16175686080",
    "latitude": "42.35910006493",
    "longitude": "-71.027150920238"
  },
  {
    "hotel_id": 58908,
    "star": 4,
    "pic": "https://img.Amerilink hotels-content.com/public/hotels/1003/58908/supplier/0.jpg",
    "address": " 425 Summer Street Boston Massachusetts 02210",
    "currency": "USD",
    "country_short": "USA",
    "country_id": 233,
    "city": "Boston",
    "city_id": "660",
    "state_name": "Massachusetts",
    "postcode": "02210",
    "phone": "+16175324600",
    "fax": "+16175324630",
    "latitude": "42.346190881589",
    "longitude": "-71.043058633804"
  }
]
}
```
| Parameter            | Count | Type   | Description                                   |
| -------------------- | ----- | ------ | --------------------------------------------- |
| hotelinfo_list    | 1     | Array  | 返回的所有酒店信息 |
| hotelinfo_list.hotel_id | 1     | Int    | Amerilink唯一酒店ID   |
| hotelinfo_list.star | 1     | Int    | 酒店星级                                   |
| hotelinfo_list.pic | 1     | String | 酒店主图片                                   |
| hotelinfo_list.address | 1     | String | 酒店地址                                       |
| hotelinfo_list.currency | 1     | String | 货币，暂时只接受USD          |
| hotelinfo_list.country_short | 1     | String | 国家代码缩写 |
| hotelinfo_list.country_id | 1     | String | Amerilink 唯一国家ID |
| hotelinfo_list.city | 1     | String | 酒店所在的城市名                     |
| hotelinfo_list.city_id | 1     | String | Amerilink唯一城市ID                              |
| hotelinfo_list.state_name | 1     | String | 省/州名                               |
| hotelinfo_list.postcode | 1     | String | 酒店邮编                              |
| hotelinfo_list.phone | 1     | String | 酒店电话                                |
| hotelinfo_list.fax | 1     | String | 酒店传真                                    |
| hotelinfo_list.latitude | 1     | String | 酒店径度                                      |
| hotelinfo_list.longitude | 1     | String | 酒店维度                                     |

## 多酒店静态信息（Multiple Hotels）

利用 `Hotel List` 接口中拿到的酒店ID或者经纬度来批量获取酒店静态信息

<aside class="notice">可以用<code>locale</code>参数，以及值： <code>zh-CN</code>， <code>en_US</code> 来获取中文以及英文静态信息</aside>
### Request

> Multi Hotel Request

```json
{
	"hotel_ids" : []
	"latitude" : "34.14002540",
	"longitude" : "-118.01525850",
	"radius" : 20,
}
```

`POST https://{Endpoint}/content/public/multi_hotels/`

| Parameter | Count | Type   | Description        |
| --------- | ----- | ------ | ------------------ |
| hotel_ids | 1     | Array  | 将被搜索的酒店列表 |
| latitude  | 1     | String | 搜索范围中心点经度 |
| longitude | 1     | String | 搜索范围中心点纬度 |
| radius    | 1     | Float  | 搜索范围半径       |
### Response

> Multi Hotel Response

```json
{
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"hotels": [
		{
			"hotel_id": 113126,
			"name": "Embassy Suites Arcadia Pasadena",
			"star": 3.5,
			"pic": "https://s1.imgs.美联国际 hotels.net.cn/apiadm/images/2019/03/01/bfe536ae4b6e18375114d2f85e33b469.jpg",
			"intro": "<p><b>Facilities</b><br/>Travellers can enjoy a pleasant stay in one of the 191 rooms. Services and facilities at the hotel include a conference room and a business centre. Those arriving in their own vehicles can leave them in the car park of the hotel.</p><p><b>Rooms</b><br/>Each of the rooms is appointed with air conditioning and a kitchen. A balcony is included as standard in most rooms, offering additional space for relaxation. All rooms feature a fridge and a tea/coffee station. The establishment offers family rooms and non-smoking rooms.</p><p><b>Sports/Entertainment</b><br/>The accommodation offers attractions including sport and entertainment opportunities. A pool and an indoor pool are available to guests. The hotel offers a gym (for a fee) to travellers.  </p><p><b>Meals</b><br/>Bed and breakfast is bookable.</p>",
			"address": " 211 East Huntington Drive Arcadia 91006 California",
			"currency": "USD",
			"country_short": "US",
			"country_name": "US",
			"country_id": 233,
			"city": "Arcadia",
			"city_id": "7844",
			"state_name": "California",
			"metro": "0",
			"postcode": "91006",
			"phone": "626-445-8525",
			"fax": "626-445-8548",
			"latitude": "34.141069",
			"longitude": "-118.024451",
			"distance": 0.8539253642658865,
			"flag": 2,
			"is_bfc_free": 1
		},
		{
			"hotel_id": 62590,
			"name": "SpringHill Suites Pasadena / Arcadia",
			"star": 3,
			"pic": "https://s8.imgs.美联国际 hotels.net.cn/public/hotels/1001/62590/supplier/1.jpg",
			"intro": "<p><b>Facilities</b><br/>Guest accommodation comprises 86 rooms. Services and facilities at the hotel include a conference room and a business centre. Wireless internet access is available to guests. Those arriving in their own vehicles can leave them in the car park of the hotel.</p><p><b>Rooms</b><br/>The accommodation features rooms with air conditioning. All rooms offer a fridge, a tea/coffee station and WiFi. The establishment offers family rooms and non-smoking rooms.</p><p><b>Sports/Entertainment</b><br/>The hotel offers attractions including sport and entertainment opportunities. A pool and an outdoor pool are available to travellers. The accommodation offers a gym (for a fee) to guests.  </p><p><b>Meals</b><br/>Bed and breakfast is bookable.</p>",
			"address": " 99 N 2nd Avenue Arcadia 91006 California",
			"currency": "USD",
			"country_short": "US",
			"country_name": "US",
			"country_id": 233,
			"city": "Arcadia",
			"city_id": "7844",
			"state_name": "California",
			"metro": "0",
			"postcode": "91006",
			"phone": "+16268215400",
			"fax": null,
			"latitude": "34.141118",
			"longitude": "-118.025673",
			"distance": 0.9661358749901667,
			"flag": 2,
			"is_bfc_free": 1
		}
	]
}{
  "result": {
    "return_status": {
      "success": "true",
      "exception": ""
    }
  },
  "hotels": [{
    "hotel_id": 113126,
    "name": "Embassy Suites Arcadia Pasadena",
    "star": 3.5,
    "pic": "https:\/\/s1.imgs.Amerilink hotels.net.cn\/apiadm\/images\/2019\/03\/01\/bfe536ae4b6e18375114d2f85e33b469.jpg",
    "intro": "<p><b>Facilities<\/b><br\/>Travellers can enjoy a pleasant stay in one of the 191 rooms. Services and facilities at the hotel include a conference room and a business centre. Those arriving in their own vehicles can leave them in the car park of the hotel.<\/p><p><b>Rooms<\/b><br\/>Each of the rooms is appointed with air conditioning and a kitchen. A balcony is included as standard in most rooms, offering additional space for relaxation. All rooms feature a fridge and a tea\/coffee station. The establishment offers family rooms and non-smoking rooms.<\/p><p><b>Sports\/Entertainment<\/b><br\/>The accommodation offers attractions including sport and entertainment opportunities. A pool and an indoor pool are available to guests. The hotel offers a gym (for a fee) to travellers.  <\/p><p><b>Meals<\/b><br\/>Bed and breakfast is bookable.<\/p>",
    "address": " 211 East Huntington Drive Arcadia 91006 California",
    "currency": "USD",
    "country_short": "US",
    "country_name": "US",
    "country_id": 233,
    "city": "Arcadia",
    "city_id": "7844",
    "state_name": "California",
    "metro": "0",
    "postcode": "91006",
    "phone": "626-445-8525",
    "fax": "626-445-8548",
    "latitude": "34.141069",
    "longitude": "-118.024451",
    "distance": 0.8539253642658865,
    "flag": 2,
    "is_bfc_free": 1
  }, {
    "hotel_id": 62590,
    "name": "SpringHill Suites Pasadena \/ Arcadia",
    "star": 3,
    "pic": "https:\/\/s8.imgs.Amerilink hotels.net.cn\/public\/hotels\/1001\/62590\/supplier\/1.jpg",
    "intro": "<p><b>Facilities<\/b><br\/>Guest accommodation comprises 86 rooms. Services and facilities at the hotel include a conference room and a business centre. Wireless internet access is available to guests. Those arriving in their own vehicles can leave them in the car park of the hotel.<\/p><p><b>Rooms<\/b><br\/>The accommodation features rooms with air conditioning. All rooms offer a fridge, a tea\/coffee station and WiFi. The establishment offers family rooms and non-smoking rooms.<\/p><p><b>Sports\/Entertainment<\/b><br\/>The hotel offers attractions including sport and entertainment opportunities. A pool and an outdoor pool are available to travellers. The accommodation offers a gym (for a fee) to guests.  <\/p><p><b>Meals<\/b><br\/>Bed and breakfast is bookable.<\/p>",
    "address": " 99 N 2nd Avenue Arcadia 91006 California",
    "currency": "USD",
    "country_short": "US",
    "country_name": "US",
    "country_id": 233,
    "city": "Arcadia",
    "city_id": "7844",
    "state_name": "California",
    "metro": "0",
    "postcode": "91006",
    "phone": "+16268215400",
    "fax": null,
    "latitude": "34.141118",
    "longitude": "-118.025673",
    "distance": 0.9661358749901667,
    "flag": 2,
    "is_bfc_free": 1
  }]
}
```

| Parameter | Count | Type  | Description                             |
| --------- | ----- | ----- | --------------------------------------- |
| hotels    | 1     | Array | 包含所有酒店的数组 |
| city_list.hotel_id | 1 | String | 美联国际唯一酒店ID |
| city_list.name | 1     | String | 酒店名             |
| city_list.star | 1     | Float | 酒店星级        |
| city_list.pic | 1     | String| 酒店主图片 |
| city_list.intro | 1     | String| 酒店简介 |
| city_list.address | 1     | String| 酒店地址 |
| city_list.currency | 1     | String | Currency will always be USD at the moment                   |
| city_list.country_short | 1     | String | 国家缩写 |
| city_list.country_name | 1     | String | Country Name|
| city_list.country_id | 1     | String | Amerilink唯一国家ID                       |
| city_list.city | 1     | String | 城市名称 |
| city_list.city_id | 1     | String | Amerilink唯一城市名称 |
| city_list.state_name | 1     | String | 省/周 名称                              |
|city_list.distance| 1| Float| 据搜索中心的举例 |
| city_list.metro | 1     | String | 预留字段 |
| city_list.postcode | 1| String | 酒店邮编 |
| city_list.phone | 1| String | 酒店电话 |
| city_list.fax    | 1     | String | 酒店传真                                    |
| city_list.latitude | 1     | String | 酒店经度                                      |
| city_list.longitude | 1     | String | 酒店纬度                                 |
| city_list.otherimages | 1| Array| 酒店的其他图片 |



## 单酒店静态信息（Single Hotel ）

利用从 `Hotel List` 接口中获取的信息来获取单酒店静态信息

<aside class="notice">可以用<code>locale</code>参数，以及值： <code>zh-CN</code>， <code>en_US</code> 来获取中文以及英文静态信息</aside>
### Request

`GET https://{Endpoint}/content/public/single_hotel/{hotel_id}?locale={language_code}`

### Response

> Hotel List Response

```json
 {
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"hotel_id": 116729,
	"hotel_data": {
		"name": "总部广场莫里斯敦凯悦酒店",
		"name_en": "Hyatt Morristown at Headquarters Plaza",
		"star": 4,
		"pic": "https://s2.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/0.jpg",
		"intro": "<p><strong>酒店设施<br/>酒店诚挚欢迎客人入住在一共有256间的客房之中。酒店提供保险柜。通过无线网络客人可在公共区域便利上网。餐饮设施有：餐厅、用餐区、咖啡厅和酒吧。旅游纪念品可在纪念品商店选购。此外尚有其他便利设施，比如报刊店。驾车前来的客人，可将车辆停放于酒店的停车场。其他贴心服务还包含接送服务、翻译服务和送餐服务。</strong></p><p><strong><strong>客房设施<br/>房间里备有空调和浴室。房间备有一张双人床和一张沙发床。此外亦提供保险柜等设备。冰箱和煮茶/咖啡机让房客在住宿期间倍感舒适方便。互联网接口、电话、电视机和无线网络使房间设备更加完善。浴缸等设施为浴室更添舒适。吹风机和浴袍能满足日常所需。酒店提供家庭房和禁烟房。</strong></strong></p><p><strong><strong><strong>运动/休闲<br/>酒店拥有游泳池和室内游泳池。需要健身的旅客有多种休闲项目可选择，比如健身中心、SPA、桑拿和按摩理疗。</strong></strong></strong></p><p><strong><strong><strong><strong>餐饮<br/>特别提供早餐、午餐和晚餐。</strong></strong></strong></strong></p><p></p><p></p><p></p>",
		"intro_en": "<p><strong>Facilities<br/>Guests are welcomed at the hotel, which has a total of 256 rooms. Amenities include a safe. Wireless internet access is available to travellers in the public areas. Among the culinary options available at the accommodation are a restaurant, a dining area, a caf&amp;eacute; and a bar. Guests can buy souvenirs at the gift shop. Additional facilities at the establishment include a newspaper stand. Those arriving in their own vehicles can leave them in the car park of the hotel. Available services and facilities include a transfer service, translation services and room service.</strong></p><p><strong><strong>Rooms<br/>Each of the rooms is appointed with air conditioning and a bathroom. The rooms have a double bed and a sofa bed. There is also a safe. A fridge and a tea/coffee station ensure a comfortable stay. Other features include internet access, a telephone, a TV and WiFi. The bathroom offers convenient facilities including a bathtub. A hairdryer and bathrobes are provided for everyday use. The accommodation offers family rooms and non-smoking rooms.</strong></strong></p><p><strong><strong><strong>Sports/Entertainment<br/>The establishment features a pool and an indoor pool. Active travellers can choose from a range of leisure activities, including a gym, a spa, a sauna and massage treatments.</strong></strong></strong></p><p><strong><strong><strong><strong>Meals<br/>The accommodation offers breakfast, lunch and dinner.</strong></strong></strong></strong></p><p></p><p></p><p></p>",
		"address": "3 Speedwell Avenue",
		"address_en": "3 Speedwell Avenue",
		"currency": "USD",
		"country_short": "US",
		"country_name": "美国",
		"country_name_en": "United States",
		"country_id": 233,
		"city": "莫瑞斯镇",
		"city_en": "Morristown",
		"city_id": "8771",
		"state_name": "New Jersey",
		"metro": "0",
		"postcode": "07960",
		"phone": "+19736471234",
		"fax": "+19732927562",
		"latitude": "40.80005",
		"longitude": "-74.481124",
		"flag": 2,
		"is_bfc_free": -1
	},
	"otherimages": [
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/0.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/12.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/15.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/18.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/21.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/24.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/27.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/3.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/30.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/33.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/6.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/9.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/0.jpg ",
		"https://s5.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/0.jpg ",
		"https://s2.imgs.aichotels.net.cn/public/hotels/1184/116729/supplier/0.jpg"
	]
}
```

| Parameter      | Count | Type   | Description                    |
| -------------- | ----- | ------ | ------------------------------ |
| city_list.hotel_id | 1 | Int | 唯一Amerilink酒店ID |
| city_list.hotel_data |1|Object| |
| city_list.name | 1     | String | 酒店名             |
| city_list.star | 1     | Int | 酒店星级        |
| city_list.pic | 1     | String| 酒店主图片 |
| city_list.intro | 1     | String| 酒店简介 |
| city_list.address | 1     | String| 酒店地址 |
| city_list.currency | 1     | String | Currency will always be USD at the moment                   |
| city_list.country_short | 1     | String | 国家缩写 |
| city_list.country_name | 1     | String | Country Name|
| city_list.country_id | 1     | String | Amerilink唯一国家ID                       |
| city_list.city | 1     | String | 城市名称 |
| city_list.city_id | 1     | String | Amerilink唯一城市名称 |
| city_list.state_name | 1     | String | 省/周 名称                              |
|city_list.distance| 1| Float| 据搜索中心的举例 |
| city_list.metro | 1     | String | 预留字段 |
| city_list.postcode | 1| String | 酒店邮编 |
| city_list.phone | 1| String | 酒店电话 |
| city_list.fax    | 1     | String | 酒店传真                                    |
| city_list.latitude | 1     | String | 酒店经度                                      |
| city_list.longitude | 1     | String | 酒店纬度                                 |
| city_list.otherimages | 1| Array| 酒店的其他图片 |

## 房型信息（Hotel Rooms）

利用从 `Hotel List` 接口中获取的信息来查询房型信息
<aside class="notice">可以用<code>locale</code>参数，以及值： <code>zh-CN</code>， <code>en_US</code> 来获取中文以及英文静态信息</aside>
### Request

`GET https://{Endpoint}/content/public/hotel_rooms/{hotel_id}?locale={language_code}`  

### Response

> Hotel Rooms Response

```json
{
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"room_list": [
		{
			"room_type": "1013",
			"room_name": "Andaz King",
			"room_name_zh": "安达仕房（特大床）",
			"room_desc": null,
			"room_size": null,
			"bed_info": null,
			"room_pics": [
				null
			],
			"nonsmoking": null,
			"amenities": null,
			"max_occupancy": null
		},
		{
			"room_type": "3604",
			"room_name": "Andaz Large Loft",
			"room_name_zh": "安达仕大型复式房",
			"room_desc": null,
			"room_size": null,
			"bed_info": null,
			"room_pics": [
				null
			],
			"nonsmoking": null,
			"amenities": null,
			"max_occupancy": null
		}	
	]
}
```

| Parameter | Count | Type  | Description                   |
| --------- | ----- | ----- | ----------------------------- |
| room_list | 1     | Array | 返回的所有房型列表 |
| room_list.room_type | 1     | String | 该字符串唯一确定特定酒店的一个房型 |
| room_list.room_name | 1     | String | 房型名称 |
| room_list.room_desc | 1     | String | 房型描述 |
| room_list.room_size | 1     | String | 房间面积(Eg(1)size1-size2 最小面积:size1,最大面积:size2;(2)只有一种大小的时候这个字段返回数字) |
| room_list.bed_info | 1     | Array | 床型及数量。当返回的数组中存在多个对象时，如[ { "S": 1, "D":1 },{ "D":2 } ]，表明属于该房型的一部分房间的床型为{ "S": 1, "D":1 }（1个”S”,1 个”D”）,另一部分房间的床型为{ "D":2 }(2 个”D”)默认值为 null, 表明床型信息不明。 |
| room_list.room_pics | 1     | array | 房型图片列表，默认值：空数组 |
| room_list.smoke | 1     | Int | 该房型是否禁烟 1: 禁烟；0: 不禁烟 ,2: 属于该房型的一部分房间禁烟，另一部分不禁烟 |
| room_list.amenities | 1     | Object | 房间设施 结构为: {“设施 1_ID”:value1,”设施 2_ID”:value2….}值：1: 该房型设有该设施,2: 该房型设有该设施，需付费. |
| room_list.max_occupancy | 1     | Object | 最大入住人数信息                                             |

