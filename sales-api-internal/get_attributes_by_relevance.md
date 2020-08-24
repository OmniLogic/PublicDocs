# /get-attributes-by-relevance

## Description

TODO

**API Endpoint:**
```http
POST /get-attributes-by-relevance
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
| `niche` | `string` | **Required.** |
| `entity` | `string` | **Required.** |


Example:

```json
{
    "niche": "bebidas",
    "entity": "vinho"
}
```

## Response Body

**Format:** `JSON`

Example:

TODO

## Status Codes

TODO

---
[Take me back to the main page](../README.md)
