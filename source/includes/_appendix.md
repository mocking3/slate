# 3 附录
## 3.1 错误码列表

错误码 | 错误说明
---------- | -------
10001 | 服务器内部错误
10002 | 错误的请求方式（GET/POST）
10003 | 无效的APP KEY
10004 | 签名校验错误
10005 | 请求已过期
10006 | 请求参数缺失
10007 | 存在不合法的请求参数
10008 | 资源不存在
10009 | 数据超过阈值
20001 | 暂不支持该渠道
20002 | 余额不足
20003 | 订单已存在
30001 | 微信统一下单异常
30002 | 微信查询订单异常
30003 | 微信申请退款异常
30004 | 微信支付结果通知异常
30101 | 支付宝统一下单异常
30102 | 支付宝查询订单异常
30103 | 支付宝申请退款异常
30104 | 支付宝支付结果通知异常

## 3.2 渠道列表

渠道代码 | 渠道名称
---------- | -------
WX_NATIVE | 微信扫码支付
WX_APP | 微信APP支付
WX_JSAPI | 微信公众号支付
WX_MWEB | 微信H5支付
ALI_WEB | 支付宝电脑网站支付
ALI_APP | 支付宝APP支付
ALI_WAP | 支付宝手机网站支付

## 3.3 签名算法

支付中心API使用的签名算法如下:

* 获取请求的http method；
* 获取请求的url，不包括query_string的部分；
* 将所有参数（包括GET或POST的参数，但不包含签名字段）格式化为“key=value”格式，如“k1=v1”、“k2=v2”、“k3=v3”；
* 将格式化好的参数键值对以字典序升序排列后，拼接在一起，如“k1：v1，k2 ：v2，k3：v3”，并将http method和url按顺序拼接在这个字符串前面；
* 在拼接好的字符串末尾追加上应用的secretKey，并进行urlencode，形成baseString；

上述字符串的MD5值即为签名的值：

即可简单描述为： `sign = MD5( urlencode( $http_method$url$k1=$v1$k2=$v2$k3=$v3$secret_key ));`

> 签名算法代码示例：

```java

import java.net.URLEncoder;
import java.util.Iterator;
import java.util.TreeMap;

public class SignatureDigest {

    public static String digest(String method, String url, String clientSecret, TreeMap<String, String> params) throws Exception {
        StringBuilder sb = new StringBuilder();
        sb.append(method).append(url);
        Iterator iterator = params.keySet().iterator();

        while (iterator.hasNext()) {
            String key = (String) iterator.next();
            String value = params.get(key) == null ? "" : params.get(key) + "";
            sb.append(key).append('=').append(value);
        }

        sb.append(clientSecret);
        String encodeString = URLEncoder.encode(sb.toString(), "UTF-8");
        if (encodeString != null) {
            encodeString = encodeString.replaceAll("\\*", "%2A");
        }

        byte[] datas = encodeString.getBytes("UTF-8");
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < datas.length; i++) {
            String hex = Integer.toHexString(datas[i] & 0xFF);
            if (hex.length() <= 1) {
                result.append('0');
            }
            result.append(hex);
        }
        return result.toString();
    }
}

```

例如,在需要发送一个POST请求给 /test/echo, 并且参数为:

header 参数

`key:e3047bcd-2w81-4a2a-a321-c75e64329381`

`expires:30000`

`timestamp:1508739061000`

body 参数

`param:test`

则参于签名的字符串：

`POST/test/echoexpires=1313293565key=e3047bcd-2w81-4a2a-a321-c75e64329381param=testtimestamp=1508739061000`

对以上字符串进行urlencode后再计算md5结果,即为本次请求的签名值.
