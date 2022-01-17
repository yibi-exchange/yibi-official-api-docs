# Signature Authentication

## legal request structure
Based on security considerations, API requests other than the market API must be signed. A legitimate request consists of the following parts：

- The method request address is the access server address: api.yibi.co followed by the method name, such as api.yibi.co/v1/order/orders.

- API Access Key (secret_key) The SECRET_KEY in the API_KEY you applied for.

- Signature method: The hash-based protocol for the user to calculate the signature, we use MD5 (32-bit).

- Timestamp The time (UTC time zone) when you made the request. Including this value in your query request helps prevent third parties from intercepting your request. Timestamps consist of 13 digits (not mandatory).

- Required and optional parameters Each method has a set of required and optional parameters that define the API call. These parameters and their meanings can be viewed in the description of each method. Please note: For GET requests, the parameters of each method need to be signed; for POST requests, the parameters of each method are not subject to signature authentication, that is, only api_key and timestamp need to be signed in POST requests. Two parameters, other parameters are placed in the body.

- Signature The value calculated by the signature to ensure that the signature is valid and has not been tampered with.

Example：
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

## Signature operation

API requests are highly likely to be tampered with as they are sent over the Internet. To ensure that requests have not changed, we require users to sign each request to verify that parameters or parameter values have not changed in transit.

### Steps required to calculate the signature：

Specification of the request to calculate the signature Because when using MD5 for signature calculation, the result of calculation with different content will be completely different. Therefore, please normalize the request before performing the signature calculation. Let's take the above request as an example to illustrate
https://api.yibi.co/v1/order/orders

-  Sort the request parameters in the order of ASCII codes, remove the sign field, add the apiSecret field (SECRET_KEY, only used for signature, and finally request not to carry this parameter), and get the following result
```
apiKey=abcdabcd1234
apiSecret=aaaabbbb1111
market=BTC/USDT
price=50000
qty=0.1
timestamp=1619798400000
type=1
```
- In the above order, connect each parameter with the character '&'.
```
apiKey=abcdabcd1234&apiSecret=aaaabbbb1111&market=BTC/USDT&price=50000&qty=0.1&timestamp=1619798400000&type=1
```
- MD5 32-bit signature using secret_key
```
sign = MD5('apiKey=abcdabcd1234&apiSecret=aaaabbbb1111&market=BTC/USDT&price=50000&qty=0.1&timestamp=1619798400000&type=1', secret_key);
```

- Finally, the sign is assembled back, and finally, the API request sent to the server should be:
```
https://api.yibi.co/v1/user/orders?apiKey=abcdabcd1234&market=BTC/USDT&price=50000&qty=0.1&timestamp=1619798400000&type=1&sign=4537fc8d082ea13a16a89523c62d6775
```

- JAVA signature code example
```
    /**
     * MD5 32-bit method
     */
    public static String md5(String string) {
        byte[] hash;
        try {
            hash = MessageDigest.getInstance("MD5").digest(string.getBytes("UTF-8"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("UTF-8 is unsupported", e);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("MessageDigest not suppurt MD5Util", e);
        }
        StringBuilder hex = new StringBuilder(hash.length * 2);
        for (byte b : hash) {
            if ((b & 0xFF) < 0x10) hex.append("0");
            hex.append(Integer.toHexString(b & 0xFF));
        }
        return hex.toString();
    }

    /**
     * md5 signature
     * Concatenate and sign parameter values in ascending order of parameter names
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
