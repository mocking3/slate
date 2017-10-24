# 1 使用方法

## 1.1 请求格式

`[GET|POST|UPDATE|DELETE] https://pay.netease.com/api/{resource_url}?{query_string}`

说明：
其中，resource_url为资源地址，query_string由通用参数部分和具体Open API调用方法和参数部分组成。目前所有的提交类接口仅支持POST方式，查询类接口同时支持POST方式和GET方式。

## 1.2 通用参数

参数 | 类型 | 必选 | 说明 | 示例值
--------- | --------- | --------- | --------- | ---------
key | String | 是 | 支付中心应用Key，可以在管理网站上查询到 | e3047bcd-2w81-4a2a-a321-c75e64329381
sign | String | 是 | 签名，详见[签名算法](#3-3) | 98383afefbf746f44e4b2b053f601b20
timestamp | Long | 否 | 时间戳，单位ms | 1508739061000
expires | Long | 否 | 有效期，单位ms | 30000
version | String | 否 | 版本 | v1.0



## 1.3 响应格式

* 成功响应
如果与支付中心服务器交互成功且服务正常，则HTTP状态码为200，返回数据存放在HTTP Body中，是一个UTF-8编码的json数据包。

参数 | 类型 | 说明
--------- | --------- | ---------
requestId | Long | 请求ID，由web server生成，返回给用户方便问题追查与定位
responseParams | Map | 由n个包含key和value属性的对象组成。表示API返回的数据内容。**后续接口列出的响应参数均在该字段下**

> 成功响应示例如下所示:

```json
{
     "requestId":12394838223,
     "responseParams":
     {  
       "queueName":"支付中心"             
     }                     
}
```

* 异常响应

如果与支付中心服务器交互成功且服务未正确完成，则HTTP状态码为200，返回的错误信息仍然是存在于HTTP Body中的、UTF-8编码的json数据包。

参数 | 类型 | 说明
--------- | --------- | ---------
requestId | Long | 请求ID，由web server生成，返回给用户方便问题追查与定位
errorCode | Integer | 错误码，详见[错误码列表](#3-1)
errorMsg | String | 错误描述

> 参数错误的响应示例如下所示：

```json
{
   "requestId":12394838223,
   "errorCode":10001,
   "errorMsg":"服务器内部错误"
}
```

## 1.4 http header

参数 | 说明 | 示例值
--------- | --------- | ---------
User-Agent | 无特殊要求 | curl/7.12.1 (x86_64-redhat-linux-gnu) libcurl/7.12.1 OpenSSL/0.9.7a zlib/1.2.1.2 libidn/0.5.6
Host | 必须为 pay.netease.com | pay.netease.com
Pragma | 无特殊要求 | no-cache
Content-Length  | 无特殊要求，自动生成 | 123
Content-Type | POST 时传 application/x-www-form-urlencoded | application/x-www-form-urlencoded

