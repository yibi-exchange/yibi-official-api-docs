# 签名认证

## 合法请求结构
基于安全考虑，除行情API外的 API 请求都必须进行签名运算。一个合法的请求由以下几部分组成：

- 方法请求地址 即访问服务器地址：api.yibi.co 后面跟上方法名，比如api.yibi.co/v1/order/orders。

- API 访问密钥（secret_key） 您申请的API_KEY中的SECRET_KEY。

- 签名方法  用户计算签名的基于哈希的协议，我们使用 MD5（32位）。

- 时间戳（timestamp） 您发出请求的时间 (UTC 时区) 。在查询请求中包含此值有助于防止第三方截取您的请求。时间戳是由13位数字组成的（非强制）。

- 必选和可选参数 每个方法都有一组用于定义 API 调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。 请一定注意：对于GET请求，每个方法自带的参数都需要进行签名运算； 对于POST请求，每个方法自带的参数不进行签名认证，即POST请求中需要进行签名运算的只有api_key、timestamp两个参数，其它参数放在body中。

- 签名 签名计算得出的值，用于确保签名有效和未被篡改。

例：
```
https://api.yibi.co/v1/user/addOrder?
apiKey=abcdabcd1234
&market=BTC/USDT
&price=50000
&qty=0.1
&timestamp=1619798400000
&type=1
&sign=calculated_value
```

## 签名运算

API 请求在通过 Internet 发送的过程中极有可能被篡改。为了确保请求未被更改，我们会要求用户在每个请求中带上签名，来校验参数或参数值在传输途中是否发生了更改。

### 计算签名所需的步骤：

规范要计算签名的请求 因为使用 MD5 进行签名计算时，使用不同内容计算得到的结果会完全不同。所以在进行签名计算前，请先对请求进行规范化处理。我们以上面请求为例进行说明
https://api.yibi.co/v1/order/orders

-  将请求参数按照ASCII码的顺序进行排序，并去除sign字段,增加apiSecret字段（SECRET_KEY,仅作签名使用，最后请求不要携带此参数），得到下面的结果
```
apiKey=abcdabcd1234
apiSecret=aaaabbbb1111
market=BTC/USDT
price=50000
qty=0.1
timestamp=1619798400000
type=1
```
- 按照以上顺序，将各参数使用字符’&’连接。
```
apiKey=abcdabcd1234&apiSecret=aaaabbbb1111&market=BTC/USDT&price=50000&qty=0.1&timestamp=1619798400000&type=1
```
- 使用secret_key通过MD5 32位签名，得到
```
sign = MD5('apiKey=abcdabcd1234&apiSecret=aaaabbbb1111&market=BTC/USDT&price=50000&qty=0.1&timestamp=1619798400000&type=1', secret_key);
```

- 最后将sign拼装回去， 最终，发送到服务器的 API 请求应该为：
```
https://api.yibi.co/v1/user/orders?apiKey=abcdabcd1234&market=BTC/USDT&price=50000&qty=0.1&timestamp=1619798400000&type=1&sign=4537fc8d082ea13a16a89523c62d6775
```

- JAVA签名代码示例
```
    /**
     * MD5 32位方法
     */
    public static String md5(String string) {
        byte[] hash;
        try {
            hash = MessageDigest.getInstance("MD5").digest(string.getBytes("UTF-8"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("UTF-8 is unsupported", e);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("MessageDigest不支持MD5Util", e);
        }
        StringBuilder hex = new StringBuilder(hash.length * 2);
        for (byte b : hash) {
            if ((b & 0xFF) < 0x10) hex.append("0");
            hex.append(Integer.toHexString(b & 0xFF));
        }
        return hex.toString();
    }

    /**
     * md5签名
     * 按参数名称升序，将参数值进行连接、签名
     */
    public static String sign(String appSecret, TreeMap<String, String> params) {
        StringBuilder paramValues = new StringBuilder();
        params.put("appSecret", appSecret);
        for (Map.Entry<String, String> entry : params.entrySet()) {
            paramValues.append(entry.getValue());
        }
        return md5(paramValues.toString());
    }
```
