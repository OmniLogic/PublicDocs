# /get-similar-offers

## Description

Returns a list of similar offers of the provided sku, matching the query.

**API Endpoint:**
```http
POST /get-similar-offers
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
| `product_sku` | `string` | **Required.** Offer to be used as seed for getting its similar offers. |
| `behavior` | `string` | **Default: `PRD`** Check the table below for more info. |
| `query` | `dict` | **Required.** Filters to be used when searching for offers. |
| `page_size` | `int` | **Default: `5`**. Number of offers to be returned. |

**Possible `behavior` values:**

| Value | Description |
| :--- | :--- |
| `PRD` | Offer "view" |
| `CART` | Offer added to cart |
| `ORD` | Offer checkout |
| `REV` | Offer revenue |


Example:

```json
{
    "store": "shopfacil",
    "product_sku": "2363598",
    "behavior": "PRD",
    "query": {
        "price": {
            "gt": 200,
            "lte": 600
        }
    },
    "page_size": 3
}
```

## Response Body

**Format:** `JSON`

Example (with only the first offer, for simplicity):

```json
[
    {
        "url": "https://www.shopfacil.com.br/frigideira-skillet-redonda-com-alca-signature-26-cm-vermelho-le-creuset-4721821/p",
        "name": "Frigideira Skillet Redonda com Alça Signature 26 cm Vermelho Le Creuset",
        "available": true,
        "img": "https://shopfacil.vteximg.com.br/arquivos/ids/39888629/4941519_1.jpg?v=637057076656470000",
        "id": "4941519",
        "seller": "900000004",
        "brand": "Le Creuset",
        "type": "Panelas",
        "price": 483.31,
        "installments": 4,
        "inactive": false,
        "ean": "0024147231233",
        "refId": "20182260600422",
        "clusters": [],
        "metadata": {
            "substantive": "Frigideira"
        },
        "parent_id": "4721821",
        "parent_offer": false,
        "created_at": 1533327259538,
        "updated_at": 1586661356025,
        "category_name": "utilidades domésticas",
        "search_category": "Utilidades Domésticas",
        "list_price": 636.0,
        "price_discount": 24.007861635220127,
        "installment_value": 120.82,
        "raw_category": [
            "/Utilidades Domésticas/Panelas/Frigideira/",
            "/Utilidades Domésticas/Panelas/",
            "/Utilidades Domésticas/"
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
| 404 - `Similar Offer Not Found` | The requested suggestion doesn't exist. You may try to change the query. |
| 422 - `Invalid Behavior` | Invalid `behavior` value. Check the table above to solve this problem.|
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |

---
[Take me back to the main page](../README.md)
