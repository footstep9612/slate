---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python
  - php
  - java
  - csharp

toc_footers:
  - <a href='https://www.aichotels.com'>Click here to  visit AIC hotels website</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# History

| Author     | Version | Date        | Description     |
| ---------- | ------- | ----------- | --------------- |
| Kyle Shang | 1.0.0   | 08 Aug 2017 | Initial Edition |
| Kyle Shang | 1.0.1   | 02 Oct 2017 | Seperating Static API and Dynamic API |
| Kyle Shang | 1.1.0   | 20 Nov 2017 | Adding PreBook Function and boradtype info |
| Kyle Shang | 1.1.1   | 02 Jan 2018 | Adding BookingSearch Function and Special Request |
| Kyle Shang | 1.2.0   | 11 Jan 2018 | Internal algrithm update |
| Kyle Shang | 1.2.1   | 23 Feb 2018 | Adding detailed pax Info |
| Kyle Shang | 2.0.0   | 11 Jun 2018 | Adding more Static API i.e. Country, Citr etc. |
| Kyle Shang | 2.0.1   | 03 Dec 2018 | Change the Endpoint |
| Kyle Shang | 2.1.1   | 01 Feb 2019 | Add Cache API and Price Change API |
| Kyle Shang | 3.0.0   | 15 Feb 2019 | Change the Endpoint |
| Kyle Shang | 3.0.1   | 21 Feb 2019 | Add MultiHotelSearch API |
| Justin Zhang | 3.1.0   | 07 Sep 2019 | Transit to Online doc and Re-write the doc;  Adding Eng ver;  Adding Python Script for Authentication |


# Introduction

The **AIC Open API** is a RESTful based API that can be used by any distributors to perform business functions with Amerilink.

There are two set of environments been provided  **Test Environment** and **Live Environment**. In each of the environment, the APIs are categorized into two part based on the functionality:

- **Content API :** Obtaining the static data such as country name, hotel name, hotel address etc.
- **Rate API :** Essential APIs to perform the full cycle of business functions from Search to Order Creation to Cancel.

**Test**:

| Type   | Endpoint |
| ------ | -------- |
| Content |  https://content-uat.aichotels.net.cn/content/public      |
| Rate | https://api-uat.aichotels.net.cn/rate/public      |

**Live**:

| Type   | Endpoint |
| ------ | -------- |
| Content |  https://content.aichotels.net.cn/content/public      |
| Rate | https://api.aichotels.net.cn/rate/public      |

# Authentication

>HTTP Header Sample

```json
{
    "APIClientKey":"TestClientKey",
    "Date":"Thu,11 Jan 2018 03:55:10 UTC",
    "APIClientToken":"uh3ap5Yq1iBId49o/3KdyTdzoDs=",
    "content_type":"application/json"
}
```

> Below are the code sample for Auth

```python
"""
Below Python script used native Python Module:
hmac, base64, and datetime.
Below Script are only for demostration purpose
Might not suitable for Production Enviroment
"""
import hmac, base46, datetime

#Global Variable
APIClinetKey = "Key provided by Amerilink"
ClientSecret = b"Secret provided by Amerilink"
bin_Method = b"POST"
bin_reqURL = b"/rate/public/ping"
bin_reqURI = b"http://api-uat.aichotels.net.cn"

# UTC Time formatting
UtcTime = datetime.datetime.utcnow()
Date =  UtcTime.strftime ("%a,%d %b %Y %H:%M:%S UTC")

# Signature Calculation

Header_Date = Date.encoding('utf-8')
# String concatenation
strsign = (bin_Method + b" "+ bin_reqURL+ b"\n" + Header_Date)
# Encyrpt use hmac-sha1 then encrept the digested value use base64 and then trasfer to string.
signature = str(base64.encodebytes(hmac.new(ClientSecret,strsign,sha1).digest()).decode('utf-8').rstrip('\n'))


Header = {
    'APIClientKey':APIClinetKey,
    'Date':Date,
    'APIClientToken':signature,
}

```

```php
<?php
function enctyption($method, $reqUrl, $date, $sercret)
{
 //String concatenation
 $stringToSign = $method. " ". $reqUrl. "\n". ($date);
 //use sha1 algorithm to generate hash string
 $signature = hash_hmac('sha1', $stringToSign, $sercret, true);
 // base64 encode
 return base64_encode($signature);

```

```java
package xxx;
import org.apache.commons.codec.binary.Base64;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.io.UnsupportedEncodingException;
class Encryption
{
 public static String generateSignature(String method, String reqUrl, String date,
String secret)
 {
 StringBuilder sign = new StringBuilder();
 sign.append(method);
 sign.append(" ");
 sign.append(reqUrl);
 sign.append("\n");
 sign.append(date);
 byte[] sha1 = hmac_sha1(sign.toString(), secret);
 String signature;
 try
 {
 signature = new String(Base64.encodeBase64(sha1), "UTF-8");
 return signature;
 }
 catch (UnsupportedEncodingException e)
 {
 return "";
 }
 }
 private static byte[] hmac_sha1(String value, String key)
 {
 try
 {
 byte[] keyBytes = key.getBytes();
 SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA1");
 Mac mac = Mac.getInstance("HmacSHA1");
 mac.init(signingKey);
 return mac.doFinal(value.getBytes());
 }
 catch (Exception e)
 {
 return null;
 }
 }
}

```

```csharp
using System;
using System.Text;
using System.Security.Cryptography;
using System.IO;
class AuthValidatorUtil
{
 public string generateSignature(string method, string reqUrl, string date, string
secret)
 {
 StringBuilder sign = new StringBuilder();
 sign.Append(method);
 sign.Append(" ");
 sign.Append(reqUrl);
 sign.Append("\n");
 sign.Append(date);
 byte[] authorization = getSignature(Encoding.UTF8.GetBytes(sign.ToString()),
Encoding.UTF8.GetBytes(secret));
 return Convert.ToBase64String(authorization);
 }
 public static byte[] getSignature(byte[] data, byte[] key)
 {
 HMACSHA1 hmac = new HMACSHA1(key);
 CryptoStream cs = new CryptoStream(Stream.Null, hmac, CryptoStreamMode.Write);
 cs.Write(data, 0, data.Length);
 cs.Close();
 return hmac.Hash;
 }
}
```



All Authentication Info needs to be included in the **Header** section when a HTTP request are being posted. There are Three Components in which needs to be included: *APIClientKey*, *Date*, *APIClientToken*. 

Since all API are in JSON format, the `Content-Type `will always be `application/json`

<aside class="warning">Please be aware of the format of the UCT timestamps, especially the white spaces</aside>
| HeaderName   | Description           |
| ------------ | --------------------- |
| APIClientKey | Provided by Amerilink |
|Date| UTC Based timestamps |
|APIClientToken| Calculated Based on Hmac-Sha1 alone with the Secret been provided by Amerilink |

# Ping

`POST https://{endpoint}/rate/public/ping`

A simple way to validate if the authentication had passed and network is okay.

## Request

> Request Message

```json
{
"message" : "Hello World"
}
```
| Parameter | Count | Type   | Description     |
| --------- | ----- | ------ | --------------- |
| message   | 1     | String | Testing Message |

## Response

> Response Message

```json
{
 "result": {
 "return_status": {
 	"success": "true",
 	"exception": ""
 }
 },
 "message": "Hello World"
}
```

| Parameter | Count | Type   | Description |
| --------- | ----- | ------ | ----------- |
| result    | 1     | Object |             |
|  ....return_status| 1| Object|             |
|  ....success| 1| Boolean| Status of the request|
|  ....exception| 1| Boolean|Error message in case failed|
|  ....message| 1| Boolean| Echo of the request message|

# Content API

Static API are used to obtain the static date such as Country Info, City Info, HotelName, Address, RoomName  etc.

## Country list

Country list API is used to obtain a full list of country name alone with the unique Amerilink `Country Code`and `Country Id`.

### Reuqest

`GET https://{endpoint}/content/public/country_list`

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
 "country_list": [
 {
 "country_id": "1",
 "country_code": "AD",
 "name": "Andorra"
 },
 {
 "country_id": "2",
     "country_code": "AE",
 "name": "United Arab Emirates"
 },
 {
 "country_id": "4",
 "country_code": "AG",
 "name": "Antigua and Barbuda"
 },
 ]
}

```

| Parameter    | Count | Type   | Description |
| ------------ | ----- | ------ | ----------- |
| country_list | 1     | Array |             |
| ....country_id | 1     | String |      Unique Amerilink Country id       |
| ....country_code | 1| String | Country Code in ISO Alpha-2 Standard|
| ....name | 1| String | Corresponding Country Name|

## City List
Obtaining a list of city id alone with corresponding information by using the unique country id obtained from `Country List` API

### Request
`GET https://{Endpoint}/content/public/city_list/{country_id}`

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


| Parameter    | Count | Type   | Description |
| ------------ | ----- | ------ | ----------- |
| city_list | 1     | Array |             |
| ....city_id | 1     | String |      Unique Amerilink City Id       |
| ....name | 1| String | Corresponding City Name|
| ....areaname | 1| String | Province/State Name of which the City Belongs|
| ....center_latitude | 1| String | Latitude of City Center|
| ....center_longitude | 1| String | Longitude of City Center|
<aside class="notice">The <code>Latitude</code> and <code>Longitude</code> are based on the coordintator of Google map </aside>

