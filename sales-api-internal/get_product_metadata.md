# /get-product-metadata

## Description

Extracts metadata from an offer name.

**API Endpoint:**
```http
POST /get-product-metadata
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
| `offer_name` | `string` | **Required.** |


Example:

```json
{
    "offer_name": "Lavadora de Roupas Brastemp 15kg BWH15ABBNA Branco"
}
```

## Response Body

**Format:** `JSON`

Example:

```json
{
    "niche": "eletrodomesticos",
    "entity": "Lavadora de Roupas",
    "probability": "0.9992455",
    "metadata": {
        "brand": "Brastemp",
        "capacity": "15 kg",
        "color": "Branco"
    }
}
```

## Status Codes

| Status Code | Description |
| :--- | :--- |
| 200 - `OK` | Success. |
| 400 - `Bad Request` | Can occur if there's a missing required parameter in the Request Body.|
| 403 - `FORBIDDEN` | There's something wrong with your credentials. |
| 404 - `Entity Not Found` | It was not possible to extract the entity, so it's impossible to extract metadata. |
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |


---
[Take me back to the main page](../README.md)
