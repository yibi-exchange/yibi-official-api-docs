## Request Explanation
- YIBI API server address:https://api.yibi.co
- Content-Type:application/json must be declared in the POST request header information.
- For all request parameters, please follow the API explanation for parameter encapsulation.
- Submit the encapsulated API request to the server via POST or GET methods, and YIBI processes and returns the corresponding JSON format result.
- Please use https request.
- The account limit frequency is 100 times per 10 seconds (a single API_KEY dimension limit, if no signature is added, only access to the market interface is allowed, and the frequency will be limited to 10 times in 10 seconds).

## Return Format Explanation
- YIBI return all in JSON format, and will contain code fields.
- If request error raise, it will return code and msg fields, as follows:
```json
{
  "code": "-1003",
  "msg": "Invalid signature"
}
```
- If request succeeded, it will return code and data fields, as follows:
```json
{
  "code": "1",
  "data": "response data..."
}
```
- Error code please see[Error Code](/v1/api/REST_error_code.md)