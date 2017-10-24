# 2 接口列表

## 2.1 发起支付

### HTTP 请求

`POST  https://pay.netease.com/api/charges`

### 请求参数

参数 | 类型 | 必选 | 说明 | 示例值
--------- | ------- | ------- | -------- | ----------
channel | String&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 是&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 渠道代码，详见[渠道列表](#3-2) | WX_NATIVE
outTradeNo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | String | 是 | 商户订单号 8到32位数字和/或字母组合，请自行确保在商户系统中唯一，同一订单号不可重复提交，否则会造成订单重复 | 7965470787672540
feeType | String | 否 | 金额类型 | CNY 
totalFee | Integer | 是 | 订单总金额 单位为分 |  1
clientIp | String | 是 | 发起支付请求客户端的 IP 地址，扫码支付填服务器IP | 127.0.0.1
subject | String | 是 | 商品的标题 | 测试商品
body | String | 是 | 商品的描述信息 | 商品xxxxx
created | Long | 是 | 下单时间， 时间戳 逝去的毫秒数 | 1508401409765
timeExpire | Integer | 否 | 订单失效时间 单位秒 默认 1天 | 86400
analysis | String | 否 | 分析数据 包括以下基本字段：<br/>os_name(系统名称，如"iOS"，"Android")<br/> os_version(系统版本，如"5.1") <br/>model(手机型号，如"iPhone 6") <br/>app_name(应用名称) <br/>app_version(应用版本号) <br/>device_id(设备ID) <br/>category(类别，用户可自定义，如游戏分发渠道，门店ID等)<br/> browser_name(浏览器名称) <br/>browser_version(浏览器版本) | 
attach | String | 否 | 附加数据	用户自定义的参数，将会在webhook通知中原样返回，该字段主要用于商户携带订单的自定义数据 | 
description | String | 否 | 订单附加说明 | 
returnUrl | String | 否 | 同步返回页面。支付渠道处理完请求后,当前页面自动跳转到商户网站里指定页面的http路径 | 
wx.openid | String | 否 | 微信支付时使用，channel=JSAPI时（即公众号支付），此参数必传，此参数为微信用户在商户对应appid下的唯一标识。 | oUpF8uMuAJO_M2pxb1Q9zNjWeS6o

> 发起支付接口调用示例

```shell
curl -H "key: e3327bcd-2681-4a2a-a321-c75ekl433ff81" \
     -H "sign: e33ewwewrd90f8ds098f09werf0w9fiesfds" \
     -d "channel=WX_NATIVE&outTradeNo=test903289329&...." \
     "https://https://pay.netease.com/api/charges"
```

```java
// 微信扫码支付调用代码示例
final PayCenterClient client = new PayCenterClient("e33l43cd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<WxNativeParams> payInfo = client.wxNativePay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "");
WxNativeParams params = payInfo.getChannelParams();

```

```java
// 微信APP支付调用代码示例
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<WxAppParams> payInfo = client.wxAppPay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "");
WxAppParams channelParams = payInfo.getChannelParams();

```

```java
// 微信公众号支付调用代码示例
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<WxJsapiParams> payInfo = client.wxJsapiPay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "", "wx_lkjDODSF0DS0FWELFDSLJFS");
WxJsapiParams channelParams = payInfo.getChannelParams();

```

```java
// 微信H5支付调用代码示例
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<WxMwebParams> payInfo = client.wxMwebPay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "");
WxMwebParams channelParams = payInfo.getChannelParams();

```

```java
// 支付宝电脑网站支付调用代码示例
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<AliParams> payInfo = client.aliWebPay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "");
AliParams channelParams = payInfo.getChannelParams();

```

```java
// 支付宝APP支付调用代码示例
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<AliParams> payInfo = client.aliAppPay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "");
AliParams channelParams = payInfo.getChannelParams();

```

```java
// 支付宝手机网站支付调用代码示例
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayInfo<AliParams> payInfo = client.aliWapPay("test903289329", 100, "127.0.0.1", "超级大礼包",
        "xxxxx", System.currentTimeMillis(), "http://www.163.com", 7200, "",
        "", "");
AliParams channelParams = payInfo.getChannelParams();

```

### 返回字段说明

`公共返回字段`

参数 | 类型 | 说明
--------- | ------- | ------- 
channel | String | 渠道类型
channelParams | Map | 支付参数集合

`支付参数集合字段 - 微信扫码支付`

参数 | 类型 | 说明
--------- | ------- | ------- 
codeUrl | String | 支付二维码地址URL

> 返回的JSON示例:

```json
{
	"requestId":1508401457368,
	"responseParams":
	{
		"channelParams":
		{
			"codeUrl":"weixin://wxpay/bizpayurl?pr=kzQ1mz8"
		},
		"channel":"WX_NATIVE"
	}
}
```

`支付参数集合字段 - 微信公众号支付`

参数 | 类型 | 说明
--------- | ------- | ------- 
appId | String | 商户注册具有支付权限的公众号成功后即可获得
noncestr | String | 随机字符串，长度要求在32位以内。
timestamp | String | 当前时间戳
package | String | 统一下单接口返回的prepay_id参数值，提交格式如：prepay_id=***
signType | String | 签名算法，暂支持MD5
paySign | String | 签名

> 微信公众号支付返回的JSON示例:

```json
{
	"requestId":1507803453595,
	"responseParams":
	{
		"channelParams":
		{
			"timeStamp":"1507803453",
			"package":"prepay_id=wx20171012181733554696221d0025555660",
			"paySign":"D939DF512ECCBE02E90AB61A50321D5C",
			"appId":"wx1f343894d9cce45d",
			"signType":"MD5",
			"nonceStr":"5e06ee178774417a8b2154307a438516"
		},
		"channel":"WX_JSAPI"
	}
}
```

`支付参数集合字段 - 微信APP支付`

参数 | 类型 | 说明
--------- | ------- | ------- 
appId | String | 商户注册具有支付权限的公众号成功后即可获得
partnerid | String | 微信支付分配的商户号
noncestr | String | 随机字符串，长度要求在32位以内。
timestamp | String | 当前时间戳
package | String | Sign=WXPay
prepayid | String | 微信生成的预支付回话标识，用于后续接口调用中使用，该值有效期为2小时,针对H5支付此参数无特殊用途
paySign | String | 签名

> 微信APP支付返回的JSON示例:

```json
{
	"requestId":1507803453595,
	"responseParams":
	{
		"channelParams":
		{
			"timeStamp":"1507803453",
			"partnerid":"1230000109",
			"package":"Sign=WXPay",
			"sign":"D939DF512ECCBE02E90AB61A50321D5C",
			"appId":"wx1f343894d9cce45d",
			"prepayid":"wx20171012181733554696221d0025555660",
			"nonceStr":"5e06ee178774417a8b2154307a438516"
		},
		"channel":"WX_APP"
	}
}
```

`支付参数集合字段 - 微信H5支付`

参数 | 类型 | 说明
--------- | ------- | ------- 
mwebUrl | String | mweb_url为拉起微信支付收银台的中间页面，可通过访问该url来拉起微信客户端，完成支付,mweb_url的有效期为5分钟。

> 微信H5支付返回的JSON示例:

```json
{
	"requestId":1508395766842,
	"responseParams":
	{
		"channelParams":
		{
			"mwebUrl":"https://wx.tenpay.com/cgi-bin/mmpayweb-bin/checkmweb?prepay_id=wx20171019144926f5b082a1810102462161&package=1707153875"
		},
		"channel":"WX_MWEB"
	}
}
```

`支付参数集合字段 - 支付宝支付`

参数 | 类型 | 说明
--------- | ------- | ------- 
body | String | 支付宝支付页面表单body，含相关支付信息

> 支付宝支付返回的JSON示例:

```json
{
	"requestId":1508401409765,
	"responseParams":
	{
		"channelParams":
		{
			"body":"<form name=\"punchout_form\" method=\"post\" action=\"https://openapi.alipay.com/gateway.do?charset=utf-8&method=alipay.trade.page.pay&sign=GWU3Azwrdp3DaSSFSMCU9wrlvfddah%2BiI2lSXCpman84xFh9UT4yky3b1aa%2F07dvh%2BsZDTfy3ktytOuabUENsMkJFlyX626UqIc8MnRnMQbV%2Fy65KvlSjIfCwtpgsTOseQDjX9FXnElcWd9LqSCAgNSpzwCfaVhRahyhs7WkI2BC%2FEHs8K4qL3CSXeuzXNrmAByqAB0Rwv5FT0nkgvEcBg%2FgFSnw%2FkY%2BBFKPa9IDFTjN90%2FB4sEndBIItFreTukKi9kECz2YKprepjgLnpnqhh04e5IiqIAdDOB5EuNmSu8JmcnotTiFkXsJuqKy7ZpCj9SG28aIhxbkCCofV8nEpQ%3D%3D&return_url=http%3A%2F%2F163.com&notify_url=https%3A%2F%2Fpay.netease.com%2Fcallbacks%2F11%2Falipay%2Fweb%2Fpaynotify&version=1.0&app_id=2016112103063721&sign_type=RSA2&timestamp=2017-10-19+16%3A23%3A29&alipay_sdk=alipay-sdk-java-dynamicVersionNo&format=json\">\n<input type=\"hidden\" name=\"biz_content\" value=\"{&quot;body&quot;:&quot;product 1 * 1 product 2 * 1&quot;,&quot;out_trade_no&quot;:&quot;9769969905246398&quot;,&quot;passback_params&quot;:&quot;&quot;,&quot;product_code&quot;:&quot;FAST_INSTANT_TRADE_PAY&quot;,&quot;subject&quot;:&quot;test-976&quot;,&quot;timeout_express&quot;:&quot;40m&quot;,&quot;total_amount&quot;:&quot;0.01&quot;}\">\n<input type=\"submit\" value=\"立即支付\" style=\"display:none\" >\n</form>\n<script>document.forms[0].submit();</script>"
		},
		"channel":"ALI_WEB"
	}
}
```

## 2.2 支付查询

### HTTP 请求

`GET  https://pay.netease.com/api/charges/pay-result`

### 请求参数

参数 | 类型 | 必选 | 说明 | 示例值
--------- | ------- | ------- | -------- | ----------
channel | String&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 是&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 渠道代码，详见[渠道列表](#3-2) |  WX_NATIVE
outTradeNo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | String | 是 | 商户订单号 8到32位数字和/或字母组合，请自行确保在商户系统中唯一，同一订单号不可重复提交，否则会造成订单重复 | 7965470787672540

> 支付查询接口调用代码示例：

```shell
curl -H "key: e3327bcd-2681-4a2a-a321-c75ekl433ff81" \
     -H "sign: e33ewwewrd90f8ds098f09werf0w9fiesfds" \
     "https://pay.netease.com/api/charges/pay-result?channel=WX_NATIVE&outTradeNo=test903289329"
```

```java
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
PayResult payResult = client.getPayResult(Channel.WX_NATIVE, "test903289329");
System.out.println(payResult.getHasPay());
```

### 返回字段说明

参数 | 类型 | 说明
--------- | ------- | ------- 
channel | String&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 渠道类型
outTradeNo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | String | 商户订单号
hasPay | Boolean | 是否已支付
payTime | Date | 支付时间，未支付为NULL
returnUrl | String | 回跳URL，未支付为NULL
attach | String | 附加数据，在查询API和支付通知中原样返回，该字段主要用于商户携带订单的自定义数据

> 返回的JSON示例:

```json
{
	"requestId":1508401467147,
	"responseParams":
	{
		"attach":"",
		"channel":"WX_NATIVE",
		"hasPay":true,
		"outTradeNo":"9769969905246398",
		"payTime":1508401466000,
		"returnUrl":"http://163.com"
	}
}
```

## 2.3 发起退款

### HTTP 请求

`POST  https://pay.netease.com/api/refunds`

### 请求参数

参数 | 类型 | 必选 | 说明 | 示例值
--------- | ------- | ------- | -------- | ----------
outTradeNo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | String | 是&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 商户订单号 8到32位数字和/或字母组合，请自行确保在商户系统中唯一，同一订单号不可重复提交，否则会造成订单重复 | 7965470787672540
outRefundNo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | String | 是&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 商户系统内部的退款单号，商户系统内部唯一，只能是数字、大小写字母，同一退款单号多次请求只退一笔。 | 1217752501201407
refundFee | Integer | 是 | 退款总金额，订单总金额，单位为分，只能为整数 | 1
refundDesc | String | 否 | 退款原因，若商户传入，会在下发给用户的退款消息中体现退款原因 | 商品退货

> 发起退款接口调用代码示例：

```shell
curl -H "key: e3327bcd-2681-4a2a-a321-c75ekl433ff81" \
     -H "sign: e33ewwewrd90f8ds098f09werf0w9fiesfds" \
     -d "outTradeNo=test903289329&outRefundNo=ref43534534534&refundFee=100" \
     "https://pay.netease.com/api/refunds"
```

```java
final PayCenterClient client = new PayCenterClient("e3327bcd-2681-4a2a-a321-c75ekl433ff81",
                "52ewc-c675-4944-af44-f4fdsww332d88");
client.refund("test903289329", "ref43534534534", 100);
```

### 返回字段说明

参数 | 类型 | 说明
--------- | ------- | ------- 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

```json
{
	"requestId":1508401467147
}
```