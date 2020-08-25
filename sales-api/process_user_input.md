# /process-user-input

## Description

Returns structured data from user shopping intention and optionally searches for offers.

**API Endpoint:**
```http
POST /process-user-input
```
## Status Codes

| Status Code | Description |
| :--- | :--- |
| 200 - `OK` | Successfully extracted `structured_data` from `relevant_fragment`. |
| 400 - `BAD_REQUEST` | Can occur if there's a missing required parameter in the Request Body.|
| 403 - `FORBIDDEN` | There's something wrong with your credentials. |
| 422 - `UNPROCESSABLE_ENTITY` | Impossible to extract `structured_data` from `relevant_fragment`. |
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. (This is rare.) |
| 503 - `SERVICE_UNAVAILABLE` | One or more service is down. (This is a service that relies on several APIs.) |

## Headers

| Key | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `YOUR_API_KEY` | **Required.** |
| `Content-Type` | `application/json` | **Required.** |


## Request Body

**Format:** `JSON`

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `user_input` | `string` | **Required.** User input from an assistant / bot (when asked for his/her shopping intention)|
| `previous_context` | `string` | **Optional.** Previously detected relevant fragment (to be used when aggregating information). See examples below.|
| `enable_search` | `boolean` | **Default: `true`**. Enables / disables offers search. |
| `search_size` | `integer` | **Default: `20`**. Maximum number of offers to be returned if `enable_search` is set to `true`. |
| `user_email` | `string` | **Optional.** User e-mail address. |
| `user_phone` | `string` | **Optional.** User phone number. |


Example:

```json
{
    "input_text": "quero uma geladeira brastemp",
    "enable_search": true,
    "search_size": 1
}
```

## Response Body

**Format:** `JSON`

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `user_input` | `string` | User input from an assistant / bot (provided in the request body). |
| `spellchecked_user_input` | `string` | Spell-checked user input (pt-BR) |
| `previous_context` | `string` | Previously detected relevant fragment (provided in the request body). |
| `relevant_fragment` | `string` | Relevant fragment detected from `spellchecked_user_input`. This is the actual part used for data extraction. |
| `structured_data` | `object` | Structured data extracted from `relevant_fragment`. |
| `metadata_sorted_by_relevance` | `array` | Array of available metadata from the detected entity. The array is sorted by metadata usefulness. |
| `metadata_translation` | `object` | Dictionary for the metadata returned in the previous array. |
| `search_result` | `object` | Contains the resulting search using data from `structured_data` as query. |

When there aren't any configured metadata for the detected entity, `metadata_sorted_by_relevance` will be returned as an empty array and `metadata_translation` will be return as an empty object.

When `enable_search` is set to false, `search_result` will be an empty object.

It is important to note the `exact_search` field inside `search_result`. It is set to `true` when `structured_data` was used as query in the search. It is set to `false` when it was impossible to find an exact match, but some similar offers were found.

The `metadata_filters` field inside `search_result` contains an array of available filters and its values.

Example:

```json
{
    "user_input": "quero uma geladeira brastemp",
    "spellchecked_user_input": "quero uma geladeira brastemp",
    "previous_context": null,
    "relevant_fragment": "geladeira brastemp",
    "structured_data": {
        "niche": "eletrodomesticos",
        "entity": "Refrigerador",
        "probability": "0.99999845",
        "metadata": {
            "brand": "Brastemp"
        }
    },
    "metadata_sorted_by_relevance": [
        "brand",
        "type-of-defrost",
        "color",
        "type-of-refrigerator",
        "voltage"
    ],
    "metadata_translation": {
        "brand": "marca",
        "type-of-defrost": "tipo de degelo",
        "color": "cor",
        "type-of-refrigerator": "tipo de geladeira",
        "voltage": "voltagem"
    },
    "search_result": {
        "total_available": 69,
        "search_size": 1,
        "exact_search": true,
        "metadata_filters": [
            {
                "name": "substantive",
                "translation": "Produto",
                "total": 69,
                "values": [
                    {
                        "value": "Refrigerador",
                        "total": 69,
                        "priceRange": {
                            "min": 0.0,
                            "max": 10589.0
                        }
                    }
                ]
            },
            {
                "name": "brand",
                "translation": "Marca",
                "total": 69,
                "values": [
                    {
                        "value": "Brastemp",
                        "total": 69,
                        "priceRange": {
                            "min": 0.0,
                            "max": 10589.0
                        }
                    }
                ]
            },
            {
                "name": "voltage",
                "translation": "Voltagem",
                "total": 66,
                "values": [
                    {
                        "value": "127 V",
                        "total": 36,
                        "priceRange": {
                            "min": 0.0,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "220 V",
                        "total": 27,
                        "priceRange": {
                            "min": 0.0,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "110V/127V",
                        "total": 3,
                        "priceRange": {
                            "min": 2649.0,
                            "max": 4827.0
                        }
                    }
                ]
            },
            {
                "name": "color",
                "translation": "Cor",
                "total": 66,
                "values": [
                    {
                        "value": "Branco",
                        "total": 33,
                        "priceRange": {
                            "min": 131.76,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "Inox",
                        "total": 18,
                        "priceRange": {
                            "min": 2459.0,
                            "max": 9517.04
                        }
                    },
                    {
                        "value": "Evox",
                        "total": 12,
                        "priceRange": {
                            "min": 0.0,
                            "max": 5770.01
                        }
                    },
                    {
                        "value": "Preto",
                        "total": 3,
                        "priceRange": {
                            "min": 0.0,
                            "max": 7329.0
                        }
                    }
                ]
            },
            {
                "name": "capacity",
                "translation": "Capacidade",
                "total": 65,
                "values": [
                    {
                        "value": "228 L",
                        "total": 1,
                        "priceRange": {
                            "min": 2859.0,
                            "max": 2859.0
                        }
                    },
                    {
                        "value": "375 L",
                        "total": 10,
                        "priceRange": {
                            "min": 2299.0,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "400 L",
                        "total": 5,
                        "priceRange": {
                            "min": 2519.1,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "419 L",
                        "total": 3,
                        "priceRange": {
                            "min": 0.0,
                            "max": 6382.5
                        }
                    },
                    {
                        "value": "443 L",
                        "total": 6,
                        "priceRange": {
                            "min": 3149.1,
                            "max": 3799.0
                        }
                    },
                    {
                        "value": "460 L",
                        "total": 5,
                        "priceRange": {
                            "min": 0.0,
                            "max": 5571.01
                        }
                    },
                    {
                        "value": "462 L",
                        "total": 7,
                        "priceRange": {
                            "min": 3059.1,
                            "max": 3369.0
                        }
                    },
                    {
                        "value": "478 L",
                        "total": 7,
                        "priceRange": {
                            "min": 4099.0,
                            "max": 5471.51
                        }
                    },
                    {
                        "value": "500 L",
                        "total": 4,
                        "priceRange": {
                            "min": 3489.0,
                            "max": 4103.0
                        }
                    },
                    {
                        "value": "540 L",
                        "total": 8,
                        "priceRange": {
                            "min": 4867.9,
                            "max": 10446.4
                        }
                    },
                    {
                        "value": "560 L",
                        "total": 2,
                        "priceRange": {
                            "min": 10589.0,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "573 L",
                        "total": 7,
                        "priceRange": {
                            "min": 4094.91,
                            "max": 4795.0
                        }
                    }
                ]
            },
            {
                "name": "type_of_refrigerator",
                "translation": "Tipo de Refrigerador",
                "total": 61,
                "values": [
                    {
                        "value": "Duplex",
                        "total": 29,
                        "priceRange": {
                            "min": 2384.91,
                            "max": 5471.51
                        }
                    },
                    {
                        "value": "Inverse",
                        "total": 25,
                        "priceRange": {
                            "min": 0.0,
                            "max": 9517.04
                        }
                    },
                    {
                        "value": "Side by Side",
                        "total": 5,
                        "priceRange": {
                            "min": 131.76,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "French Door",
                        "total": 2,
                        "priceRange": {
                            "min": 10446.4,
                            "max": 10446.4
                        }
                    }
                ]
            },
            {
                "name": "model",
                "translation": "Modelo",
                "total": 60,
                "values": [
                    {
                        "value": "BRM56AK",
                        "total": 3,
                        "priceRange": {
                            "min": 3199.0,
                            "max": 3369.0
                        }
                    },
                    {
                        "value": "BRE80",
                        "total": 3,
                        "priceRange": {
                            "min": 4094.91,
                            "max": 4459.0
                        }
                    },
                    {
                        "value": "BRM45HB",
                        "total": 3,
                        "priceRange": {
                            "min": 2399.0,
                            "max": 2609.91
                        }
                    },
                    {
                        "value": "BRM56AB",
                        "total": 3,
                        "priceRange": {
                            "min": 3059.1,
                            "max": 3130.9
                        }
                    },
                    {
                        "value": "BRE80AK",
                        "total": 3,
                        "priceRange": {
                            "min": 4499.0,
                            "max": 4795.0
                        }
                    },
                    {
                        "value": "BRM59AK",
                        "total": 3,
                        "priceRange": {
                            "min": 4342.0,
                            "max": 5471.51
                        }
                    },
                    {
                        "value": "BRM57AB",
                        "total": 3,
                        "priceRange": {
                            "min": 3489.0,
                            "max": 3549.0
                        }
                    },
                    {
                        "value": "BRM45HK",
                        "total": 2,
                        "priceRange": {
                            "min": 2649.0,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "BRM54HBA",
                        "total": 2,
                        "priceRange": {
                            "min": 2599.0,
                            "max": 2599.0
                        }
                    },
                    {
                        "value": "BRE57AK",
                        "total": 2,
                        "priceRange": {
                            "min": 3529.0,
                            "max": 3799.0
                        }
                    },
                    {
                        "value": "BRO80AK",
                        "total": 2,
                        "priceRange": {
                            "min": 4949.1,
                            "max": 5770.01
                        }
                    },
                    {
                        "value": "BRE57AB",
                        "total": 2,
                        "priceRange": {
                            "min": 3489.0,
                            "max": 3489.0
                        }
                    },
                    {
                        "value": "BRE57",
                        "total": 2,
                        "priceRange": {
                            "min": 3149.1,
                            "max": 3509.1
                        }
                    },
                    {
                        "value": "BRE59AK",
                        "total": 2,
                        "priceRange": {
                            "min": 0.0,
                            "max": 5571.01
                        }
                    },
                    {
                        "value": "GRO80AB",
                        "total": 2,
                        "priceRange": {
                            "min": 10446.4,
                            "max": 10446.4
                        }
                    },
                    {
                        "value": "BRM59AB",
                        "total": 2,
                        "priceRange": {
                            "min": 4099.0,
                            "max": 4099.0
                        }
                    },
                    {
                        "value": "BRO80AB",
                        "total": 1,
                        "priceRange": {
                            "min": 4867.9,
                            "max": 4867.9
                        }
                    },
                    {
                        "value": "BRO81AR",
                        "total": 1,
                        "priceRange": {
                            "min": 9517.04,
                            "max": 9517.04
                        }
                    },
                    {
                        "value": "BRM44HB",
                        "total": 1,
                        "priceRange": {
                            "min": 2299.0,
                            "max": 2299.0
                        }
                    },
                    {
                        "value": "BRM44",
                        "total": 1,
                        "priceRange": {
                            "min": 2384.91,
                            "max": 2384.91
                        }
                    },
                    {
                        "value": "BRY59AE",
                        "total": 1,
                        "priceRange": {
                            "min": 6382.5,
                            "max": 6382.5
                        }
                    },
                    {
                        "value": "BRO80AE",
                        "total": 1,
                        "priceRange": {
                            "min": 7329.0,
                            "max": 7329.0
                        }
                    },
                    {
                        "value": "BRM54",
                        "total": 1,
                        "priceRange": {
                            "min": 2699.1,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "BRY59",
                        "total": 1,
                        "priceRange": {
                            "min": 0.0,
                            "max": 0.0
                        }
                    },
                    {
                        "value": "BRM54HB",
                        "total": 1,
                        "priceRange": {
                            "min": 2659.0,
                            "max": 2659.0
                        }
                    },
                    {
                        "value": "BRM58AK",
                        "total": 1,
                        "priceRange": {
                            "min": 4103.0,
                            "max": 4103.0
                        }
                    },
                    {
                        "value": "BRM56",
                        "total": 1,
                        "priceRange": {
                            "min": 3239.1,
                            "max": 3239.1
                        }
                    },
                    {
                        "value": "BRY59AK",
                        "total": 1,
                        "priceRange": {
                            "min": 5199.0,
                            "max": 5199.0
                        }
                    },
                    {
                        "value": "BRE59AB",
                        "total": 1,
                        "priceRange": {
                            "min": 4259.0,
                            "max": 4259.0
                        }
                    },
                    {
                        "value": "BRE59",
                        "total": 1,
                        "priceRange": {
                            "min": 4229.91,
                            "max": 4229.91
                        }
                    },
                    {
                        "value": "BVR28MB",
                        "total": 1,
                        "priceRange": {
                            "min": 2859.0,
                            "max": 2859.0
                        }
                    },
                    {
                        "value": "BRM59",
                        "total": 1,
                        "priceRange": {
                            "min": 4139.91,
                            "max": 4139.91
                        }
                    },
                    {
                        "value": "BRE80AB",
                        "total": 1,
                        "priceRange": {
                            "min": 4643.0,
                            "max": 4643.0
                        }
                    },
                    {
                        "value": "BRO81",
                        "total": 1,
                        "priceRange": {
                            "min": 9517.04,
                            "max": 9517.04
                        }
                    },
                    {
                        "value": "BRM44HK",
                        "total": 1,
                        "priceRange": {
                            "min": 2459.0,
                            "max": 2459.0
                        }
                    },
                    {
                        "value": "BRS62",
                        "total": 1,
                        "priceRange": {
                            "min": 131.76,
                            "max": 131.76
                        }
                    },
                    {
                        "value": "BRS75BR",
                        "total": 1,
                        "priceRange": {
                            "min": 462.5,
                            "max": 462.5
                        }
                    }
                ]
            },
            {
                "name": "type_of_defrost",
                "translation": "Tipo de Degelo",
                "total": 55,
                "values": [
                    {
                        "value": "Frost Free",
                        "total": 55,
                        "priceRange": {
                            "min": 0.0,
                            "max": 10589.0
                        }
                    }
                ]
            },
            {
                "name": "features",
                "translation": "Atributos",
                "total": 55,
                "values": [
                    {
                        "value": "Inverse",
                        "total": 16,
                        "priceRange": {
                            "min": 0.0,
                            "max": 10446.4
                        }
                    },
                    {
                        "value": "Painel Eletrônico",
                        "total": 16,
                        "priceRange": {
                            "min": 2399.0,
                            "max": 9517.04
                        }
                    },
                    {
                        "value": "Prateleiras Removíveis",
                        "total": 5,
                        "priceRange": {
                            "min": 2499.0,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "Controle eletrônico",
                        "total": 3,
                        "priceRange": {
                            "min": 4499.0,
                            "max": 4795.0
                        }
                    },
                    {
                        "value": "Smart bar",
                        "total": 3,
                        "priceRange": {
                            "min": 4499.0,
                            "max": 4795.0
                        }
                    },
                    {
                        "value": "Dispenser de Água",
                        "total": 3,
                        "priceRange": {
                            "min": 3489.0,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "Controle Eletrônico",
                        "total": 2,
                        "priceRange": {
                            "min": 4094.91,
                            "max": 4201.9
                        }
                    },
                    {
                        "value": "Painel Touch",
                        "total": 2,
                        "priceRange": {
                            "min": 2699.1,
                            "max": 3239.1
                        }
                    },
                    {
                        "value": "Dispenser De Água",
                        "total": 2,
                        "priceRange": {
                            "min": 3489.0,
                            "max": 4103.0
                        }
                    },
                    {
                        "value": "Porta Latas",
                        "total": 1,
                        "priceRange": {
                            "min": 2699.1,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "Turbo Freezer",
                        "total": 1,
                        "priceRange": {
                            "min": 2699.1,
                            "max": 2699.1
                        }
                    },
                    {
                        "value": "Porta latas",
                        "total": 1,
                        "priceRange": {
                            "min": 4259.0,
                            "max": 4259.0
                        }
                    }
                ]
            },
            {
                "name": "doors",
                "translation": "Portas",
                "total": 47,
                "values": [
                    {
                        "value": "2 Portas",
                        "total": 32,
                        "priceRange": {
                            "min": 2399.0,
                            "max": 10589.0
                        }
                    },
                    {
                        "value": "2 portas",
                        "total": 7,
                        "priceRange": {
                            "min": 131.76,
                            "max": 4259.0
                        }
                    },
                    {
                        "value": "3 Portas",
                        "total": 6,
                        "priceRange": {
                            "min": 4867.9,
                            "max": 10446.4
                        }
                    },
                    {
                        "value": "3 portas",
                        "total": 2,
                        "priceRange": {
                            "min": 0.0,
                            "max": 9517.04
                        }
                    }
                ]
            }
        ],
        "offers": [
            {
                "url": "https://www.shopfacil.com.br/refrigerador-brastemp-brm45hk-375-l-inox-4980041/p",
                "name": "Refrigerador Brastemp BRM45HK 375 L Inox 220 V",
                "available": true,
                "img": "https://shopfacil.vteximg.com.br/arquivos/ids/61034669/5280038_1.jpg?v=637333809354700000",
                "sku": "5280038",
                "price": 2649.0,
                "listPrice": 2649.0,
                "priceDiscount": 0.0,
                "installments": 10,
                "installmentValue": 264.9,
                "score": 3.25,
                "productId": "4980041",
                "sellerName": "pontofrio",
                "categories": [
                    "/Eletrodomésticos/Geladeira/",
                    "/Eletrodomésticos/"
                ],
                "normalizedCategories": [
                    "/eletrodomesticos/geladeira/",
                    "/eletrodomesticos/"
                ],
                "clusters": [
                    "1791"
                ],
                "label": [
                    {
                        "name": "voltage",
                        "value": "220 V"
                    }
                ],
                "seller_list": [
                    "Casas Bahia",
                    "Extra",
                    "Fast Shop",
                    "Ponto Frio"
                ]
            }
        ]
    }
}
```

## More examples

### Demonstrating the use of `previous_context`:

*First interaction:*

Request Body:

```json
{
    "input_text": "quero um celular",
    "enable_search": false
}
```

Response Body:

```json
{
    "user_input": "quero um celular",
    "spellchecked_user_input": "quero um celular",
    "previous_context": null,
    "relevant_fragment": "celular",
    "structured_data": {
        "niche": "eletronicos",
        "entity": "Celular",
        "probability": "0.99996483",
        "metadata": {}
    },
    "metadata_sorted_by_relevance": [
        "brand",
        "color"
    ],
    "metadata_translation": {
        "brand": "marca",
        "color": "cor"
    },
    "search_result": {}
}
```

*Second interaction:*

Note how the previously detected `relevant_fragment` is being used in the `previous_context` parameter.

Request Body:

```json
{
    "previous_context": "celular",
    "input_text": "pode ser um vermelho da apple",
    "enable_search": true,
    "search_size": 1
}
```

Response Body:

```json
{
    "user_input": "pode ser um vermelho da apple",
    "spellchecked_user_input": "pode ser um vermelho da Apple",
    "previous_context": "celular",
    "relevant_fragment": "celular vermelho da Apple",
    "structured_data": {
        "niche": "eletronicos",
        "entity": "Celular",
        "probability": "0.9999317",
        "metadata": {
            "brand": "Apple",
            "color": "Vermelho"
        }
    },
    "metadata_sorted_by_relevance": [
        "brand",
        "color"
    ],
    "metadata_translation": {
        "brand": "marca",
        "color": "cor"
    },
    "search_result": {
        "total_available": 14,
        "search_size": 1,
        "exact_search": true,
        "metadata_filters": [
            {
                "name": "substantive",
                "translation": "Produto",
                "total": 14,
                "values": [
                    {
                        "value": "Celular",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    }
                ]
            },
            {
                "name": "connectivity",
                "translation": "Conectividade",
                "total": 42,
                "values": [
                    {
                        "value": "4G",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    },
                    {
                        "value": "Bluetooth",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    },
                    {
                        "value": "WiFi",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    }
                ]
            },
            {
                "name": "brand",
                "translation": "Marca",
                "total": 14,
                "values": [
                    {
                        "value": "Apple",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    }
                ]
            },
            {
                "name": "capacity",
                "translation": "Capacidade",
                "total": 14,
                "values": [
                    {
                        "value": "64 GB",
                        "total": 3,
                        "priceRange": {
                            "min": 3945.0,
                            "max": 4642.48
                        }
                    },
                    {
                        "value": "64GB",
                        "total": 1,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 2849.05
                        }
                    },
                    {
                        "value": "128 GB",
                        "total": 5,
                        "priceRange": {
                            "min": 3485.55,
                            "max": 4499.0
                        }
                    },
                    {
                        "value": "128GB",
                        "total": 1,
                        "priceRange": {
                            "min": 3134.05,
                            "max": 3134.05
                        }
                    },
                    {
                        "value": "256 GB",
                        "total": 4,
                        "priceRange": {
                            "min": 3599.0,
                            "max": 5393.9
                        }
                    }
                ]
            },
            {
                "name": "color",
                "translation": "Cor",
                "total": 14,
                "values": [
                    {
                        "value": "Vermelho",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    }
                ]
            },
            {
                "name": "type_",
                "translation": "Tipo",
                "total": 14,
                "values": [
                    {
                        "value": "Smartphone",
                        "total": 14,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    }
                ]
            },
            {
                "name": "operating_system",
                "translation": "Sistema Operacional",
                "total": 14,
                "values": [
                    {
                        "value": "iOS 13",
                        "total": 10,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 5393.9
                        }
                    },
                    {
                        "value": "iOS 12",
                        "total": 4,
                        "priceRange": {
                            "min": 3485.55,
                            "max": 3945.0
                        }
                    }
                ]
            },
            {
                "name": "product_weight",
                "translation": "Peso do Produto",
                "total": 14,
                "values": [
                    {
                        "value": "194 g",
                        "total": 11,
                        "priceRange": {
                            "min": 3485.55,
                            "max": 5393.9
                        }
                    },
                    {
                        "value": "148 g",
                        "total": 3,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 3599.0
                        }
                    }
                ]
            },
            {
                "name": "name",
                "translation": "Nome",
                "total": 14,
                "values": [
                    {
                        "value": "iPhone 11",
                        "total": 7,
                        "priceRange": {
                            "min": 4399.0,
                            "max": 5393.9
                        }
                    },
                    {
                        "value": "iPhone XR",
                        "total": 4,
                        "priceRange": {
                            "min": 3485.55,
                            "max": 3945.0
                        }
                    },
                    {
                        "value": "iPhone SE",
                        "total": 3,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 3599.0
                        }
                    }
                ]
            },
            {
                "name": "screen_size",
                "translation": "Tamanho da Tela",
                "total": 14,
                "values": [
                    {
                        "value": "4.7\"",
                        "total": 3,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 3599.0
                        }
                    },
                    {
                        "value": "6.1\"",
                        "total": 10,
                        "priceRange": {
                            "min": 3485.55,
                            "max": 4926.77
                        }
                    },
                    {
                        "value": "6.1 polegadas",
                        "total": 1,
                        "priceRange": {
                            "min": 5393.9,
                            "max": 5393.9
                        }
                    }
                ]
            }
        ],
        "offers": [
            {
                "url": "https://www.shopfacil.com.br/iphone-xr-apple-128gb-branco-61quot-12mp---ios-1170319189/p",
                "name": "iPhone XR Apple 128GB Branco 6,1&quot; 12MP - iOS",
                "available": true,
                "img": "https://shopfacil.vteximg.com.br/arquivos/ids/60600760/6242353_1.jpg?v=637330552774100000",
                "sku": "6242353",
                "price": 3609.05,
                "listPrice": 3609.05,
                "priceDiscount": 0.0,
                "installments": 10,
                "installmentValue": 360.91,
                "score": 3.25,
                "productId": "1170319189",
                "sellerName": "004443713",
                "categories": [
                    "/Celular e Smartphone/Smartphone/",
                    "/Celular e Smartphone/"
                ],
                "normalizedCategories": [
                    "/celular-e-smartphone/smartphone/",
                    "/celular-e-smartphone/"
                ],
                "clusters": [
                    "1363",
                    "1627",
                    "1634",
                    "1642",
                    "1654",
                    "1730",
                    "1774",
                    "1789",
                    "1801"
                ],
                "seller_list": [
                    "Magazine Luiza"
                ]
            }
        ]
    }
}
```


