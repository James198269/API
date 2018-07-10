# 认证
BI.TOP 使用 API key 和 API secret 进行验证，请访问 设置中心，并注册成为开发者，获取 API key 和 API secret。

BI.TOP[www.bi.top](https://www.bi.top "www.bi.top") 的 API 请求，除公开的 API 外都需要携带 API key 以及签名

# 访问限制
目前访问频率为每个用户 100次 / 10秒，未来会按照业务区分访问频率限制。

# API 签名
签名前准备的数据如下：

HTTP_METHOD + HTTP_REQUEST_URI + TIMESTAMP + POST_BODY

连接完成后，进行 Base64 编码，对编码后的数据进行 HMAC-SHA1 签名，并对签名进行二次 Base64 编码，各部分解释如下：
 **请注意需要进行两次 `Base64` 编码！**
 
#  HTTP_METHOD
GET, POST, DELETE, PUT 需要大写

# HTTP_REQUEST_URI
https://api.bi.top.com/v2/ 为 v2 API 的请求前缀

后面再加上真正要访问的资源路径，如 orders?param1=value1，最终即 https://api.bi.top.com/v2/orders?param1=value1

对于请求的 URI 中的参数，需要按照按照字母表排序！

即如果请求的 URI 为 https://api.bi.top.com/v2/orders?c=value1&b=value2&a=value3，则进行签名时，应先将请求参数按照字母表排序，最终进行签名的 URI 为 https://api.bi.top.com/v2/orders?a=value3&b=value2&c=value1， 请注意，原请求 URI 中的三个参数顺序为 c, b, a，排序后为 a, b, c。

# TIMESTAMP
访问 API 时的 UNIX EPOCH 时间戳，需要和服务器之间的时间差少于 30 秒

# POST_BODY
如果是 POST 请求，POST 请求数据也需要被签名，签名规则如下：

所有请求的 key 按照字母顺序排序，然后进行 url 参数化，并使用 & 连接。

 **请注意 POST_BODY 的键值需要按照字母表排序！**
 
#  完整示例
# 查询服务器时间
    
**简要描述：** 

- 此 API 用于获取服务器时间

**请求URL：** 
- ` http://xx.com/v1/public/server-time `
  
**请求方式：**
- GET 

**参数：** 

无

 **返回示例**

 ``` 
 { 
     "exeStatus" : "1", 
     "data" : 1530179524864 
 } 

 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data | long | 服务器时间，毫秒值

 **备注** 

- 更多返回错误代码请看首页的错误代码描述


# 查询可用币种
**简要描述：** 

- 此 API 用于获取可用币种。

**请求URL：** 
- ` http://xx.com/v1/public/currencies `
  
**请求方式：**
- GET 

**参数：** 

无

 **返回示例**
```
{
	"exeStatus": "1",
	"data": [
		"snt", 
		"btc",
		"ltc",
		"usdt", 
		"dnt", 
		"btm",
		"eos",
		"eth",
		"qtum",
		"omg"
		]
}
```



 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data | list | 币种简称
 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 查询可用交易对
**简要描述：** 

- 此 API 用于获取可用交易对。

**请求URL：** 
- ` http://xx.com/v1/public/symbols `
  
**请求方式：**
- GET 

**参数：** 

无

 **返回示例**

 ``` 
{
	"exeStatus": "1",
	"data": [{
		"amount_decimal": "1",
		"base_currency": "bnt",
		"name": "bntbtc",
		"price_decimal": "8",
		"quote_currency": "btc"
	}, {
		"amount_decimal": "2",
		"base_currency": "qtum",
		"name": "qtumbtc",
		"price_decimal": "6",
		"quote_currency": "btc"
	}, {
		"amount_decimal": "1",
		"base_currency": "eos",
		"name": "eosbtc",
		"price_decimal": "8",
		"quote_currency": "btc"
	}, {
		"amount_decimal": "6",
		"base_currency": "btc",
		"name": "btcusdt",
		"price_decimal": "2",
		"quote_currency": "usdt"
	}, {
		"amount_decimal": "2",
		"base_currency": "ltc",
		"name": "ltcbtc",
		"price_decimal": "6",
		"quote_currency": "btc"
	}, {
		"amount_decimal": "5",
		"base_currency": "eth",
		"name": "ethusdt",
		"price_decimal": "2",
		"quote_currency": "usdt"
	}, {
		"amount_decimal": "1",
		"base_currency": "snt",
		"name": "sntbtc",
		"price_decimal": "8",
		"quote_currency": "btc"
	}, {
		"amount_decimal": "3",
		"base_currency": "eth",
		"name": "ethbtc",
		"price_decimal": "6",
		"quote_currency": "btc"
	}]
} 

 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data|list|交易对数据封装|
|name|String|交易对简称|
|base_currency|String|基础币种|
|quote_currency|String|计价币种|
|price_decimal|String|价格精度位数|
|amount_decimal|String|数量精度位数|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述


# 行情描述
行情是一个全公开的 API, 当前仅提供了 HTTP的 API. 

所有 HTTP 请求的 URL base 为: https://api.fcoin.com/v2/market

下文会统一术语:

symbol 表示对应交易币种.
ticker 行情 tick 信息, 包含最新成交价, 最新成交量, 近 24 小时成交量.
depth 表示行情深度, 买卖盘, 盘口.
level 表示行情深度类型. 如 L20, L100，暂时仅支持L100.
trade 表示最新成交, 最新交易.
candle 表示蜡烛图, 蜡烛棒, K 线.
resolution 表示蜡烛图的种类. 如 M1, M15.
base volume 表示基准货币成交量, 如 btcusdt 中 btc 的量.
quote volume 表示计价货币成交量, 如 btcusdt 中 usdt 的量
ts 表示推送服务器的时间. 是毫秒为单位的数字型字段, unix epoch in millisecond.
    
# 获取ticker数据
**简要描述：** 

- 为了使得 ticker 信息组足够小和快, 我们强制使用了列表格式.

**请求URL：** 
- ` http://xx.com/api/v1/market/ticker/$symbol `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|symbol |是  |string |交易币种，如btcusdt,ethbtc   |

 **返回示例**

 ``` 
{
	"exeStatus": "1",
	"data": {
		"type": "ticker.ethbtc",
		"ticker": [
			0.777770,
			0.108082,
			33461.989,
			0.108083,
			94.746,
			0.777770,
			0.777770,
			0.777770,
			0.000,
			0.000000
			]
		}
	} 

 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data|map|ticker返回值封装|
|type |String   |ticker的类型  |
|ticker |list   |ticker详细字段的封装  |

**ticker对应字段含义如下：**
``` 
[
	"最新成交价", 
	"最大买一价",
	"最大买一量",
	"最小卖一价",
	"最小卖一量",
	"24小时前成交价", 
	"24小时内最高价", 
	"24小时内最低价", 
	"24小时内基准货币成交量, 如 btcusdt 中 btc 的量", 
	"24小时内计价货币成交量, 如 btcusdt 中 usdt 的量"
]

 ```
 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 获取最新的深度明细
**简要描述：** 

- 此API用于获取最新的深度明细，目前行情深度暂仅支持100档。

**请求URL：** 
- ` http://xx.com/api/v1/market/depth/$level/$symbol `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|symbol |是  |string |交易币种，如btcusdt,ethbtc   |
|level |否  |string |行情深度，如L20表示20档行情深度，L100表示100档行情深度   |

 **返回示例**

 ``` 
{
	"exeStatus": "1",
	"asks": [],
	"bids": [0.073330, 1.00000000],
	"type": "depth.L20.ethbtc",
	"ts": 1530181831170
}
 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|type |String   |行情的类型，如depth.L20.ethbtc，depth.L100.ethbtc  |
|ts |long   |响应生成时间点，单位：毫秒  |
|bids|list|买盘,[price(成交价), amount(成交量),price(成交价), amount(成交量)], 按price降序|
|asks|list|卖盘,[price(成交价), amount(成交量),price(成交价), amount(成交量)], 按price升序|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 获取最新的成交明细
**简要描述：** 

- 此API用于获取最新的成交明细。

**请求URL：** 
- ` http://xx.com/api/v1/market/trades/$symbol `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|symbol |是  |string |交易币种，如btcusdt,ethbtc   |
|limit |否  |string |条数，默认20，最多100   |

 **返回示例**

 ``` 
{
	"exeStatus": "1",
	"ts": 1530183006962,
	"data": [{
		"amount": 0.000,
		"price": 0.073330,
		"ts": 1530169322000
	}, {
		"amount": 0.000,
		"price": 0.073330,
		"ts": 1530169309000
	}, {
		"amount": 0.000,
		"price": 0.073330,
		"ts": 1530169295000
	}]
}
 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|ts |long   |响应生成时间点，单位：毫秒  |
|data|list|成交数据的封装|
|amount|BigDecimal|成交量|
|price|BigDecimal|成交价|
|ts|BigDecimal|成交时间|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 获取Candle信息
**简要描述：** 

- 此API用于获取Candle信息。

**请求URL：** 
- ` http://xx.com/api/v1/market/candles/$resolution/$symbol `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|symbol |是  |string |交易币种，如btcusdt,ethbtc   |
|resolution |是  |string |蜡烛图的种类，如M1表示1分钟的K线，M5表示五分钟的K线   |
|limit |否  |string |条数，默认20，范围是1-1000   |

**$resolution 包含的种类**

|类型|说明|
|:----    |:---|
|M1|1分钟|
|M5|5分钟|
|M15|15分钟|
|M30|30分钟|
|H1|1小时|
|H4|4小时|
|D1|1日|
|W1|1周|
|MN|1月|

 **返回示例**

 ``` 
{
	"exeStatus": "1",
	"type": "candle.M1.ethbtc",
	"data": [{
		"quote_vol": 0.000000,
		"high": 0.777770,
		"low": 0.777770,
		"base_vol": 0.000,
		"close": 0.777770,
		"open": 0.777770
	}, {
		"quote_vol": 0.000000,
		"high": 0.777770,
		"low": 0.777770,
		"base_vol": 0.000,
		"close": 0.777770,
		"open": 0.777770
	}, {
		"quote_vol": 0.000000,
		"high": 0.777770,
		"low": 0.777770,
		"base_vol": 0.000,
		"close": 0.777770,
		"open": 0.777770
	}, {
		"quote_vol": 0.000000,
		"high": 0.777770,
		"low": 0.777770,
		"base_vol": 0.000,
		"close": 0.777770,
		"open": 0.777770
	}, {
		"quote_vol": 0.000000,
		"high": 0.777770,
		"low": 0.777770,
		"base_vol": 0.000,
		"close": 0.777770,
		"open": 0.777770
	}]
}
 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|type|String|K线类型|
|data|list|K线数据的封装|
|open|BigDecimal|开盘价|
|close|BigDecimal|收盘价|
|high|BigDecimal|最高价|
|low|BigDecimal|最低价|
|base_vol|BigDecimal|基准货币成交量, 如 btcusdt 中 btc 的量|
|quote_vol|BigDecimal|计价货币成交量, 如 btcusdt 中 usdt 的量|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述
# 查询账户资产
    
**简要描述：** 

- 此API用于查询用户的资产列表

**请求URL：** 
- ` http://xx.com/v1/accounts/balance `
  
**请求方式：**
- GET 

**参数：** 
无

 **返回示例**

``` 
  {
	"exeStatus": "1",
	"data": [{
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "snt"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "btc"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "ltc"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "usdt"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "btm"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "eos"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "eth"
	}, {
		"balance": "0.00000000",
		"available": "0.00000000",
		"frozen": "0.00000000",
		"currency": "omg"
	}]
}
```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data |jsonArray   |用户资产余额的封装  |
|currency|String|资产简称|
|balance|String|资产总额|
|available|String|可用余额|
|frozen|String|冻结余额|


 **备注** 

- 更多返回错误代码请看首页的错误代码描述


# 订单模型说明
订单模型由以下属性构成：

|属性|类型|含义解释|
|:----    |:---|:----- |
|id|String|订单ID|
|symbol|String|交易对|
|side|String|交易方向（buy, sell）|
|type|String|订单类型（limit，market）|
|price|String|下单价格|
|amount|String|下单数量|
|state|String|订单状态|
|executed_value|String|已成交总金额|
|filled_amount|String|已成交量|
|fill_fees|String|手续费|
|created_at|Long|创建时间|
|source|String|来源|

订单状态说明：

|属性|含义解释|
|:----    |:---|
|submitted|已提交|
|filled|完全成交|
|canceled|已撤销|

# 创建新的订单
**简要描述：** 

- 此 API 用于创建新的订单。

**请求URL：** 
- ` http://xx.com/api/v1/orders `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|symbol |是  |string |交易币种，如btcusdt，ethbtc   |
|side |是  |string |交易方向，如buy，sell   |
|type |是  |string |订单类型，如limit，market   |
|price |是  |string |下单价格   |
|amount |是  |string |下单数量   |

 **返回示例**

 ``` 
 { 
     "exeStatus" : "1", 
     "data" : "B180628214433836336014" 
 } 

 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data|String|订单ID|

 **备注** 
- 市价下单不需要传price
- 更多返回错误代码请看首页的错误代码描述

# 查询订单列表
**简要描述：** 

- 此API用于查询订单列表。

**请求URL：** 
- ` http://xx.com/api/v1/orders `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|symbol |是  |string |交易币种，如btcusdt,ethbtc   |
|states |是  |string |订单状态，如submitted   |
|before |否  |string |查询某个页码之前的订单 |
|after |否  |string |查询某个页码之后的订单|
|limit |否  |string |每页的订单数量，默认为 20 条，最大支持到100条|

**注意before和after只能有1个时间戳**

 **返回示例**

 ``` {
	"exeStatus": "1",
	"data": [{
		"amount": "1.00000000",
		"created_at": "1528422687000",
		"executed_value": "0",
		"fill_fees": "0",
		"filled_amount": "0.51200000",
		"id": "B180608095127029911718",
		"price": "0.88800000",
		"side": "buy",
		"source": "web",
		"state": "submitted",
		"symbol": "ethbtc",
		"type": "limit"
	}, {
		"amount": "1.00000000",
		"created_at": "1528422715000",
		"executed_value": "0",
		"fill_fees": "0",
		"filled_amount": "0.00000000",
		"id": "B180608095155628600230",
		"price": "0.71000000",
		"side": "buy",
		"source": "web",
		"state": "submitted",
		"symbol": "ethbtc",
		"type": "limit"
	}, {
		"amount": "1.00000000",
		"created_at": "1528820139000",
		"executed_value": "0",
		"fill_fees": "0",
		"filled_amount": "0.48800000",
		"id": "B180613121539563947537",
		"price": "0.87777000",
		"side": "buy",
		"source": "web",
		"state": "submitted",
		"symbol": "ethbtc",
		"type": "limit"
	}, {
		"amount": "1.00000000",
		"created_at": "1528875028000",
		"executed_value": "0",
		"fill_fees": "0",
		"filled_amount": "0.00000000",
		"id": "B180613153028857098126",
		"price": "0.77777000",
		"side": "buy",
		"source": "web",
		"state": "submitted",
		"symbol": "ethbtc",
		"type": "limit"
	}, {
		"amount": "0.00100000",
		"created_at": "1528882031000",
		"executed_value": "0",
		"fill_fees": "0",
		"filled_amount": "0.00000000",
		"id": "B180613172711073534467",
		"price": "0.77777000",
		"side": "buy",
		"source": "web",
		"state": "submitted",
		"symbol": "ethbtc",
		"type": "limit"
	}]
}
 ```

 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data |list|订单列表数据封装|
|created_at |long   |创建时间|
|amount|String|下单数量|
|executed_value|String|已成交总金额|
|fill_fees |String|成交手续费|
|filled_amount|String|已成交数量|
|id|String|订单ID|
|price |String   |下单价格|
|side|String|交易方向|
|source|String|来源|
|state |String|订单状态|
|symbol|String|交易对|
|type|String|交易类型|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 获取指定订单
**简要描述：** 

- 此API用于获取指定订单。

**请求URL：** 
- ` http://xx.com/api/v1/orders/{order_id} `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|order_id |是  |string |订单ID，如B180608095127029911718|

 **返回示例**

 ``` 
 { 
     "exeStatus" : "1", 
     "data" : { 
          "amount" : "1.00000000", 
          "created_at" : "1528422687000", 
          "executed_value" : "0", 
          "fill_fees" : "0", 
          "filled_amount" : "0.51200000", 
          "id" : "B180608095127029911718", 
          "price" : "0.88800000", 
          "side" : "buy", 
          "source" : "web", 
          "state" : "submitted", 
          "symbol" : "ethbtc", 
          "type" : "limit" 
     } 
 } 

 ```



 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data |list|订单列表数据封装|
|created_at |long   |创建时间|
|amount|String|下单数量|
|executed_value|String|已成交总金额|
|fill_fees |String|成交手续费|
|filled_amount|String|已成交数量|
|id|String|订单ID|
|price |String   |下单价格|
|side|String|交易方向|
|source|String|来源|
|state |String|订单状态|
|symbol|String|交易对|
|type|String|交易类型|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 申请撤销订单
**简要描述：** 

- 此API用于申请撤销订单。

**请求URL：** 
- ` http://xx.com/api/v1/orders/{order_id}/submit-cancel `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|order_id |是  |string |订单ID，如B180608095127029911718|

 **返回示例**
 
 ``` 
 { 
     "exeStatus" : "1", 
     "msg" : "B180608095127029911718", 
     "data" : "true" 
 } 

 ```
 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|msg|String|撤单ID|
|data|String|true:撤单成功；false:撤单失败|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述

# 查询指定订单的成交记录
**简要描述：** 

- 此API用于查询指定订单的成交记录。

**请求URL：** 
- ` http://xx.com/api/v1/orders/{order_id}/match-results `
  
**请求方式：**
- GET 

**参数：** 

|参数名|必须|类型|说明|
|:----    |:---|:----- |-----   |
|order_id |是  |string |订单ID，如B180608095127029911718|

 **返回示例**
 
 ``` 
{
	"exeStatus": "1",
	"data": [{
		"side": "buy",
		"price": "0.91110000",
		"created_at": 1529649344000,
		"type": "market",
		"filled_amount": "0",
		"fill_fees": "0"
	}, {
		"side": "buy",
		"price": "1.00000000",
		"created_at": 1529858160000,
		"type": "market",
		"filled_amount": "0.769",
		"fill_fees": "0.01538"
	}]
}

 ```
 **返回参数说明** 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|exeStatus |String   |执行结果（1：操作成功；0：操作失败，操作失败时会有errorCode）  |
|data|list|成交记录封装|
|side|String|交易方向|
|price|String|成交价|
|created_at|Long|成交时间|
|type|String|交易类型|
|filled_amount|String|成交量|
|fill_fees|String|成交手续费|

 **备注** 

- 更多返回错误代码请看首页的错误代码描述
# 错误代码

|错误代码|含义解释|
|:---|:---|
|400|Bad Request -- 错误的请求|
|401|Unauthorized -- API key 或者签名，时间戳有误|
|403|Forbidden -- 禁止访问|
|404|Not Found -- 未找到请求的资源|
|405|Method Not Allowed -- 使用的 HTTP 方法不适用于请求的资源|
|406|Not Acceptable -- 请求的内容格式不是 JSON|
|429|Too Many Requests -- 请求受限，请降低请求频率|
|500|Internal Server Error -- 服务内部错误，请稍后再进行尝试|
|503|Service Unavailable -- 服务不可用，请稍后再进行尝试|
