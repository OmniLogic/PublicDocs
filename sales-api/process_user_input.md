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
| 500 - `INTERNAL SERVER ERROR` | Something went wrong. |
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
    "input_text": "quero a televisão sony mais vendida da loja",
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

There is a `price_range` field inside `structured_data` that returns an array with the minimum and maximum price defined by the user. It is returned as an empty array when there isn't a defined price.

There is a `sort_behavior` field inside `structured_data` that recognizes how to sort the search according to what the user said. It can have the following values: `"best_selling"`, `"most_relevant"`, `"best_discount"` or `null`. 

It is important to note the `exact_search` field inside `search_result`. It is set to `true` when `structured_data` was used as query in the search. It is set to `false` when it was impossible to find an exact match, but some similar offers were found.

The `metadata_filters` field inside `search_result` contains an array of available filters and its values.

Example:

```json
{
    "user_input": "quero a televisão sony mais vendida da loja",
    "spellchecked_user_input": "quero a televisão sony mais vendida da loja",
    "previous_context": null,
    "relevant_fragment": "televisão sony",
    "structured_data": {
        "niche": "eletronicos",
        "entity": "TV",
        "probability": "0.9999807",
        "metadata": {
            "brand": "Sony"
        },
        "price_range": [],
        "sort_behavior": "best_selling"
    },
    "metadata_sorted_by_relevance": [
        "brand",
        "screen-size"
    ],
    "metadata_translation": {
        "brand": "marca",
        "screen-size": "tamanho da tela"
    },
    "search_result": {
        "total_available": 17,
        "search_size": 1,
        "exact_search": true,
        "metadata_filters": [
            {
                "name": "substantive",
                "translation": "Produto",
                "total": 17,
                "values": [
                    {
                        "value": "TV",
                        "total": 17,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    }
                ]
            },
            {
                "name": "resources",
                "translation": "Recursos",
                "total": 30,
                "values": [
                    {
                        "value": "Smart",
                        "total": 17,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    },
                    {
                        "value": "X-Reality PRO",
                        "total": 10,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    },
                    {
                        "value": "Conversor Digital",
                        "total": 3,
                        "priceRange": {
                            "min": 4274.05,
                            "max": 9214.05
                        }
                    }
                ]
            },
            {
                "name": "conectivity",
                "translation": "Conectividades",
                "total": 22,
                "values": [
                    {
                        "value": "HDMI",
                        "total": 9,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 25499.0
                        }
                    },
                    {
                        "value": "USB",
                        "total": 5,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 25499.0
                        }
                    },
                    {
                        "value": "Fone de Ouvido",
                        "total": 3,
                        "priceRange": {
                            "min": 4690.8,
                            "max": 25499.0
                        }
                    },
                    {
                        "value": "WiFi",
                        "total": 3,
                        "priceRange": {
                            "min": 4690.8,
                            "max": 25499.0
                        }
                    },
                    {
                        "value": "Wi-Fi",
                        "total": 2,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 8929.05
                        }
                    }
                ]
            },
            {
                "name": "brand",
                "translation": "Marca",
                "total": 17,
                "values": [
                    {
                        "value": "Sony",
                        "total": 17,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    }
                ]
            },
            {
                "name": "screen_size",
                "translation": "Tamanho da Tela",
                "total": 17,
                "values": [
                    {
                        "value": "32\"",
                        "total": 1,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 1325.75
                        }
                    },
                    {
                        "value": "48\"",
                        "total": 1,
                        "priceRange": {
                            "min": 2547.02,
                            "max": 2547.02
                        }
                    },
                    {
                        "value": "55\"",
                        "total": 3,
                        "priceRange": {
                            "min": 4299.0,
                            "max": 6568.67
                        }
                    },
                    {
                        "value": "65\"",
                        "total": 7,
                        "priceRange": {
                            "min": 4274.05,
                            "max": 17021.99
                        }
                    },
                    {
                        "value": "70\"",
                        "total": 1,
                        "priceRange": {
                            "min": 8929.05,
                            "max": 8929.05
                        }
                    },
                    {
                        "value": "75\"",
                        "total": 2,
                        "priceRange": {
                            "min": 7399.0,
                            "max": 12999.0
                        }
                    },
                    {
                        "value": "85\"",
                        "total": 2,
                        "priceRange": {
                            "min": 25499.0,
                            "max": 31271.13
                        }
                    }
                ]
            },
            {
                "name": "screen_type",
                "translation": "Tipo de Tela",
                "total": 17,
                "values": [
                    {
                        "value": "LED",
                        "total": 16,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    },
                    {
                        "value": "OLED",
                        "total": 1,
                        "priceRange": {
                            "min": 17021.99,
                            "max": 17021.99
                        }
                    }
                ]
            },
            {
                "name": "resolution",
                "translation": "Resolução",
                "total": 17,
                "values": [
                    {
                        "value": "4K",
                        "total": 15,
                        "priceRange": {
                            "min": 4274.05,
                            "max": 31271.13
                        }
                    },
                    {
                        "value": "HD",
                        "total": 1,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 1325.75
                        }
                    },
                    {
                        "value": "Full HD",
                        "total": 1,
                        "priceRange": {
                            "min": 2547.02,
                            "max": 2547.02
                        }
                    }
                ]
            },
            {
                "name": "voltage",
                "translation": "Voltagem",
                "total": 15,
                "values": [
                    {
                        "value": "Bivolt",
                        "total": 15,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    }
                ]
            },
            {
                "name": "model",
                "translation": "Modelo",
                "total": 15,
                "values": [
                    {
                        "value": "KDL-32W655D",
                        "total": 1,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 1325.75
                        }
                    },
                    {
                        "value": "XBR-65X905F",
                        "total": 1,
                        "priceRange": {
                            "min": 6839.05,
                            "max": 6839.05
                        }
                    },
                    {
                        "value": "XBR-55X905F",
                        "total": 1,
                        "priceRange": {
                            "min": 6568.67,
                            "max": 6568.67
                        }
                    },
                    {
                        "value": "XBR-65A9G",
                        "total": 1,
                        "priceRange": {
                            "min": 17021.99,
                            "max": 17021.99
                        }
                    },
                    {
                        "value": "XBR-65X805G",
                        "total": 1,
                        "priceRange": {
                            "min": 4690.8,
                            "max": 4690.8
                        }
                    },
                    {
                        "value": "XBR-55X955G",
                        "total": 1,
                        "priceRange": {
                            "min": 5015.0,
                            "max": 5015.0
                        }
                    },
                    {
                        "value": "XBR-65X905E",
                        "total": 1,
                        "priceRange": {
                            "min": 7810.43,
                            "max": 7810.43
                        }
                    },
                    {
                        "value": "XBR-75X805G",
                        "total": 1,
                        "priceRange": {
                            "min": 7399.0,
                            "max": 7399.0
                        }
                    },
                    {
                        "value": "XBR-65X955G",
                        "total": 1,
                        "priceRange": {
                            "min": 9214.05,
                            "max": 9214.05
                        }
                    },
                    {
                        "value": "XBR-70X835F",
                        "total": 1,
                        "priceRange": {
                            "min": 8929.05,
                            "max": 8929.05
                        }
                    },
                    {
                        "value": " XBR-85X905F",
                        "total": 1,
                        "priceRange": {
                            "min": 31271.13,
                            "max": 31271.13
                        }
                    },
                    {
                        "value": "XBR-75X955G",
                        "total": 1,
                        "priceRange": {
                            "min": 12999.0,
                            "max": 12999.0
                        }
                    },
                    {
                        "value": "XBR-85X955G",
                        "total": 1,
                        "priceRange": {
                            "min": 25499.0,
                            "max": 25499.0
                        }
                    },
                    {
                        "value": "KD-65X7505D",
                        "total": 1,
                        "priceRange": {
                            "min": 7839.02,
                            "max": 7839.02
                        }
                    },
                    {
                        "value": "KDL-48W655D",
                        "total": 1,
                        "priceRange": {
                            "min": 2547.02,
                            "max": 2547.02
                        }
                    }
                ]
            },
            {
                "name": "type_",
                "translation": "Tipo",
                "total": 13,
                "values": [
                    {
                        "value": "Smart TV",
                        "total": 13,
                        "priceRange": {
                            "min": 1325.75,
                            "max": 31271.13
                        }
                    }
                ]
            }
        ],
        "offers": [
            {
                "url": "https://www.shopfacil.com.br/smart-tv-sony-led-hd-32-com-motionflow-xr-240-x-protection-pro-e-wi-fi---kdl-32w655d-z-1171185325/p",
                "name": "Smart TV Sony LED HD 32 com Motionflow XR 240, X-Protection PRO e Wi-Fi - KDL-32W655D/Z BIVOLT",
                "available": true,
                "img": "https://shopfacil.vteximg.com.br/arquivos/ids/53437746/6878377_1.jpg?v=637286088969130000",
                "sku": "6878377",
                "price": 1325.75,
                "listPrice": 1325.75,
                "priceDiscount": 0.0,
                "installments": 1,
                "installmentValue": 1325.75,
                "score": 2.305081605911255,
                "productId": "1171185325",
                "sellerName": "004486803",
                "categories": [
                    "/Eletrônicos/TV/Smart TV/",
                    "/Eletrônicos/TV/",
                    "/Eletrônicos/"
                ],
                "normalizedCategories": [
                    "/eletronicos/tv/smart-tv/",
                    "/eletronicos/tv/",
                    "/eletronicos/"
                ],
                "label": [
                    {
                        "name": "voltage",
                        "value": "Bivolt"
                    }
                ],
                "seller_list": [
                    "Fast Shop"
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
        "metadata": {},
        "price_range": [],
        "sort_behavior": null
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
    "input_text": "pode ser um vermelho da apple de até 5 mil reais",
    "enable_search": true,
    "search_size": 1
}
```

Response Body:

```json
{
    "user_input": "pode ser um vermelho da apple de até 5 mil reais",
    "spellchecked_user_input": "pode ser um vermelho da Apple de até 5 mil reais",
    "previous_context": "celular",
    "relevant_fragment": "celular vermelho da Apple",
    "structured_data": {
        "niche": "eletronicos",
        "entity": "Celular",
        "probability": "0.9999317",
        "metadata": {
            "brand": "Apple",
            "color": "Vermelho"
        },
        "price_range": [
            0,
            5000
        ],
        "sort_behavior": null
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
        "total_available": 10,
        "search_size": 1,
        "exact_search": false,
        "metadata_filters": [
            {
                "name": "substantive",
                "translation": "Produto",
                "total": 10,
                "values": [
                    {
                        "value": "Celular",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "connectivity",
                "translation": "Conectividade",
                "total": 30,
                "values": [
                    {
                        "value": "4G",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    },
                    {
                        "value": "Bluetooth",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    },
                    {
                        "value": "WiFi",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "brand",
                "translation": "Marca",
                "total": 10,
                "values": [
                    {
                        "value": "Apple",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "capacity",
                "translation": "Capacidade",
                "total": 10,
                "values": [
                    {
                        "value": "64GB",
                        "total": 1,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 2999.0
                        }
                    },
                    {
                        "value": "64 GB",
                        "total": 3,
                        "priceRange": {
                            "min": 3789.61,
                            "max": 4642.48
                        }
                    },
                    {
                        "value": "128 GB",
                        "total": 3,
                        "priceRange": {
                            "min": 3999.0,
                            "max": 4386.9
                        }
                    },
                    {
                        "value": "128GB",
                        "total": 1,
                        "priceRange": {
                            "min": 3299.0,
                            "max": 3299.0
                        }
                    },
                    {
                        "value": "256 GB",
                        "total": 2,
                        "priceRange": {
                            "min": 3704.05,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "camera_resolution",
                "translation": "Resolução de Câmera",
                "total": 10,
                "values": [
                    {
                        "value": "12 MP",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "color",
                "translation": "Cor",
                "total": 10,
                "values": [
                    {
                        "value": "Vermelho",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "sim",
                "translation": "Número de Chip",
                "total": 10,
                "values": [
                    {
                        "value": "Dual Chip",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "type_",
                "translation": "Tipo",
                "total": 10,
                "values": [
                    {
                        "value": "Smartphone",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "warranty",
                "translation": "Garantia",
                "total": 10,
                "values": [
                    {
                        "value": "12 meses",
                        "total": 10,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 4799.0
                        }
                    }
                ]
            },
            {
                "name": "front_camera_resolution",
                "translation": "front_camera_resolution",
                "total": 10,
                "values": [
                    {
                        "value": "7 MP",
                        "total": 5,
                        "priceRange": {
                            "min": 2999.0,
                            "max": 3999.0
                        }
                    },
                    {
                        "value": "12 MP",
                        "total": 5,
                        "priceRange": {
                            "min": 4199.0,
                            "max": 4799.0
                        }
                    }
                ]
            }
        ],
        "offers": [
            {
                "url": "https://www.shopfacil.com.br/smartphone-apple-iphone-xr-vermelho-128-gb-5049664/p",
                "name": "Smartphone Apple iPhone XR Vermelho 128 GB",
                "available": true,
                "img": "https://shopfacil.vteximg.com.br/arquivos/ids/60111147/5271702_1.jpg?v=637323681408970000",
                "sku": "5271702",
                "price": 3999.0,
                "listPrice": 3999.0,
                "priceDiscount": 0.0,
                "installments": 10,
                "installmentValue": 399.9,
                "score": 76.37367248535156,
                "productId": "5049664",
                "sellerName": "casasbahia",
                "categories": [
                    "/Celular e Smartphone/Smartphone/",
                    "/Celular e Smartphone/"
                ],
                "normalizedCategories": [
                    "/celular-e-smartphone/smartphone/",
                    "/celular-e-smartphone/"
                ],
                "clusters": [
                    "1789",
                    "1446",
                    "1322",
                    "1330"
                ],
                "seller_list": [
                    "Casas Bahia",
                    "Extra",
                    "Fast Shop"
                ]
            }
        ]
    }
}
```
---
