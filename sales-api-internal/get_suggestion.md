# /get-suggestion

## Description

Returns a personalized list of offers that best match the provided query.

**API Endpoint:**
```http
POST /get-suggestion
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
| `store` | `string` | **Required.** (Example: "shopfacil")|
| `query` | `dict` | **Required.** Filters to be used when searching for offers.|
| `page_size` | `int` | **Default: `5`**. Number of offers to be returned. |
| `behavior` | `string` | **Default: `PRD`**. Defines the recommendation behavior. (Check the table below) |
| `user_email` | `string` | **Optional** E-mail used to identify the user for a personalized recommendation. |

**Possible `behavior` values:**

| Value | Description |
| :--- | :--- |
| `PRD` | Offer "view" |
| `CART` | Offer added to cart |
| `ORD` | Offer checkout |
| `REV` | Offer revenue |


If no `user_email` is supplied, the provided behavior will be used to guide the recommendation.
It can also be the case when the user is not found in our database.

Example:

```json
{
    "store": "shopfacil",
    "query": {
        "price": {
            "gt": 20,
            "lte": 500
        },
        "metadata": {
            "substantive": {
                "or": [
                    "Calça",
                    "Blusa"
                ]
            },
            "color": {
                "or": [
                    "Azul",
                    "Verde"
                ]
            },
            "size": "M"
        }
    },
    "page_size": 10,
    "user_email": "email@omnilogic.ai"
}
```

## Response Body

**Format:** `JSON`

Example (with only the first offer, for simplicity):

```json
[
    {
        "url": "https://www.shopfacil.com.br/blusa-mob-ampla-amarracao-feminina-1170829835/p",
        "name": "Blusa Mob Ampla Amarração Feminina Verde M",
        "available": true,
        "img": "https://shopfacil.vteximg.com.br/arquivos/ids/45699590/6526534_1.jpg?v=637163369290200000",
        "id": "6526534",
        "seller": "zattini",
        "brand": "Mob",
        "type": "Moda Feminina",
        "price": 109.99,
        "installments": 4,
        "inactive": false,
        "refId": "136-0558-060-03",
        "metadata": {
            "color": "Verde",
            "gender": "Feminino",
            "size": "M",
            "substantive": "Blusa",
            "brand": "MOB"
        },
        "parent_id": "1170829835",
        "parent_offer": false,
        "created_at": 1570749042614,
        "category_name": "moda",
        "search_category": "Moda",
        "list_price": 109.99,
        "price_discount": 0.0,
        "installment_value": 27.49,
        "raw_category": [
            "/Moda/Moda Feminina/Blusas/",
            "/Moda/Moda Feminina/",
            "/Moda/"
        ]
    }
]
```

## Status Codes

| Status Code | Description |
| :--- | :--- |
| 200 - `OK` | Successfully found offers. |
| 400 - `Bad Request` | Can occur if there's a missing required parameter in the Request Body.|
| 403 - `FORBIDDEN` | There's something wrong with your credentials. |
| 404 - `Suggestion Not Found` | The requested suggestion doesn't exist. You may try to change the query. |
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |

---
[Take me back to the main page](../README.md)
