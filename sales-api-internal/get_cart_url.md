# /get-cart-url

## Description

Generates cart URLs.

**API Endpoint:**
```http
POST /get-cart-url
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
| `store` | `string` | **Default: `"shopfacil"`.** |
| `product_list` | `List` | **Required.** List of products to be added in cart. |
| `utm_source` | `string` | **Optional.** UTM Source |
| `utm_medium` | `string` | **Optional.** UTM Medium |
| `utm_campaign` | `string` | **Optional.** UTM Campaign |
| `shortened_url` | `bool` | **Default: `false`**. Determines if a shortened version of the URL should be returned.|

**`product_list` format:** This parameter should be a list of lists. Each inner list should contain the product sku (`str`), the amount (`int`) and the seller (`str`) of the offer.

In order to add the UTM strings in the URL, all `utm_source`, `utm_medium` and `utm_campaign` parameters should be provided.


Example:

```json
{
    "store": "shopfacil",
    "product_list": [
        [
            "6346577",
            1,
            "004576209"
        ],
        [
            "5857548",
            1,
            "zattini"
        ],
        [
            "6350869",
            1,
            "zattini"
        ]
    ],
    "utm_source": "UTM_SOURCE_STRING",
    "utm_medium": "UTM_MEDIUM_STRING",
    "utm_campaign": "UTM_CAMPAIGN_STRING",
    "shortened_url": false
}
```

## Response Body

Example:

```json
"https://www.shopfacil.com.br/checkout?opzcart=%5B%5B%226346577%22%2C%201%2C%20%22004576209%22%5D%2C%20%5B%225857548%22%2C%201%2C%20%22zattini%22%5D%2C%20%5B%226350869%22%2C%201%2C%20%22zattini%22%5D%5D&utm_source=UTM_SOURCE_STRING&utm_medium=UTM_MEDIUM_STRING&utm_campaign=UTM_CAMPAIGN_STRING#/cart"
```

## Status Codes

| Status Code | Description |
| :--- | :--- |
| 200 - `OK` | Success. |
| 400 - `Bad Request` | Can occur if there's a missing required parameter in the Request Body.|
| 403 - `FORBIDDEN` | There's something wrong with your credentials. |
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |
| 501 - `Not Implemented` | Store is not supported. |



---
[Take me back to the main page](../README.md)
