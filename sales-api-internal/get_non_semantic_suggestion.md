# /get-non-semantic-suggestion

## Description

Returns a list of offers using non-structured data as query.

**API Endpoint:**
```http
POST /get-non-semantic-suggestion
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
| `price_range` | `List[float]` | Sets the minimum and maximum price of the offers to be returned. |
| `substantive` | `string` | Sets the entity type to be returned. |
| `query` | `string` | **Required.** Query to be used when searching for offers. |
| `page_size` | `int` | **Default: `5`**. Number of offers to be returned. |

Example:

```json
{
    "store": "shopfacil",
    "substantive": "blusa",
    "price_range": [
        0,
        200
    ],
    "query": "blusa azul listrada qazwsx",
    "page_size": 1
}
```

## Response Body

**Format:** `JSON`

Example:

```json
{
    "query": "blusa azul listrada qazwsx",
    "altered_query": "blusa azul listrada",
    "price_statistics": {
        "average": 3387.244610778443,
        "median": 2699.0,
        "quantiles": [
            1454.03,
            2699.0,
            4199.0
        ]
    },
    "suggestions": [
        {
            "url": "https://www.shopfacil.com.br/camiseta-infantil-masculina-manga-longa-moletinho-2721223/p",
            "name": "Camiseta Infantil Masculina Manga Longa Moletinho -Azul Escuro-8",
            "available": true,
            "img": "https://shopfacil.vteximg.com.br/arquivos/ids/29205300/2803752_1.jpg?v=636956980576300000",
            "sku": "2803752",
            "price": 25.12,
            "listPrice": 25.9,
            "priceDiscount": 3.0115830115830025,
            "installments": 1,
            "installmentValue": 25.12,
            "score": 15.12683391571045,
            "productId": "2721223",
            "sellerName": "900000032",
            "categories": [
                "/Moda/Moda Infantil/Camiseta/",
                "/Moda/Moda Infantil/",
                "/Moda/"
            ],
            "normalizedCategories": [
                "/moda/moda-infantil/camiseta/",
                "/moda/moda-infantil/",
                "/moda/"
            ],
            "label": [
                {
                    "name": "size",
                    "value": "8"
                }
            ],
            "seller_list": [
                "Poetique"
            ]
        }
    ]
}
```

When it's not possible to find offers with the provided query, its last tokens are recursively removed until at least one offer is found.

`query` represents the query passed in the Request Body.
`altered_query` represents the actual query that was able to return results.

`price_statistics` contains several useful statistics about the price of the returned offers.

## Status Codes

| Status Code | Description |
| :--- | :--- |
| 200 - `OK` | Successfully found offers. |
| 400 - `Bad Request` | Can occur if there's a missing required parameter in the Request Body.|
| 403 - `FORBIDDEN` | There's something wrong with your credentials. |
| 404 - `Suggestion Not Found` | The requested suggestion doesn't exist. You may try to change the query. |
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |
| 501 - `Not Implemented` | Store is not supported. |

---
[Take me back to the main page](../README.md)
