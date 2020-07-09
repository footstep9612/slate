# 动态价格接口

本章描述了动态价格接口的参数以及功能。动态价格类的接口会涉及到具体业务的整个流程，具体见本章详细内容。

## 多酒店搜索(Multi-Hotel Search)

多酒店搜索接口利用 Amerilink 唯一 `Hotel_ids` ，来批量拉取酒店价格。

<aside class="notice"> 现阶段，<code>Multi-hotel</code>接口只支持<code>多酒店单天</code>搜所，如果在搜索条件中搜索多天则会返回checkou日期当日的价格。 </aside>
### 请求	

> Multi-Hotel Availability 请求

```json
{
	"hotelCodesList": [
		"50017"
	],
	"checkin": "2019-03-28",
	"checkout": "2019-03-29"
}
```

`POST https://{{Endpoint}}/rate/public/cache/multiple_availability`

| Parameter      | Count | Type   | Description    |
| -------------- | ----- | ------ | -------------- |
| hotelCodesList | 1     | Array  | 待搜索酒店列表 |
| checkin        | 1     | String | 入住日期  |
| checkout       | 1     | String | 退房日期       |

### 返回

> Multi-Hotel Availability 返回
>
> 下例展示了2家酒店中1家酒店有价格，另一家无房的样例

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
			"hotelId": 50017,
			"room_list": [
				{
					"room_name": "Deluxe Poolside Cabana, 1 King, Non-Smoking",
					"room_desc": "The Deluxe Poolside Cabana provides private patios with direct access to The LINQ's pool and waitress service from the pool bar. Electronic features include USB charging stations and a flat-screen TV. Guests can check-in via the VIP check-in desk.",
					"room_features": "",
					"room_type": "CCV1",
					"rates_and_cancellation_policies": [
						{
							"rate_plan_code": "DISCS",
							"meal_plan_code": "NOM",
							"rate_comments": "Excludes $39.70 daily resort fee",
							"total_amount_before_tax": "229.90",
							"total_amount_after_tax": "260.66",
							"currency": "USD",
							"rates": [
								{
									"check_in": "2019-10-28",
									"check_out": "2019-10-29",
									"rooms": "1",
									"amount_before_tax": {
										"night_rate": "229.90",
										"sub_total": "229.90"
									},
									"amount_after_tax": {
										"night_rate": "260.66",
										"sub_total": "260.66"
									}
								}
							],
							"cancellation_information": {
								"support_cancel": "yes",
								"non_refundable": "no",
								"details": [
									{
										"datetime": "2019-10-22T00:00:00",
										"fee_type_value": "260.66",
										"fee_type": "total_amount",
										"amount_penalty": "260.66",
										"policy_code": "CXP"
									},
									{
										"datetime": "2019-10-22T00:00:00",
										"fee_type_value": "1",
										"fee_type": "nights",
										"amount_penalty": "236.96",
										"policy_code": "CXP"
									},
									{
										"fee_type_value": "260.66",
										"fee_type": "total_amount",
										"amount_penalty": "260.66",
										"policy_code": "CNS"
									}
								],
								"timezone": "America/Los_Angeles UTC-08:00"
							},
							"room_key": "Q0NWMS4uKkRJU0NTLi4qNTAwMTc=",
							"nationality": "|",
							"breakfast": {
								"include": "0",
								"count": "0"
							},
							"meal_plan_desc": "“Room Only”, “It only includes the stay for all guests in selected room. No meals are included.”"
						}
					],
					"rate_supplier_id": 0
				},
				{
					"room_name": "King Suite, Non-Smoking",
					"room_desc": "The 600-square-foot King Suite features all of the space of a Deluxe King room, plus a separate living area. This suite offers one king bed with a pillow-top mattress. Electronic features include USB charging stations and two LED flat-screen TVs.",
					"room_features": "",
					"room_type": "CCT1",
					"rates_and_cancellation_policies": [
						{
							"rate_plan_code": "DISCS",
							"meal_plan_code": "NOM",
							"rate_comments": "Excludes $39.70 daily resort fee",
							"total_amount_before_tax": "124.34",
							"total_amount_after_tax": "140.98",
							"currency": "USD",
							"rates": [
								{
									"check_in": "2019-10-28",
									"check_out": "2019-10-29",
									"rooms": "1",
									"amount_before_tax": {
										"night_rate": "124.34",
										"sub_total": "124.34"
									},
									"amount_after_tax": {
										"night_rate": "140.98",
										"sub_total": "140.98"
									}
								}
							],
							"cancellation_information": {
								"support_cancel": "yes",
								"non_refundable": "no",
								"details": [
									{
										"datetime": "2019-10-22T00:00:00",
										"fee_type_value": "140.98",
										"fee_type": "total_amount",
										"amount_penalty": "140.98",
										"policy_code": "CXP"
									},
									{
										"datetime": "2019-10-22T00:00:00",
										"fee_type_value": "1",
										"fee_type": "nights",
										"amount_penalty": "128.16",
										"policy_code": "CXP"
									},
									{
										"fee_type_value": "140.98",
										"fee_type": "total_amount",
										"amount_penalty": "140.98",
										"policy_code": "CNS"
									}
								],
								"timezone": "America/Los_Angeles UTC-08:00"
							},
							"room_key": "Q0NUMS4uKkRJU0NTLi4qNTAwMTc=",
							"nationality": "|",
							"breakfast": {
								"include": "0",
								"count": "0"
							},
							"meal_plan_desc": "“Room Only”, “It only includes the stay for all guests in selected room. No meals are included.”"
						}
					],
					"rate_supplier_id": 0
				}
			]
		},
		{
			"hotelId": 116264
		}
	]
}
```

| Parameter                                                | Count | Type   | Description                                                  |
| -------------------------------------------------------- | ----- | ------ | ------------------------------------------------------------ |
| hotels                                                   | 1     | Array  | 多个酒店返回组成的队列，每个酒店以一个对象返回               |
| hotelId                                                  | 1     | Int    | 美联国际唯一酒店ID                                           |
| room_list                                                | 1     | Array  | 所有可入住的房型列表                                         |
| room_list.room_name                                      | 1     | String | 房型名称                                                     |
| room_list.room_desc                                      | 1     | String | 房型描述                                                     |
| room_list.room_features                                  | 1     | String | 房间内设施/特色                                              |
| room_list.room_type                                      | 1     | String | 房型码                                                       |
| room_list.rates_and_cancellation_policies                | 1     | Array  | 所有有效的价格与其对应的取消政策数组                         |
| rates_and_cancellation_policies.rate_plan_code           | 1     | String | 价格方案的唯一标识                                           |
| rates_and_cancellation_policies.meal_plan_code           | 1     | String | 餐食方案的唯一标识                                           |
| rates_and_cancellation_policies.total_amount_before_tax  | 1     | Float  | 基于输入条件，预定所需的费用（不含税）                       |
| rates_and_cancellation_policies.total_amount_ after _tax | 1     | Float  | 基于输入条件，预定所需的费用（含税）                         |
| rates_and_cancellation_policies.currency                 | 1     | String | 费用基于的货币币种                                           |
| rates_and_cancellation_policies.rates                    | 1     | Array  | 价格政策                                                     |
| rates.check_in                                           | 1     | String | 入住日期                                                     |
| rates.check_out                                          | 1     | String | 退房日期                                                     |
| rates.rooms                                              | 1     | String | 总房间数                                                     |
| rates.amount_before_tax                                  | 1     | Object | 税前价格列表                                                 |
| rates.amount_before_tax.night_rate                       | 1     | Float  | 基于入住时间,税前一间房一晚的花销                            |
| rates.amount_before_tax.sub_total                        | 1     | Float  | 基于入住时间，退房时间的总花销（不含税）                     |
| rates.amount_after_tax                                   | 1     | Object | 税后价格                                                     |
| rates.amount_after_tax.night_rate                        | 1     | Float  | 税后一间房一晚的花销                                         |
| rates.amount_after_tax.sub_total                         | 1     | Float  | 基于入住时间，退房时间的总花销（含税）                       |
| rates_and_cancellation_policies.cancellation_information | 1     | Object | 此价格方案的取消政策                                         |
| cancellation_information.support_cancel                  | 1     | String | ‘yes’ 表明可以被取消,<br />‘no’ 不可被取消。                 |
| cancellation_information.non_refundable                  | 1     | String | ‘yes’ 表明取消不返款, ‘ no’ 表明取消返全款.‘partial’表明取消返还部分付款 |
| cancellation_information.details                         | 1     | Array  | 具体取消政策，为空时对接方需要调用下单前验证来获取取消政策.  |
| details.datetime                                         | 1     | 0...1  | 此条取消政策的生效时间                                       |
| details.fee_type_value                                   | 1     | String | 针对具体扣费类型的花费                                       |
| details.fee_type                                         | 1     | String | 具体扣费类型                                                 |
| details.amount_penalty                                   | 1     | String | 取消所需的花销                                               |
| details.policy_code                                      | 1     | Object | CXP:一般取消政策；    CKP:过早退房；<br />CFC:更改行程；<br />CNS:未出现； |
| cancellation_information.timezone                        | 1     | String | 取消政策依据的时区                                           |
| rates_and_cancellation_policies.room_key                 | 1     | String | 用于识别具体的房型和对应的价格政策                           |
| rates_and_cancellation_policies.nationality              | 1     | String | 国籍(默认为CN)                                               |
| rates_and_cancellation_policies.breakfast                | 1     | String | 早餐信息                                                     |
| breakfast.include                                        | 1     | Enum   | 0:不含早餐;<br />1:含早餐,<br />-1:不确定是否含早餐          |
| breakfast.count                                          | 1     | Int    | 包含的早餐数量                                               |
| cancellation_information.meal_plan_desc                  | 1     | String | 对于餐食计划的描述T                                           |

## 最低价接口（Multi-Hotel by Geo location）

最低价接口可以根据`Hotel_id`或者基于google map 的经纬度来搜索多酒店的最低价

### 请求

> Geo-Location Price Search 请求

```json
{
	"hotel_ids": [],
	"latitude": "34.14002540",
	"longitude": "-118.01525850",
	"radius": 5,
	"check_in": "2019-10-15",
	"check_out": "2019-10-15",
	"room_number": 2,
	"adult_number": 1,
	"kids_number": 1,
	"light": 0
}
```

`POST https://{{Endpoint}}/rate/public/search/hotels`

| Parameter    | Count | Type   | Description                                                  |
| ------------ | ----- | ------ | ------------------------------------------------------------ |
| hotel_ids    | 1     | Array  | 需搜索的酒店id列表，不为空的时候，将基于此字段搜索，忽略地理信息 |
| latitude     | 1     | String | 搜索范围中心点经度                                           |
| longitude    | 1     | String | 搜索范围中心点纬度                                           |
| radius       | 1     | Float  | 搜索范围半径                                                 |
| check_in     | 1     | String | 入住日期                                                     |
| check_out    | 1     | String | 退房日期                                                     |
| room_number  | 1     | Int    | 每个房间入住的成人数                                         |
| adult_number | 1     | Int    | 每个房间入住的小孩数                                         |
| kids_number  | 1     | Int    | 儿童数                                                       |
| light        | 1     | Int    | 有效值: 0 或 1,0 返回单酒店最低价和酒店基本信息(英语);1 只返回酒店最低价 |

### 返回

> Geo-Location Price Search 返回

```json
{
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"key_word": {
		"/apitester/send": null,
		"hotel_ids": [
			50126,
			50758
		],
		"latitude": "34.14002540",
		"longitude": "-118.01525850",
		"radius": "20",
		"check_in": "2019-12-20",
		"check_out": "2019-12-21",
		"room_number": "1",
		"adult_number": "2",
		"kids_number": "0",
		"light": "0",
		"/v3/public/search/hotels": null
	},
	"search_id": null,
	"hotel_list": [
		{
			"hotel_id": 50758,
			"hotel_name": "Best Western Plus Newark Airport West",
			"country_short": "US",
			"state_province": "New Jersey",
			"city": "Newark",
			"distance": null,
			"address": " 101 International Way Newark New Jersey 07114",
			"postal_code": "07114",
			"star": 3,
			"thumbnail": "https://s3.imgs.aichotels.net.cn/data37/50758/0/M.jpg",
			"latitude": "40.705099277344",
			"longitude": "-74.187492728233",
			"phone": "+19736216200",
			"picture_flag": 11,
			"night_rate_after_tax": "117.27",
			"night_rate_before_tax": "101.64",
			"currency": "USD",
			"weight": 1000
		}
	]
}
```
| Parameter  | Count | Type   | Description                |
| ---------- | ----- | ------ | -------------------------- |
| key_word   | 1     | Object | 与 输入相同 |
| hotel_list.search_id | 1     | Int    | 弃用             |
| hotel_list.hotel_list | 1     | Array  | 酒店价格列表 |
| hotel_list.hotel_id | 1     | Int    | 不为空的时候，将基于此字段搜索，忽略地理信息 |
|   hotel_list.hotel_name  | 1     | String | 酒店名称          |
| hotel_list.coutry_short | 1     | String | 国家名称缩写 |
| hotel_list.state_province | 1     | String | 州/省名                                |
| hotel_list.city | 1     | String | 城市名 |
|hotel_list.distance| 1| Float| 酒店到搜索点距离，单位为千米 |
| hotel_list.address | 1     | String| 酒店地址 |
| hotel_list.post_code | 1| String | 酒店邮编 |
| hotel_list.star | 1     | Float | 酒店星级    |
| hotel_list.thumbnail | 1 | String | 酒店小图标 |
| hotel_list.latitude | 1     | String | 酒店经度                                    |
| hotel_list.longitude | 1     | String | 酒店维度                                     |
| hotel_list.phone  | 1     | String | 酒店电话                                  |
| hotel_list.picture_flag | 1 | String | 弃用 |
| hotel_list.night_rate_before_tax | 1 | Float | 税前日均价|
| hotel_list.night_rate_after_tax | 1 | Float | 税后日均价 |
| hotel_list.weight | 1 | Float | 弃用 |

## 单酒店搜索(Room Availability)

单酒店搜索用来根据客人真实搜索条件来获取房态以及价格政策。单酒店接口有两个信息库，分别是：

- 缓存

  大部分请求会在 0.3s - 2s 返回

- 实时

  大部分请求会在 3s - 10s 返回

### 请求

> Room Availability 请求

```json
{
  "hotel_id": 112004,
  "check_in": "2019-11-09",
  "check_out": "2019-11-10",
  "room_number": 1,
  "adult_number": 2,
  "kids_number": 0
}
```
Cache Post `POST https://{{Endpoint}}/rate/public/cache/room_availability`

Live Post `POST https://{{Endpoint}}/rate/public/search/room_availability` 

| Parameter    | Count | Type   | Description |
| ------------ | ----- | ------ | ----------- |
| hotel_id     | 1     | Int    |    Amerilink 唯一酒店ID|
| check_in     | 1     | String | 入住日期    |
| check_out    | 1     | String | 退房日期    |
| room_number  | 1     | Int    | 预定减数    |
| adult_number | 1     | Int    | 入住成人数  |
| child        | 1     | Int    | 入住儿童数  |

### 返回

> Room Availability 返回 

```json
{
  "result": {
    "return_status": {
      "success": "true",
      "exception": ""
    }
  },
  "room_list": [{
    "room_name": "I KING HEARING ACC 1 KING BED HEARING ACCESSIBLE VIS FIREALRM\/DOOR\/PHN ALRT HI SPEED 9.95\/ DAY-SERENITY BED-LUX LINENS",
    "room_desc": "",
    "room_features": "",
    "room_type": "K1RRV",
    "rates_and_cancellation_policies": [{
      "rate_plan_code": "OG12TA",
      "meal_plan_code": "RO",
      "total_amount_before_tax": "128.74",
      "total_amount_after_tax": "145.49",
      "currency": "USD",
      "rates": [{
        "check_in": "2019-11-09",
        "check_out": "2019-11-10",
        "rooms": "1",
        "amount_before_tax": {
          "night_rate": "128.74",
          "sub_total": "128.74"
        },
        "amount_after_tax": {
          "night_rate": "145.49",
          "sub_total": "145.49"
        }
      }],
      "cancellation_information": {
        "support_cancel": "yes",
        "non_refundable": "no",
        "details": [{
          "datetime": "2019-11-04T00:00:00",
          "fee_type_value": "145.49",
          "fee_type": "total_amount",
          "amount_penalty": "145.49",
          "policy_code": "CXP"
        }, {
          "fee_type_value": "145.49",
          "fee_type": "total_amount",
          "amount_penalty": "145.49",
          "policy_code": "CNS"
        }],
        "timezone": "America\/Chicago UTC-06:00"
      },
      "room_key": "SzFSUlYuLipPRzEyVEEuLioxMTIwMDQ=",
      "rate_comments": "",
      "nationality": "|",
      "breakfast": {
        "include": 0,
        "count": 0
      },
      "meal_plan_desc": "\u201cRoom Only\u201d, \u201cIt only includes the stay for all guests in selected room. No meals are included.\u201d"
    }, 
    {
      "rate_plan_code": "OG12AP",
      "meal_plan_code": "RO",
      "total_amount_before_tax": "118.45",
      "total_amount_after_tax": "133.84",
      "currency": "USD",
      "rates": [{
        "check_in": "2019-11-09",
        "check_out": "2019-11-10",
        "rooms": "1",
        "amount_before_tax": {
          "night_rate": "118.45",
          "sub_total": "118.45"
        },
        "amount_after_tax": {
          "night_rate": "133.84",
          "sub_total": "133.84"
        }
      }],
      "cancellation_information": {
        "support_cancel": "yes",
        "non_refundable": "yes",
        "details": [{
          "datetime": "2018-11-06T00:00:00",
          "fee_type_value": "133.84",
          "fee_type": "total_amount",
          "amount_penalty": "133.84",
          "policy_code": "CXP"
        }, {
          "fee_type_value": "133.84",
          "fee_type": "total_amount",
          "amount_penalty": "133.84",
          "policy_code": "CNS"
        }],
        "timezone": "America\/Chicago UTC-06:00"
      },
      "room_key": "SzFSUlYuLipPRzEyQVAuLioxMTIwMDQ=",
      "rate_comments": "",
      "nationality": "|",
      "breakfast": {
        "include": 0,
        "count": 0
      },
      "meal_plan_desc": "\u201cRoom Only\u201d, \u201cIt only includes the stay for all guests in selected room. No meals are included.\u201d"
    }],
    "bed_info": null
  }, 
  {
    "room_name": "2 QN EXE LVL HEARING ACC 2 QN EXE LVL HEARING ACCESSIBLE VIS FIREALRM\/DOOR\/PHN ALRT HI SPEED 9.95\/ DAY-SERENITY BED-LUX LINENS",
    "room_desc": "",
    "room_features": "",
    "room_type": "Q2ERV",
    "rates_and_cancellation_policies": [{
      "rate_plan_code": "OG12TA",
      "meal_plan_code": "RO",
      "total_amount_before_tax": "149.07",
      "total_amount_after_tax": "168.45",
      "currency": "USD",
      "rates": [{
        "check_in": "2019-11-09",
        "check_out": "2019-11-10",
        "rooms": "1",
        "amount_before_tax": {
          "night_rate": "149.07",
          "sub_total": "149.07"
        },
        "amount_after_tax": {
          "night_rate": "168.45",
          "sub_total": "168.45"
        }
      }],
      "cancellation_information": {
        "support_cancel": "yes",
        "non_refundable": "no",
        "details": [{
          "datetime": "2019-11-04T00:00:00",
          "fee_type_value": "168.45",
          "fee_type": "total_amount",
          "amount_penalty": "168.45",
          "policy_code": "CXP"
        }, {
          "fee_type_value": "168.45",
          "fee_type": "total_amount",
          "amount_penalty": "168.45",
          "policy_code": "CNS"
        }],
        "timezone": "America\/Chicago UTC-06:00"
      },
      "room_key": "UTJFUlYuLipPRzEyVEEuLioxMTIwMDQ=",
      "rate_comments": "",
      "nationality": "|",
      "breakfast": {
        "include": 0,
        "count": 0
      },
      "meal_plan_desc": "\u201cRoom Only\u201d, \u201cIt only includes the stay for all guests in selected room. No meals are included.\u201d"
    }],
    "bed_info": null
  }]
}
```

| Parameter                                                | Count | Type   | Description                                                  |
| -------------------------------------------------------- | ----- | ------ | ------------------------------------------------------------ |
| room_list                                                | 1     | Array  | 所有可入住的房型列表                                         |
| room_list.room_name                                      | 1     | String | 房型名称                                                     |
| room_list.room_desc                                      | 1     | String | 房型简述                                                     |
| room_list.room_features                                  | 1     | String | 房间特色/设施                                                |
| room_list.room_type                                      | 1     | String | 房型代码                                                     |
| room_list.rates_and_cancellation_policies                | 1     | Array  | 所有有效的价格与其对应的取消政策数组                         |
| rates_and_cancellation_policies.rate_plan_code           | 1     | String | 价格方案的唯一标识                                           |
| rates_and_cancellation_policies.meal_plan_code           | 1     | String | 餐食方案的唯一标识                                           |
| rates_and_cancellation_policies.total_amount_before_tax  | 1     | String | 基于输入条件，预定所需的费用（不含税）                       |
| rates_and_cancellation_policies.total_amount_ after _tax | 1     | String | 基于输入条件，预定所需的费用（含税）                         |
| rates_and_cancellation_policies.currency                 | 1     | String | 费用基于的货币币种                                           |
| rates_and_cancellation_policies.rates                    | 1     | Array  | 价格政策                                                     |
| rates.check_in                                           | 1     | String | 入住日期                                                     |
| rates.check_out                                          | 1     | String | 退房日期                                                     |
| rates.rooms                                              | 1     | String | 总房间数                                                     |
| rates.amount_before_tax                                  | 1     | Object | 税前价格                                                     |
| rates.amount_before_tax.night_rate                       | 1     | Float  | 基于入住时间,税前一间房一晚的花销                            |
| rates.amount_before_tax.sub_total                        | 1     | Float  | 基于入住时间，退房时间的总花销（不含税）                     |
| rates.amount_after_tax                                   | 1     | Object | 税后价格                                                     |
| rates.amount_after_tax.night_rate                        | 1     | Float  | 税后一间房一晚的花销                                         |
| rates.amount_after_tax.sub_total                         | 1     | Float  | 基于入住时间，退房时间的总花销（含税）                       |
| rates_and_cancellation_policies.cancellation_information | 1     | Object | 此价格方案的取消政策                                         |
| cancellation_information.support_cancel                  | 1     | String | ‘yes’ 表明可以被取消,<br />‘no’ 不可被取消。                 |
| cancellation_information.non_refundable                  | 1     | String | ‘yes’ 表明取消不返款, ‘ no’ 表明取消返全款.‘partial’表明取消返还部分付款 |
| cancellation_information.details                         | 1     | Array  | 具体取消政策，为空时对接方需要调用下单前验证来获取取消政策.  |
| details.datetime                                         | 0...1 | String | 此条取消政策的生效时间                                       |
| details.fee_type_value                                   | 1     | String | 针对具体扣费类型的花费                                       |
| details.fee_type                                         | 1     | String | 具体扣费类型                                                 |
| details.amount_penalty                                   | 1     | String | 取消所需的花销                                               |
| details.policy_code                                      | 1     | String | CXP:一般取消政策；    CKP:过早退房；<br />CFC:更改行程；<br />CNS:未出现； |
| cancellation_information.timezone                        | 1     | String | 取消政策依据的时区                                           |
| rates_and_cancellation_policies.room_key                 | 1     | String | 用于识别具体的房型和对应的价格政策                           |
| rates_and_cancellation_policies.rate_comments            | 1     | String | 费用说明                                                     |
| rates_and_cancellation_policies.nationality              | 1     | String | 国籍(默认为CN)                                               |
| rates_and_cancellation_policies.breakfast                | 1     | Object | 早餐信息                                                     |
| breakfast.include                                        | 1     | Enum   | 0:不含早餐;<br />1:含早餐,<br />-1:不确定是否含早餐          |
| breakfast.count                                          | 1     | Int    | 包含的早餐数量                                               |
| cancellation_information.meal_plan_desc                  | 1     | String | 对于餐食计划的描述                                           |
| bed_info                                                 | 1     | String | 床型信息(默认为空)                                           |

## 验价(PreBook) 

验价接口需要使用 `room_key` 来验证价格以及取消政策

### 请求

> PreBook 请求

```json
{
  "hotel_id": 116729,
  "check_in": "2019-10-15",
  "check_out": "2019-10-16",
  "room_number": 1,
  "adult_number": 1,
  "kids_number": 1,
  "room_key":"RERCTC4uKjEwNDgyX0lQUlRBLi4qMTE2NzI5"
}
```
`POST https://{{Endpoint}}/rate/public/booking/prebook`

| Parameter    | Count | Type   | Description |
| ------------ | ----- | ------ | ----------- |
| hotel_id     | 1     | Int    |    Amerilink 唯一酒店ID|
| check_in     | 1     | String | 入住日期    |
| check_out    | 1     | String | 退房日期    |
| room_number  | 1     | Int    | 预定减数    |
| adult_number | 1     | Int    | 入住成人数  |
| child        | 1     | Int    | 入住儿童数  |
| room_key     | 1     | Int    | 搜索中获取的唯一房型Key |

### 返回

> PreBook 返回

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
			"room_name": "1 King Sofa Bed",
			"room_desc": " Grand Bed/Sitting Area/Sofabed/Corner Room",
			"room_features": " Large Work Space/55\" Flat Screen Tv",
			"room_type": "RM01",
			"rates_and_cancellation_policies": [
				{
					"rate_plan_code": "10482_IPRTA",
					"meal_plan_code": "RO",
					"total_amount_before_tax": "191.64",
					"total_amount_after_tax": "219.67",
					"currency": "USD",
					"rates": [
						{
							"check_in": "2020-01-19",
							"check_out": "2020-01-20",
							"rooms": "1",
							"amount_before_tax": {
								"night_rate": "191.64",
								"sub_total": "191.64"
							},
							"amount_after_tax": {
								"night_rate": "219.67",
								"sub_total": "219.67"
							}
						}
					],
					"cancellation_information": {
						"support_cancel": "yes",
						"non_refundable": "no",
						"details": [
							{
								"datetime": "2020-01-14T00:00:00",
								"fee_type_value": "1",
								"fee_type": "nights",
								"amount_penalty": "219.67",
								"policy_code": "CXP"
							},
							{
								"fee_type_value": "1",
								"fee_type": "nights",
								"amount_penalty": "219.67",
								"policy_code": "CNS"
							}
						],
						"timezone": "America/New_York UTC05:00"
					},
					"room_key": "Uk0wMS4uKjEwNDgyX0lQUlRBLi4qMTE2NzI5",
					"nationality": "|",
					"breakfast": {
						"include": 0,
						"count": 0
					},
					"meal_plan_desc": "“Room Only”, “It only includes the stay for all guests in selected room. No meals are included.”",
					"rate_comments": "费用仅包含房费及房费税费。不含其它费用"
				}
			]
		}
	]
}
```

| Parameter                                                | Count | Type   | Description                                                  |
| -------------------------------------------------------- | ----- | ------ | ------------------------------------------------------------ |
| room_list.room_list                                      | 1     | Array  | 房间以及价格的具体信息                                       |
| room_list.room_name                                      | 1     | String | 房型名称                                                     |
| room_list.room_desc                                      | 1     | String | 房型简述                                                     |
| room_list.room_features                                  | 1     | String | 房型特色                                                     |
| room_list.room_type                                      | 1     | String | 房型                                                         |
| room_list.rates_and_cancellation_policies                | 1     | Array  | 所有有效的价格与其对应的取消政策数组                         |
| rates_and_cancellation_policies.rate_plan_code           | 1     | String | 价格方案的唯一标识                                           |
| rates_and_cancellation_policies.meal_plan_code           | 1     | String | 餐食方案的唯一标识                                           |
| rates_and_cancellation_policies.total_amount_before_tax  | 1     | Float  | 基于输入条件，预定所需的费用（不含税）                       |
| rates_and_cancellation_policies.total_amount_ after _tax | 1     | Float  | 基于输入条件，预定所需的费用（含税）                         |
| rates_and_cancellation_policies.currency                 | 1     | String | 费用基于的货币币种                                           |
| rates_and_cancellation_policies.rates                    | 1     | Array  | 价格政策                                                     |
| rates.check_in                                           | 1     | String | 入住日期                                                     |
| rates.check_out                                          | 1     | String | 退房日期                                                     |
| rates.rooms                                              | 1     | String | 总房间数                                                     |
| rates.amount_before_tax                                  | 1     | Object | 税前价格                                                     |
| rates.amount_before_tax.night_rate                       | 1     | Float  | 税前一间房一晚的花销                                         |
| rates.amount_before_tax.sub_total                        | 1     | Float  | 基于入住时间，退房时间的总花销（不含税）                     |
| rates.amount_after_tax                                   | 1     | Object | 税后价格                                                     |
| rates.amount_after_tax.night_rate                        | 1     | Float  | 税后一间房一晚的花销                                         |
| rates.amount_after_tax.sub_total                         | 1     | Float  | 基于入住时间，退房时间的总花销（含税）                       |
| ates_and_cancellation_policies.cancellation_information  | 1     | Object | 此价格方案的取消政策                                         |
| cancellation_information.support_cancel                  | 1     | String | ‘yes’ 表明可以被取消, ‘no’ 不可被取消                        |
| cancellation_information.non_refundable                  | 1     | String | ‘yes’ 表明取消不返款， 'no'表明取消返全款，<br />‘partial’表明取消返还部分付款 |
| cancellation_information.details                         | 1     | Array  | 具体取消政策，为空时对接方需要调用下单前验证来获取取消政策.  |
| details.datetime                                         | 0...1 | String | 此条取消政策的生效时间                                       |
| details.fee_type_value                                   | 1     | String | 针对具体扣费类型的花费                                       |
| details.fee_type                                         | 1     | String | 具体扣费类型                                                 |
| details.amount_penalty                                   | 1     | String | 取消所需的花销                                               |
| details.policy_code                                      | 1     | String | CXP:一般取消政策；    CKP:过早退房；<br />CFC:更改行程；<br />CNS:未出现； |
| cancellation_information.timezone                        | 1     | String | 取消政策依据的时区                                           |
| rates_and_cancellation_policies.room_key                 | 1     | String | 用于识别具体的房型和对应的价格政策                           |
| rates_and_cancellation_policies.rate_comments            | 1     | String | 费用说明                                                     |
| rates_and_cancellation_policies.nationality              | 1     | String | 国籍(默认为CN)                                               |
| rates_and_cancellation_policies.breakfast                | 1     | Object | 早餐信息                                                     |
| breakfast.include                                        | 1     | Enum   | 0:不含早餐;<br />1:含早餐,<br />-1:不确定是否含早餐          |
| breakfast.count                                          | 1     | Int    | 包含的早餐数量                                               |
| cancellation_information.meal_plan_desc                  | 1     | String | 对于餐食计划的描述                                           |
| cancellation_information.rate_comments                   | 1     | String | 费用说明                                                     |

## 下单接口（Create Reservation）

下单接口会收集入住人信息，如称谓，姓名等。并在传输到Amerilink系统中生成订单后，返回酒店确认号，或美联确认号供出行人入住



### 请求

> Create Reservation 请求

```json
{
	"partner_reservation_id": "11711156241847",
	"hotel_id": "110722",
	"room_key": "SzFELi4qT0RDQzMwLi4qMTEzNjQ1",
	"check_in": "2017-10-19",
	"check_out": "2017-10-24",
	"room_number": 2,
	"adult_number": 2,
	"kids_number": 1,
	"guests": [
		{
			"adult_name_prefix": [
				"Mr",
				"Mr"
			],
			"adult_given_name": [
				"Yi",
				"Er"
			],
			"adult_surname": [
				"Zhao",
				"Li"
			],
			"child_name_prefix": [
				"Master"
			],
			"child_given_name": [
				"San"
			],
			"child_surname": [
				"Wang"
			]
		},
		{
			"adult_name_prefix": [
				"Mr",
				"Mr"
			],
			"adult_given_name": [
				"Si",
				"Wu"
			],
			"adult_surname": [
				"Zhang",
				"Zhang"
			],
			"child_name_prefix": [
				"Master"
			],
			"child_given_name": [
				"Liu"
			],
			"child_surname": [
				"Wang"
			]
		}
	],
	"special_request": "late arrive",
	"children_ages": [
		[
			5
		],
		[
			12
		]
	],
	"card_type": "VI",
	"card_number": "4444333322222111",
	"expire_date": "0815",
	"card_holder": {
		"name_prefix": "Mr",
		"given_name": "Yi",
		"surname": "Li"
	},
	"total_before_tax": 3783.22,
	"total_after_tax": 3932.33,
	"currency": "USD"
}
```

`POST https://{{Endpoint}}/rate/public/booking/create_reservation`

| Parameter              | Count | Type   | Description                          |
| ---------------------- | ----- | ------ | ------------------------------------ |
| partner_reservation_id | 1     | String | 美联合作的分销商订单编号。需保持唯一 |
| hotel_id               | 1     | String | 美联唯一酒店ID                       |
| room_key               | 1     | String | 产品唯一密匙                         |
| check_in               | 1     | String | 入住日期                             |
| check_out              | 1     | String | 退房日期                             |
| room_number            | 1     | Int    | 房间数                               |
| adult_number           | 1     | Int    | 入住成人数                           |
| kids_number            | 1     | Int    | 入住儿童数                           |
| guests                 | 1     | Array  | 客人信息                             |
| adult_name_prefix      | 1     | String | 客人称谓                             |
| adult_given_name       | 1     | String | 客人名                               |
| adult_surname          | 1     | String | 客人姓                               |
| child_name_prefix      | 1     | String | 儿童客人称谓                         |
| child_given_name       | 1     | String | 儿童名                               |
| child_surname          | 1     | String | 儿童姓                               |
| special_request        | 1     | String | 特殊要求                             |
| children_ages          | 1     | Array  | 儿童年龄                             |
| card_type              | 1     | String | 用于支付的卡的类型                   |
| card_number            | 1     | String | 卡号                                 |
| expire_date            | 1     | String | 卡的过期时间                         |
| card_holder            | 1     | object | 卡的持有者信息                       |
| total_before_tax       | 1     | Float  | 订单的总费用（不含税）               |
| total_after_tax        | 1     | Float  | 订单的总费用（含税）                 |
| currency               | 1     | String | 订单所使用的货币币种                 |

<aside class="warning">如无儿童入住，则 <code>child_name_prefix</code>, <code>child_given_name</code>,以及<code>child_surname</code> 需要默认为 <code>Null</code> 而不是删去入参。否则接口会报错</aside>
### 返回

> Create Reservation 返回

```json
{
	"result": {
		"return_status": {
			"success": "true",
			"exception": ""
		}
	},
	"reservation_ids": {
		"aic_reservation_id": "86355202",
		"hotel_confirmation_number": "50801881",
		"partner_reservation_id": "test2019122054575610155",
		"confirmation_number_type": 2
	},
	"supplier_info": {
		"name": null,
		"vat_number": null,
		"rate_comments": null
	},
	"board_info": {
		"meal_plan_code": null,
		"meal_plan_desc": null
	},
	"reservation_status": 2,
	"created_at": 1576834574
}

```

| Parameter                                 | Count | Type   | Description                |
| ----------------------------------------- | ----- | ------ | -------------------------- |
| reservation_ids                           | 1     | Object | 预定信息                   |
| reservation_ids.hotel_confirmation_number | 1     | String | 酒店确认号                 |
| reservation_ids.aic_reservation_id        | 1     | String | 美联国际确认号             |
| reservation_ids.partner_reservation_id    | 1     | String | 美联国际合作的分销商确认号 |
| reservation_ids.confirmation_number_type  | 1     | Int    | 确认号类型(非必要参数)     |
| reservation_status                        | 1     | String | 订单状态                   |
| created_at                                | 1     | String | 订单创建时间(时间戳)       |

## 取消订单（Cancel Reservation）

取消订单接口需要用分销商的订单号来取消订单。如有取消罚金则根据PreBook接口中返回的取消政策来计算。

### 请求 

`DELETE https://{{Endpoint}}/rate/public/booking/cancel_reservation/{{partner_reservation_id}} `

### 返回

> Cancel Reservation 返回

```json
{
  "result": {
    "return_tatus": {
      "success": "true",
      "exception": ""
    }
  },
  "cancellation_ids": {
    "hotel_cancellation_number": "AIJ262517",
    "aic_reservation_id": "213449878",
    "partner_reservation_id": "11711156241847",
  },
  "reservation_status": 4,
  "cancel_ charge": {
    "penalty": 0.00,
    "currency": "USD"
  },
  "cancellation _time": "2017-08-28 13:24:12"
}
```

| Parameter                 | Count | Type   | Description                                             |
| ------------------------- | ----- | ------ | ------------------------------------------------------- |
| cancellation_ids          | 1     | Object | 取消信息                  |
| hotel_cancellation_number | 1     | String | 酒店集团返回的取消编号       |
| aic_reservation_id        | 1     | String | Amerilink 唯一订单号  |
| partner_reservation_id    | 1     | String | Amerilink 合作的分销商订单号 |
| reservation_status        | 1     | Int    | 订单状态，详情见附录 |
| cancel_charge             | 1     | Object | 罚金相关信息     |
| penalty                   | 1     | Float  | 取消产生的罚金                                |
| currency                  | 1     | String | 罚金结算货币种类，暂时只接受美元                       |
| cancellation_time         | 1     | String | 订单取消时间                        |

## 订单查询（Booking Read）

订单查询接口可以用两个维度来查询历史订单：

- 分销商订单号
- 订单生成日期

### 请求

> Booking Read 请求

```json
{
  "type": 2,
   "partner_reservation_ids": ["213255382", "213254597", "213248481"],
  "time_range": {
    "from": "2017-06-30",
    "end": "2017-07-07"
  }
}
```
`POST https://{{Endpoint}}/rate/public/booking/read`

| Parameter               | Count | Type   | Description                                                  |
| ----------------------- | ----- | ------ | ------------------------------------------------------------ |
| type                    | 1     | Int    | 如为1则会根据订单号查询，如为0则会根据下单日查询 |
| partner_reservation_ids | 1     | Array  | 需查询的订单列表                         |
| time_range              | 1     | Object | 查询日期                   |
| from                    | 1     | String | 查询日期起始日                                 |
| end                     | 1     | String | 查询日期结束日                                    |

### 返回

> Booking Read 返回

```json
{
  "result": {
    "return_status": {
      "status": "success ",
      "exception": ""
    }
  },
  "reservation_list": [{
    "hotel_id": 50449,
    "aic_reservation_id": "213386199",
    "partner_reservation_id": "11711156241847",
    "reservation_status": 2,
    "hotel_confirmation_number": null,
    "hotel_cancellation_number": null,
    "check_in_date": "2017-06-27",
    "check_out_date": "2017-06-28",
    "adult_number": 2,
    "kids_number": 1,
    "kids_number": 1,
    "guests": [

      {
        "adult_ name_prefix ": [
          "Mr",
          "Mr"
        ],
        "adult_given_name": [
          "Yi",
          "Er"
        ],
        "adult_surname": [
          "Zhao",
          "Li"
        ],
        "child_name_prefix": [
          "Master"
        ],
        " child_given_name": [
          "San"
        ],
        " child_surname": [
          "Wang"

        ]
      },

      {
        "adult_ name_prefix": [
          "Mr",
          "Mr"
        ],
        "adult_given_name": [
          "Si",
          "Wu"
        ],
        " adult_surname": [
          "Zhang",
          "Zhang"
        ],
        " child_name_prefix ": [
          "Master"
        ],
        " child_given_name ": [
          "Liu"
        ],
        " child_surname": [
          "Wang"
        ]
      }
    ],
    "special_request": "late arrive",
    "children_ages": [
      [
        5
      ],
      [
        12
      ]
    ],
    "total_rooms": 2,
    "total_amount": "121.31",
    "currency ": "USD"
  }, ]
}
```

| Parameter                 | Count | Type   | Description              |
| ------------------------- | ----- | ------ | ------------------------ |
| reservation_list          | 1     | Array  | 搜索结果列表             |
| hotel_id                  | 1     | Int    | Amerilink 唯一酒店ID     |
| aic_reservation_id        | 1     | String | Amerilink 确认号         |
| partner_reservation_id    | 1     | String | 分销商确认号             |
| reservation_status        | 1     | Int    | 订单状态，详情见附录     |
| hotel_confirmation_number | 1     | String | 酒店确认号               |
| hotel_cancellation_number | 1     | String | 酒店取消编号             |
| check_in_date             | 1     | String | 入住日期                 |
| check_out_date            | 1     | String | 退房日期                 |
| adult_number              | 1     | Int    | 入住成人数               |
| kids_number               | 1     | Int    | 入住儿童数               |
| guests                    | 1     | Array  | 客人信息                 |
| adult_name_prefix         | 1     | String | 入住人称谓               |
| adult_given_name          | 1     | String | 入住人名                 |
| adult_surname             | 1     | String | 入住人姓                 |
| child_name_prefix         | 1     | String | 儿童称谓                 |
| child_given_name          | 1     | String | 儿童名                   |
| child_surname             | 1     | String | 儿童姓                   |
| special_request           | 1     | String | 下单时使用的特殊请求     |
| children_ages             | 1     | String | 儿童年龄                 |
| total_rooms               | 1     | Int    | 订单中房间总数           |
| total_amount              | 1     | Float  | 结算金额                 |
| currency                  | 1     | String | 结算币种，暂时只支持美元 |

