# /save-log

## Description

This API is intended to be used only by the Omnilogic team. It emits a logging message in the server.


**API Endpoint:**
```http
POST /save-log
```

## Headers

| Key | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `YOUR_API_KEY` | **Required.** |
| `Content-Type` | `application/json` | **Required.** |


## Request Body

**Format:** `JSON`

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `level` | `string` | **Default: `"INFO"`**. Logging level.|
| `source` | `string` | **Required.** The name of your application. |
| `log_string` | `string` | **Required.** Log message. |

**Possible `level` values:**

| Value | 
| :--- | 
| `INFO` |
| `WARNING` | 
| `ERROR` | 
| `CRITICAL` | 


Example:

```json
{
    "level": "info",
    "source": "source_string",
    "log_string": "TESTE"
}
```

## Response Body

No body is returned. The success can be checked in the Response Status Code.

The logging message can be consumed in Kibana.

Example: ```INFO:     [SOURCE_STRING] - TESTE```

## Status Codes

| Status Code | Description |
| :--- | :--- |
| 200 - `OK` | Success. |
| 400 - `Bad Request` | Can occur if there's a missing required parameter in the Request Body.|
| 403 - `FORBIDDEN` | There's something wrong with your credentials. |
| 422 - `Invalid Logging Level` | Invalid `level` value. Check the table above to solve this problem.|
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |

---
[Take me back to the main page](../README.md)
