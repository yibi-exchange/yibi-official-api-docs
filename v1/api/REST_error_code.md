# Error Code
code = 1, it means request succeeded, other situations mean error. See error code reference form below:

| Error Code | Explanation |
| ------ | ------ |
| -1001 | The request is beyond limit, if you don’t have API_KEY, please[apply for API_KEY](https://yibi.co/cn/apiManage)as soon as possible. |
| -1002 | Invalid API_KEY |
| -1003 | Invalid signature , pleas refer to [signature verification](/v1/api/REST_authentication.md) |
| -1004 | Request timestamp has expired |
| -1005 | No permission, request trade and order API need to verify API_KEY |
| -1006 | Account is abnormal, please contact customer service |
| -1101 | Invalid  ticker_Id |
| -1102 | Invalid order_id |
| -1103 | Invalid price |
| -1104 | Invalid qty |
| 0 | Unknown error |