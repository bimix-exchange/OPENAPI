## Description

## API Domain

```
https://api.bimix.com
```
## Return code
All API return the code field. The value of the code field is the string "000000" which means success, and all other values ​​are failures.

# Public API

## ping

** desc: **

-Test whether the server is connected

** Request method and request URL: **
-GET `/v1/ping`

** Parameter: **
None

** Return example **
```
{
"code": "000000"
}
```

** Return value description **
no

## timestamp

**A brief description:** 

-Get server time
-GET `/v1/timestamp`
Get server time


** Parameter: **
None

** Return example **
```
{
  "code": "000000",
  "data": {
    "timezone": "UTC",
    "timestamp": 1499827319559
  },
  "msg": "success"
}
```

 **Return value description**

| Field name | Type | Description |
| :-------- | :----- | ------ |
| timezone | string | time zone |
| timestamp | integer | timestamp |

 ** Remarks **

-No




## Market (Trading Pair) Information

**A brief description:** 

-Get trading pair details

** Request method and request URL: **
-GET `/v1/public/spot/market`

** Parameter: **

None

 ** Return example **

```
{
    "code": "000000",
    "data": {
"BTC / USDT": {
"limits": {
"amount": {
"min": 0.001,
"max": 9000
},
"price": {
"min": 0
}
},
        "precision": {
            "amount": 4,
            "price": 2
        },
        "percentage": true,
        "taker": 0.001,
        "maker": 0.001,
        "id": "BTCUSDT",
        "symbol": "BTC / USDT",
        "base": "BTC",
        "quote": "USDT",
        "baseId": "BTC",
        "quoteId": "USDT",
        "active": true
},
"ETH/USDT": {
....
}
},
    "msg": "OK"
}
```

 ** Return value description **

| Parameter name | Type | Description |
| :----- | :-------------------------- | ---------------|
| data | Map <SymbolName, SymbolInfo> | Details of trading pairs |

SymbolName: string, for example `BTC/USDT`

** SymbolInfo structure: **

| Field name | Type | Description |
| :-------- | :----- | -------------------- |
| limits | Dict | trading limit information for trading pairs |
| precision | Dict | trading pair accuracy information |
| taker | float | taker fee |
| maker | float | maker handling fee |
| id | string | trading pair id |
| symbol | string | trading symbol |
| base | string | trading pair / former currency name |
| quote | string | trading pair / post currency name |
| baseId | string | trading pair / pre-currency id |
| quoteId | string | trading pair / post currency id |
| active | string | trading pair status |

 ** Remarks **

-No



## ticker

**A brief description:** 

-Get the latest quotes of trading pairs

** Request method and request URL: **
-GET `/v1/public/spot/ticker`

** Parameter: **

| Parameter name | Type | Required | Description |
| :----- | :----- | :------- | -------------------- |
| s | string | no | trading pair name, / use-instead |

* If the parameter symbol exists, return the ticker of the symbol, otherwise return the ticker of all symbols

** Return example **

```
{
    "code": "000000",
    "data": {
"BTC / USDT": {
"s": "BTC/USDT",
"t": 1560994431098,
"o": 1000,
"h": 1050,
"l": 990,
"c": 1020,
"p": 0.02,
"cg": 20,
"v": 10,
"to": 10080,
"a": 1022,
"b": 1019,
"aa": 5,
"ba": 5.5,
}
},
    "msg": "OK"
}
```

 ** Return value description **

| Field name | Type | Description |
| :----- | :----- | ----------------------------- |
| t | long | Current timestamp in milliseconds |
| s | string | symbol trading pairs, such as BTC / USDT |
| o | float | open opening price |
| h | floating point number | high
| l | floating point number | low lowest price |
| c | floating point | close closing price |
| p | floating point | percent increase |
| cg | floating point number | change, opening price-closing price |
| v | floating point number | volume volume, calculated in base |
| to | floating point number | turnover, turnover, in quotes |
| a | float | ask, sell for one price |
| b | floating point number | bid, buy one price |
| aa | float | askAmount to sell a pending order amount |
ba | floating point number | bidAmount buy a pending order amount |


 ** Remarks **

-If you only need to get the latest price information, it is recommended to use the ticker price interface, which is faster
-If symbol is not specified, the ticker data of all symbols is returned. In the data data, the symbol of each trading pair is used as the key value, and the value is the ticker data of the symbol



## depth

**A brief description:** 

-Get in-depth information about trading pairs

** Request method and request URL: **
-GET `/v1/public/spot/depth`

** Parameter: **

| Parameter name | Type | Required | Description |
| :----- | :------ | :--------- | -------------------------------------- |
| s | string | yes | trading pair name, / replaced with-|
| depth | integer | No, default 20 | The number of depths returned, optional values: 5,10,20,50,100 |

** Request example **
```
GET /v1/public/spot/depth?S=BTC-USDT&depth=5
```

** Return example **

```
{
    "code": "000000",
    "data": {
        "symbol": "BTC / USDT",
"asks": [["9.9", "1"], ["9.89", "0.4"], ["9.88", "10"], ["9.7", "13.2"], ["9.68", "3.2"]],
"bids": [["10", "9.8"], ["10.1", "1.2"], ["10.15", "0.9"], ["10.2", "3.55"], ["10.3", "1.6"]]
},
    "msg": "OK"
}
```

 ** Return value description **

| Field name | Type | Description |
| :----- | :---------------- | ------------------------------------------------------------ |
| symbol | String | Trade Pair Name |
asks | Array Array <Array> | Sell depth array, each element of the array is an array, consisting of price and quantity, arranged from low to high price |
| bids | Array Array <Array> | Buy depth array, each element of the array is an array, consisting of price and quantity, arranged from highest to lowest price |

 ** Remarks **

-For trading pairs in the request parameters, use-instead of /, for example, BTC-USDT instead of BTC / USDT
-In order to prevent loss of precision, both price and quantity are of type string



## Transaction History

** Request method and request URL: **
-GET `/v1/public/spot/trades`

** Parameter: **

| Parameter name | Type | Required | Description |
| :----- | :----- | :------- | -------------

** Request example **

```
GET /v1/public/spot/trades?s=BTC-ETH
```

** Return example **

```
{
    "code": "000000",
    "data": [
{
        "t": 1502962946216, // Unix timestamp in milliseconds
        "s": "ETH / BTC", // symbol
        "d": "buy", // direction of the trade, 'buy' or 'sell'
        "p": "0.06917684", // float price in quote currency
        "v": "1.5", // amount of base currency
        "to": "15.12", // amount of base currency
},
]
    "msg": "OK"
}
```

 ** Return value description **

| Field name | Type | Description |
| :----- | :----- | --------------------- |
| t | long | Transaction timestamp, in milliseconds |
| s | string | trading pairs |
| d | string | Buying and selling direction, buy or sell |
| p | string | Transaction price |
| v | string | Volume in units of base |
| to | string | Turnover, in quotes |


 ** Remarks **

-No



## K Line

**A brief description:** 

-Obtain trading pair K line data, the opening time of each K line can be regarded as a unique ID

** Request method and request URL: **
-GET `/v1/public/spot/kline`

** Parameter: **

| Parameter name | Type | Required | Description |
| :----- | :----- | :------- | ------------------------------------------------------------ |
| s | string | yes | trading pair name, / replaced with-|
| p | string | Yes | Time interval (minute system: 1m, 5m, 10m, 15m, 30m. hour system: 1H, 4H, 12H, day system: 1D, week: 1W, month system: 1Month) |
| l | int | No | Return the number of data, the default is 500, the maximum is 1000 |

** Request example **
```
GET /v1/public/spot/kline?s=BTC-ETH&p=1m
```

** Return example **

```
{
    "code": "000000",
    "data": [
[
1566839160000, // opening time
"0.01634790", // opening price
"0.80000000", // highest price
"0.01575800", // lowest price
"0.01577100", // closing price (the current price is not the end of the K line is the latest price)
"148976.11427815", // Volume (in base)
"2434.19055334", // turnover (in quotes)
308, // Number of transactions
"2019-08-27 01:06:00"
],
]
    "msg": "OK"
}
```

 ** Return value description **


| Array Elements | Type | Description |
| :------- | :----- | --------------------------------- |
| 0 | long | Opening time in milliseconds |
| 1 | string | Opening price |
| 2 | string | Highest price |
| 3 | string | Lowest price |
| 4 | string | Closing price (the current price is the end of the latest price) |
| 5 | string | Volume (in base units) |
| 6 | string | Turnover (in quotes) |
| 7 | int | Number of transactions |
| 8 | string | time |

 ** Remarks **

-No



# Private interface

Private interfaces include:

* Order interface
* Cancel order interface
* Query order list interface
* Query order details interface
* Query asset interface



The private interface must contain the following parameters:

* accessKey
* timestamp, current timestamp in milliseconds
* sign, the signature of the request parameters



## Order Status

* TRADING
* CANCCELING canceling
* CANCELED cancelled successfully
* COMPLETED transaction completed

## Commission price type
Currently only supports limit orders, LIMIT



## Return code and error code

When the interface is successfully called, "000000" is returned; the rest are returned as failure status, and the error codes are as follows:

| Error Code | Description |
| :---------- | :-------------------- |
| ORDERADD001 | Enter the order amount less than or equal to 0 |
| ORDERADD002 | Order quantity is less than or equal to 0 |
| ORDERADD003 | Trading pair does not exist |



## Signature

All private interfaces need to verify the user's parameters, the verification method is as follows:
1. If it is the GET method, what is involved in the signature calculation? After all the parameters, after sorting, as the signature parameters;
2. If it is POST method, use the requested json structure as the signature parameter, convert it to String, and sort it as the signature parameter
3. The parameter must have accessKey and timestamp; accessKey is the user's API accessKey, timestamp is the current timestamp, and the unit is milliseconds;
4. The signature algorithm is as follows:
a. Sort all parameters (not including sign parameter) according to ascii characters from small to large, and use standard URL Encode encoding for the parameter value, and then use & to connect as a payload;
b. Use secretKey as the key;
c. Use HMACsha256 algorithm to calculate the signature;
d. Convert the signature result to a hexadecimal string;
e. Use the result of the previous step as the sign parameter

Examples of the signature process are as follows:
1. Assume the following parameters:
accessKey = ce2a18e0-dshs-4c44-4515-9aca67dd706e
timestamp = 1566461351656
price = 10000.00
amount = 0.01
symbol = BTC-USDT
side = BUY
priceType = LIMIT

After sorting, the resulting string is as follows:
```
accessKey = ce2a18e0-dshs-4c44-4515-9aca67dd706e & amount = 0.01 & price = 10000.00 & priceType = LIMIT & side = BUY & symbol = BTC-USDT & timestamp = 1566461351656
```

Assuming secretKey = c11a122s-dshs-shsa-4515-954a67dd706e, the final calculated sign is:
```
xxx
```

The resulting param parameters are:
```
accessKey = ce2a18e0-dshs-4c44-4515-9aca67dd706e & amount = 0.01 & price = 10000.00 & priceType = LIMIT & side = BUY & symbol = BTC-USDT & timestamp = 1566461351656 & sign = xxx
```





## Order interface

**A brief description:** 

-Order interface

** Request URL: **
-`/v1/trade/spot/add`

** Request method: **
-POST

** Parameter: **

| Parameter name | Required | Type | Description |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | yes | string | accessKey |
| price | yes | string | price |
| amount | yes | string | commission amount |
| priceType | no | string | commission type, currently only supports limit orders LIMIT |
| symbol | yes | string | trading pair, / replace with-, for example BTC-USDT |
direction | yes | string | buying and selling direction Buy: BUY; Sell: SELL |
| no | yes | string | request no, if it does, the server returns the same parameters |
| sign | yes | string | signature |

 ** Return example **

```
  {
    "code": "000000",
    "data": {
      "orderId": "EBLxxxxxxxxxxx"
    },
"no": ""
  }
```

 ** Return parameter description **

| Parameter name | Type | Description |
| :------ | :----- | ---------- |
| orderId | string | order number |
| no | string | request no |

** Remarks **

-For more error codes, please see the error code description on the homepage
-Use string type for price and amount. If decimal type is used, precision may be lost and sign value will be inconsistent!



## Cancel order interface

** Request URL: **
-`/v1/trade/spot/cancel`

** Request method: **
-POST

**parameter:**

| Parameter name | Required | Type | Description |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | yes | string | accessKey |
| orderId | yes | string | price |
| no | yes | string | request no, if it does, the server returns the same parameters |
| symbol | yes | string | trading pair, / replace with-, for example BTC-USDT |
| sign | yes | string | signature |

 ** Return example **

```
  {
    "code": "000000"
"data": {
"orderId": ""
},
"no": ""
  }
```

 ** Return parameter description **

| Parameter name | Type | Description |
| :------ | :----- | ---------- |
| orderId | string | order number |
| no | string | request no |

 ** Remarks **

-This interface is an asynchronous interface. The success of the server does not mean that the order was cancelled successfully. The user must call the order details to obtain the order status to determine whether the order was successfully cancelled.



## Query single order details interface

** Request URL: **
-`/v1/trade/spot/detail`

** Request method: **
-GET / POST, POST method is recommended

**parameter:**

| Parameter name | Required | Type | Description |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | yes | string | accessKey |
| orderId | yes | string | price |
| no | yes | string | request no, if it does, the server returns the same parameters |
| symbol | yes | string | trading pair, / replace with-, for example BTC-USDT |
| sign | yes | string | signature |

 ** Return example **

```
  {
    "code": "000000"
     "data": {
        "amount": 0,
        "baseSymbol": "string",
        "coinSymbol": "string",
        "completedTime": 0,
        "createTime": "2019-08-22T11: 31: 47.453Z",
        "directionStr": "string",
        "dk": 0,
        "fee": 0,
        "memberId": 0,
        "orderId": "string",
        "price": 0,
        "statusStr": "string",
        "symbol": "string",
        "time": 0,
        "tradedAmount": 0,
        "tradedAvgPrice": 0,
        "turnover": 0,
        "typeStr": "string"
"details": [{
"tradedAmount": 0,
"dk": 0,
"fee": 0,
"price": 0,
"time": 0,
"turnover": 0
}]
},
"no": ""
  }
```

 ** Return parameter description **

| Field name | Type | Description |
| :------------- | :------ | ----------------------------------------- |
| amount | string | commission amount |
| baseSymbol | string | target currency, the name after the trading pair "/" |
| coinSymbol | string | trading currency, name before trading pair "/" |
| completedTime | string | completion time, only has value when order is in the completed state |
| createTime | string | order creation time |
| directionStr | string | Buying and selling direction: BUY, SELL |
| dk | decimal | point card use |
| fee | string | transaction commission |
| memberId | long | user id |
| orderId | string | order id |
| price | string | Order pending price |
| statusStr | string | order status |
| symbol | string | order trading pair name |
| time | string | Transaction timestamp in milliseconds |
| tradedAmount | string | Number of deals |
| tradedAvgPrice | string | average transaction price |
| turnover | string | turnover |
| typeStr | string | Price type, currently only limited price type LIMIT_PRICE |


details is the transaction that the order has been completed, and the fields in the structure are described as follows:

| Field name | Type | Description |
| :------------- | :------ | -------------------- |
| dk | decimal | point card use |
| fee | string | transaction commission |
| memberId | long | user id |
| orderId | string | order id |
| price | string | Order pending price |
| symbol | string | order trading pair name |
| time | string | Transaction timestamp in milliseconds |
| tradedAmount | string | Number of deals |
| tradedAvgPrice | string | average transaction price |

## Get order interface based on status

** Request URL: **

-`/v1/trade/spot/listOrders`

** Request method: **

-GET / POST, POST method is recommended

**parameter:**

| Parameter name | Required | Type | Description |
| :-------- | :--- | :----- | ---------------------------------------------- |
| accessKey | yes | string | accessKey |
| status | yes | string | see description below |
| symbol | no | string | trading pairs, if this parameter is absent, all trading pairs are returned |
| pageNum | No | int | Pagination, page number starts from 1 |
| pageSize | No | int | Number returned per page, default 10, maximum 100 |
| startTime | No | long | millisecond timestamp, query orders with order creation time later than this time |
| endTime | no | long | millisecond timestamp, query orders with order creation time earlier than this time |
| no | yes | string | request no, if it does, the server returns the same parameters |
| sign | yes | string | signature |

** status parameter **
The status parameter values ​​are as follows:

| Value | Description

** status parameter **
The status parameter values ​​are as follows:

| Value | Description |
| :-------- | :------------------------------- |
| TRADING | Orders in the transaction, including partially completed orders |
| COMPLETED | Transaction completed order |
| CANCELED | Cancelled orders |
| CANCELING | Cancelling order |

 ** Return example **

```
  {
    "code": "000000"
"data": {
"pages": 12,
"total": 112,
"list": [{
        "amount": 0,
        "baseSymbol": "string",
        "coinSymbol": "string",
        "completedTime": 0,
        "createTime": "2019-08-22T11: 31: 47.453Z",
        "directionStr": "string",
        "dk": 0,
        "fee": 0,
        "memberId": 0,
        "orderId": "string",
        "price": 0,
        "statusStr": "string",
        "symbol": "string",
        "time": 0,
        "tradedAmount": 0,
        "tradedAvgPrice": 0,
        "turnover": 0,
        "typeStr": "string"
}]
},
"no": ""
  }
```

 ** Return result description **

| Field name | Type | Description |
| :----- | :---- | -------- |
| list | Array | Order list |
| pages | int | Total pages |
| total | int | Total orders |

 ** Return result list array description **

| Field name | Type | Description |
| :------------- | :------ | ----------------------------------------- |
| amount | string | order number |
| baseSymbol | string | target currency, the name after the trading pair "/" |
| coinSymbol | string | trading currency, name before trading pair "/" |
| completedTime | string | completion time, only has value when order is in the completed state |
| createTime | string | order creation time |
| directionStr | string | Buying and selling direction: BUY, SELL |
| dk | decimal | point card use |
| fee | string | transaction commission |
| memberId | long | user id |
| orderId | string | order id |
| price | string | Order pending price |
| statusStr | string | order status |
| symbol | string | order trading pair name |
| time | string | Transaction timestamp in milliseconds |
| tradedAmount | string | Number of deals |
| tradedAvgPrice | string | average transaction price |
| turnover | string | turnover |
| typeStr | string | Price type, currently only limited price type LIMIT_PRICE |

## Get user balance interface

** Request URL: **
-`/v1/trade/spot/balance`

** Request method: **

-GET / POST, POST method is recommended

**parameter:**

| Parameter name | Required | Type | Description |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | yes | string | accessKey |
| no | yes | string | request no, if it does, the server returns the same parameters |
| sign | yes | string | signature |

 ** Return example **

```
  {
    "code": "000000"
"data": {
"BTC": {
"balanceAvailable": "1.0",
        "balanceFrozen": "0.0",
        "balanceOTCFrozen": "0.0"
}
},
"no": ""
  }
```

** Return parameter description **

| Parameter name | Type | Description |
| :--------------- | :----- | ------------------- |
| balanceAvailable | String | Available Quantity |
| balanceFrozen | String | Frozen Quantity |
| balanceOTCFrozen | String | Number of funds frozen in OTC transactions |


# Appendix

## Signature Algorithm



** The JAVA verification signature code is as follows: **



```
public class HMACUtil {
    private static final String signatureMethodValue = "HmacSHA256";
    private static final String signKey = "sign";

    static public String toStringByKey (Map <String, Object> json) throws Exception {
        Map <String, String> map = new TreeMap <> ();
        for (String key: json.keySet ()) {
            if (key.equals (signKey)) {
                continue;
            }
            map.put (key, json.get (key) .toString ());
        }

        StringBuilder sb = new StringBuilder (1024);
        for (Map.Entry <String, String> entry: map.entrySet ()) {
            if (! (""). equals (sb.toString ())) {
                sb.append ("&");
            }

            sb.append (entry.getKey ());
            sb.append ("=");
            sb.append (urlEncode ((String) entry.getValue ()));
        }

        return sb.toString ();
    }

    static public String hmacSign (JSONObject json, String secretKey) throws Exception {
        String payload = toStringByKey (json);
        return hmacSign (payload, secretKey);
    }

    / **
     * Use standard URL Encode encoding. Note that unlike the default JDK, spaces are encoded as% 20 instead of +.
     *
     * @param s String
     * @return URL encoded string
     * /
    private static String urlEncode (String s) throws Exception {
        try {
            return URLEncoder.encode (s, "UTF-8"). replaceAll ("\\ +", "% 20");
        } catch (UnsupportedEncodingException e) {
            throw new Exception ("[URL] UTF-8 encoding not supported!");
        }
    }

    static public String hmacSign (String payload, String secretKey) throws Exception {
        Mac hmacSha256;

        hmacSha256 = Mac.getInstance (signatureMethodValue);
        SecretKeySpec secKey = new SecretKeySpec (secretKey.getBytes (StandardCharsets.UTF_8),
                signatureMethodValue);
        hmacSha256.init (secKey);

        byte [] hash = hmacSha256.doFinal (payload.getBytes (StandardCharsets.UTF_8));

        return Hex.encodeHexString (hash);
    }
}
```





** Python calculation signature code is as follows: **



```
import hmac
import hashlib

# msg is the sorted string
def hmacsha256 (msg, secret):
    hash = hmac.new (secret, msg = msg, digestmod = hashlib.sha256) .hexdigest ()
    return hash
```

** The JavaScript calculation signature code is as follows: **



```
const CryptoJS = require ("crypto-js")

// Sort parameters
function toStringByKey (param) {
     let result = ''
     let keys = []

     for (let key in param) {
         keys.push (key)
     }
     keys.sort ()
     keys.forEach (key => {
             let val = param [key]
             result + = key + '='
             if (typeof val === 'string') {
                 result + = encodeURIComponent (val)
             } else if (typeof val === 'number') {
                 result + = encodeURIComponent (+ val)
             }
         result + = '&'
     })

     return result.slice (0, result.length-1)
}

// payload is the sorted string
function sign (payload, secret) {
let hash = CryptoJS.HmacSHA256 (payload, secret)
// return CryptoJS.enc.Base64.stringify (hash)
return CryptoJS.enc.Hex.stringify (hash)
}
```

