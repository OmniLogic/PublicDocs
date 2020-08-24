# /shorten-url

## Description

TODO

**API Endpoint:**
```http
POST /shorten-url
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
| `url` | `string` | **Required.** |


Example:

```json
{
    "url": "https://omnilogic.ai"
}
```

## Response Body

**Format:** `JSON`

Example:

```json
{
    "original_url": "https://omnilogic.ai",
    "shortened_url": "https://bit.ly/3gaHAbj"
}
```

## Status Codes

TODO

---
[Take me back to the main page](../README.md)
