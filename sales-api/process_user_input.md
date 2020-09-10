# /process-user-input

## Description

Returns structured data from user shopping intention and optionally searches for offers.

**API Endpoint:**
```http
POST /process-user-input
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
| `previous_context` | `string` | **Optional.** Previously detected relevant fragment (to be used when aggregating information). See examples below.|
| `sort_behavior` | `string` | **Optional.** Describes the sorting method used for searching offers. See possible values below.|
| `price_range` | `Array[float, float]` | **Optional.** Array with min_price and max_price. This is used as a filter for searching offers. |
| `input_text` | `string` | **Required.** User input from an assistant / bot (when asked for his/her shopping intention)|
| `extraction_probability_threshold` | `float` | **Default: `0.6`** Minimum probability value to consider entity extraction as trustworthy.|
| `enable_search` | `boolean` | **Default: `true`**. Enables / disables offers search. |
| `search_size` | `integer` | **Default: `20`**. Maximum number of offers to be returned if `enable_search` is set to `true`. |
| `user_email` | `string` | **Optional.** User e-mail address. |
| `user_phone` | `string` | **Optional.** User phone number. |

**Possible `sort_behavior` values:**

| Value | Description |
| :--- | :--- |
| `best_selling` | Best selling products will have priority |
| `most_relevant` | Most popular products (most seen) will have priority |


Example:

```json
{
    "input_text": "quero uma televisão samsung de 3000 reais",
    "extraction_probability_threshold": 0.6,
    "enable_search": true,
    "search_size": 1,
    "user_email": "omnilogic@omnilogic.ai",
    "user_phone": "5531999999999"
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

There is a `successful_entity_extraction` field inside `structured_data` that indicates whether or not it was possible to extract the entity from `relevant_fragment`.

It is important to note the `entity_filtered` and `metadata_filtered` fields inside `search_result`. They are booleans that indicate whether or not the entity and the metadata extracted were used to build the search query.

The `available_metadata_filters` field inside `search_result` contains an array of available filters and its values.

Example:

```json
{
    "user_input": "televisão samsung de 3000 reais",
    "spellchecked_user_input": "televisão samsung de 3000 reais",
    "previous_context": null,
    "relevant_fragment": "tv samsung",
    "structured_data": {
        "successful_entity_extraction": true,
        "niche": "eletronicos",
        "entity": "TV",
        "probability": "0.99999833",
        "metadata": {
            "brand": "Samsung"
        },
        "price_range": [
            0,
            3000
        ],
        "sort_behavior": null
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
        "total_available": 9,
        "search_size": 1,
        "entity_filtered": true,
        "metadata_filtered": true,
        "available_metadata_filters": [
            {
                "name": "substantive",
                "translation": "Produto",
                "total": 9,
                "values": [
                    {
                        "value": "TV",
                        "total": 9,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2949.0
                        }
                    }
                ]
            },
            {
                "name": "resources",
                "translation": "Recursos",
                "total": 22,
                "values": [
                    {
                        "value": "Smart",
                        "total": 5,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2849.05
                        }
                    },
                    {
                        "value": "DLNA",
                        "total": 3,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2849.05
                        }
                    },
                    {
                        "value": "Sensor de luminosidade inteligente",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Modo Ambiente",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Amplificador automático de voz",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Guia de voz",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Modo Arte The Frame",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Busca automática de canais",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Desligamento Automático",
                        "total": 1,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 1249.0
                        }
                    },
                    {
                        "value": "Espelhamento",
                        "total": 1,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 1249.0
                        }
                    }
                ]
            },
            {
                "name": "brand",
                "translation": "Marca",
                "total": 9,
                "values": [
                    {
                        "value": "Samsung",
                        "total": 9,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2949.0
                        }
                    }
                ]
            },
            {
                "name": "screen_size",
                "translation": "Tamanho da Tela",
                "total": 9,
                "values": [
                    {
                        "value": "32\"",
                        "total": 1,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 1249.0
                        }
                    },
                    {
                        "value": "43\"",
                        "total": 3,
                        "priceRange": {
                            "min": 2098.0,
                            "max": 2305.0
                        }
                    },
                    {
                        "value": "50\"",
                        "total": 2,
                        "priceRange": {
                            "min": 2250.55,
                            "max": 2949.0
                        }
                    },
                    {
                        "value": "55\"",
                        "total": 3,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2849.05
                        }
                    }
                ]
            },
            {
                "name": "operating_system",
                "translation": "Sistema Operacional",
                "total": 9,
                "values": [
                    {
                        "value": "Tizen",
                        "total": 9,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2949.0
                        }
                    }
                ]
            },
            {
                "name": "type_",
                "translation": "Tipo",
                "total": 9,
                "values": [
                    {
                        "value": "Smart TV",
                        "total": 9,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2949.0
                        }
                    }
                ]
            },
            {
                "name": "voltage",
                "translation": "Voltagem",
                "total": 9,
                "values": [
                    {
                        "value": "Bivolt",
                        "total": 9,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2949.0
                        }
                    }
                ]
            },
            {
                "name": "resolution",
                "translation": "Resolução",
                "total": 9,
                "values": [
                    {
                        "value": "4K",
                        "total": 8,
                        "priceRange": {
                            "min": 2098.0,
                            "max": 2949.0
                        }
                    },
                    {
                        "value": "HD",
                        "total": 1,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 1249.0
                        }
                    }
                ]
            },
            {
                "name": "screen_type",
                "translation": "Tipo de Tela",
                "total": 8,
                "values": [
                    {
                        "value": "LED",
                        "total": 7,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 2849.05
                        }
                    },
                    {
                        "value": "QLED",
                        "total": 1,
                        "priceRange": {
                            "min": 2949.0,
                            "max": 2949.0
                        }
                    }
                ]
            },
            {
                "name": "model",
                "translation": "Modelo",
                "total": 6,
                "values": [
                    {
                        "value": "UN55TU8000GXZD",
                        "total": 2,
                        "priceRange": {
                            "min": 2706.55,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "UN50TU8000GXZD",
                        "total": 1,
                        "priceRange": {
                            "min": 2250.55,
                            "max": 2250.55
                        }
                    },
                    {
                        "value": "UN43TU7000GXZD",
                        "total": 1,
                        "priceRange": {
                            "min": 2305.0,
                            "max": 2305.0
                        }
                    },
                    {
                        "value": "UN32T4300AGXZD",
                        "total": 1,
                        "priceRange": {
                            "min": 1249.0,
                            "max": 1249.0
                        }
                    },
                    {
                        "value": "UN55RU7100GXZD",
                        "total": 1,
                        "priceRange": {
                            "min": 2849.05,
                            "max": 2849.05
                        }
                    }
                ]
            }
        ],
        "offers": [
            {
                "url": "https://www.shopfacil.com.br/smart-tv-4k-samsung-qled-50-com-plataforma-tizen-controle-remoto-unico-e-wi-fi---qn50q60tagxzd-1171356875/p",
                "name": "Smart TV 4K Samsung QLED 50 com Plataforma Tizen, Controle Remoto Único e Wi-Fi - QN50Q60TAGXZD BIVOLT",
                "available": true,
                "img": "https://shopfacil.vteximg.com.br/arquivos/ids/61232903/7050096_1.jpg?v=637334373280170000",
                "sku": "7050096",
                "price": 2949.0,
                "listPrice": 2949.0,
                "priceDiscount": 0.0,
                "installments": 1,
                "installmentValue": 2949.0,
                "score": 2.8230502605438232,
                "productId": "1171356875",
                "sellerName": "004486803",
                "categories": [
                    "/Eletrônicos/TV/TV 4K/",
                    "/Eletrônicos/TV/",
                    "/Eletrônicos/"
                ],
                "normalizedCategories": [
                    "/eletronicos/tv/tv-4k/",
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

### Demonstrating the use of `previous_context`, `sort_behavior` and `price_range`:

**Let's imagine the following conversation:**

**Bot:** O que você deseja comprar?

**User:** Quero o celular da samsung mais vendido de até 3000 reais.

**Bot:** Gostaria de escolher uma cor?

**User:** Pode ser um preto.


*First interaction:*

Request Body:

```json
{
    "input_text": "Quero o celular da samsung mais vendido de até 3000 reais.",
    "enable_search": false
}
```

Response Body:

```json
{
    "user_input": "Quero o celular da samsung mais vendido de até 3000 reais..",
    "spellchecked_user_input": "Quero o celular da samsung mais vendido de até 3000 reais..",
    "previous_context": null,
    "relevant_fragment": "celular samsung",
    "structured_data": {
        "successful_entity_extraction": true,
        "niche": "eletronicos",
        "entity": "Celular",
        "probability": "0.9999701",
        "metadata": {
            "brand": "Samsung"
        },
        "price_range": [
            0,
            3000
        ],
        "sort_behavior": "best_selling"
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
Also the previously detected `price_range` and `sort_behavior` are now passed as a parameters. 

Request Body:

```json
{
    "previous_context": "celular samsung",
    "sort_behavior": "best_selling",
    "price_range": [0, 3000],
    "input_text": "Pode ser um preto.",
    "enable_search": true,
    "search_size": 1
}
```

Response Body:

```json
{
    "user_input": "Pode ser um preto.",
    "spellchecked_user_input": "Pode ser um preto.",
    "previous_context": "celular samsung",
    "relevant_fragment": "celular samsung preto",
    "structured_data": {
        "successful_entity_extraction": true,
        "niche": "eletronicos",
        "entity": "Celular",
        "probability": "0.99996364",
        "metadata": {
            "brand": "Samsung",
            "color": "Preto"
        },
        "price_range": [
            0.0,
            3000.0
        ],
        "sort_behavior": "best_selling"
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
        "total_available": 31,
        "search_size": 1,
        "entity_filtered": true,
        "metadata_filtered": true,
        "available_metadata_filters": [
            {
                "name": "substantive",
                "translation": "Produto",
                "total": 31,
                "values": [
                    {
                        "value": "Celular",
                        "total": 31,
                        "priceRange": {
                            "min": 757.15,
                            "max": 2944.05
                        }
                    }
                ]
            },
            {
                "name": "connectivity",
                "translation": "Conectividade",
                "total": 76,
                "values": [
                    {
                        "value": "4G",
                        "total": 29,
                        "priceRange": {
                            "min": 757.15,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "WiFi",
                        "total": 24,
                        "priceRange": {
                            "min": 914.3,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "Bluetooth",
                        "total": 14,
                        "priceRange": {
                            "min": 1049.0,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "Bluetooth 5.0",
                        "total": 7,
                        "priceRange": {
                            "min": 914.3,
                            "max": 1899.05
                        }
                    },
                    {
                        "value": "Bluetooth 4.2",
                        "total": 2,
                        "priceRange": {
                            "min": 1255.8,
                            "max": 1399.0
                        }
                    }
                ]
            },
            {
                "name": "camera_resolution",
                "translation": "Resolução de Câmera",
                "total": 63,
                "values": [
                    {
                        "value": "2 MP",
                        "total": 9,
                        "priceRange": {
                            "min": 914.3,
                            "max": 1349.0
                        }
                    },
                    {
                        "value": "2.0 MP",
                        "total": 1,
                        "priceRange": {
                            "min": 1399.0,
                            "max": 1399.0
                        }
                    },
                    {
                        "value": "5 MP",
                        "total": 13,
                        "priceRange": {
                            "min": 1049.0,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "5.0 MP",
                        "total": 1,
                        "priceRange": {
                            "min": 1399.0,
                            "max": 1399.0
                        }
                    },
                    {
                        "value": "8 MP",
                        "total": 4,
                        "priceRange": {
                            "min": 757.15,
                            "max": 1499.0
                        }
                    },
                    {
                        "value": "12 MP",
                        "total": 8,
                        "priceRange": {
                            "min": 1839.0,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": " 12 MP",
                        "total": 3,
                        "priceRange": {
                            "min": 2399.0,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "13 MP",
                        "total": 9,
                        "priceRange": {
                            "min": 914.3,
                            "max": 1255.8
                        }
                    },
                    {
                        "value": "13.0 MP",
                        "total": 1,
                        "priceRange": {
                            "min": 1399.0,
                            "max": 1399.0
                        }
                    },
                    {
                        "value": " 16 MP",
                        "total": 1,
                        "priceRange": {
                            "min": 2944.05,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "16 MP",
                        "total": 3,
                        "priceRange": {
                            "min": 1959.99,
                            "max": 2518.5
                        }
                    },
                    {
                        "value": "25 MP",
                        "total": 1,
                        "priceRange": {
                            "min": 1499.0,
                            "max": 1499.0
                        }
                    },
                    {
                        "value": "32MP",
                        "total": 2,
                        "priceRange": {
                            "min": 1899.05,
                            "max": 2279.05
                        }
                    },
                    {
                        "value": "48 MP",
                        "total": 4,
                        "priceRange": {
                            "min": 1349.0,
                            "max": 2055.0
                        }
                    },
                    {
                        "value": " 48 MP",
                        "total": 2,
                        "priceRange": {
                            "min": 2399.0,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "64 MP",
                        "total": 1,
                        "priceRange": {
                            "min": 2073.39,
                            "max": 2073.39
                        }
                    }
                ]
            },
            {
                "name": "brand",
                "translation": "Marca",
                "total": 31,
                "values": [
                    {
                        "value": "Samsung",
                        "total": 31,
                        "priceRange": {
                            "min": 757.15,
                            "max": 2944.05
                        }
                    }
                ]
            },
            {
                "name": "capacity",
                "translation": "Capacidade",
                "total": 31,
                "values": [
                    {
                        "value": "16 GB",
                        "total": 4,
                        "priceRange": {
                            "min": 757.15,
                            "max": 1757.41
                        }
                    },
                    {
                        "value": "32 GB",
                        "total": 10,
                        "priceRange": {
                            "min": 914.3,
                            "max": 2518.5
                        }
                    },
                    {
                        "value": "64 GB",
                        "total": 5,
                        "priceRange": {
                            "min": 1255.8,
                            "max": 1899.05
                        }
                    },
                    {
                        "value": "128 GB",
                        "total": 12,
                        "priceRange": {
                            "min": 1499.0,
                            "max": 2944.05
                        }
                    }
                ]
            },
            {
                "name": "color",
                "translation": "Cor",
                "total": 31,
                "values": [
                    {
                        "value": "Preto",
                        "total": 31,
                        "priceRange": {
                            "min": 757.15,
                            "max": 2944.05
                        }
                    }
                ]
            },
            {
                "name": "name",
                "translation": "Nome",
                "total": 31,
                "values": [
                    {
                        "value": "Galaxy A10S",
                        "total": 6,
                        "priceRange": {
                            "min": 914.3,
                            "max": 1099.0
                        }
                    },
                    {
                        "value": "Galaxy A51",
                        "total": 3,
                        "priceRange": {
                            "min": 1839.0,
                            "max": 2055.0
                        }
                    },
                    {
                        "value": "Galaxy A71",
                        "total": 2,
                        "priceRange": {
                            "min": 2073.39,
                            "max": 2279.05
                        }
                    },
                    {
                        "value": "Galaxy A11",
                        "total": 2,
                        "priceRange": {
                            "min": 1255.8,
                            "max": 1399.0
                        }
                    },
                    {
                        "value": "Galaxy A21S",
                        "total": 2,
                        "priceRange": {
                            "min": 1349.0,
                            "max": 1899.05
                        }
                    },
                    {
                        "value": "Galaxy Note 10 Lite",
                        "total": 2,
                        "priceRange": {
                            "min": 2065.0,
                            "max": 2089.05
                        }
                    },
                    {
                        "value": "Galaxy S10 Lite",
                        "total": 2,
                        "priceRange": {
                            "min": 2399.0,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "Galaxy A20S",
                        "total": 1,
                        "priceRange": {
                            "min": 1049.0,
                            "max": 1049.0
                        }
                    },
                    {
                        "value": "Galaxy A30S",
                        "total": 1,
                        "priceRange": {
                            "min": 1499.0,
                            "max": 1499.0
                        }
                    },
                    {
                        "value": "Galaxy S10",
                        "total": 1,
                        "priceRange": {
                            "min": 2944.05,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "Galaxy A31",
                        "total": 1,
                        "priceRange": {
                            "min": 1499.0,
                            "max": 1499.0
                        }
                    },
                    {
                        "value": "Galaxy A5 2016",
                        "total": 1,
                        "priceRange": {
                            "min": 1757.41,
                            "max": 1757.41
                        }
                    },
                    {
                        "value": "Galaxy A5 2017",
                        "total": 1,
                        "priceRange": {
                            "min": 1994.05,
                            "max": 1994.05
                        }
                    },
                    {
                        "value": "Galaxy E7",
                        "total": 1,
                        "priceRange": {
                            "min": 1175.02,
                            "max": 1175.02
                        }
                    },
                    {
                        "value": "Galaxy J2",
                        "total": 1,
                        "priceRange": {
                            "min": 757.15,
                            "max": 757.15
                        }
                    },
                    {
                        "value": "Galaxy S6 Edge",
                        "total": 1,
                        "priceRange": {
                            "min": 2518.5,
                            "max": 2518.5
                        }
                    },
                    {
                        "value": "Galaxy M31",
                        "total": 1,
                        "priceRange": {
                            "min": 1899.05,
                            "max": 1899.05
                        }
                    },
                    {
                        "value": "Galaxy A5",
                        "total": 1,
                        "priceRange": {
                            "min": 1253.91,
                            "max": 1253.91
                        }
                    },
                    {
                        "value": "Galaxy S6",
                        "total": 1,
                        "priceRange": {
                            "min": 1959.99,
                            "max": 1959.99
                        }
                    }
                ]
            },
            {
                "name": "screen_size",
                "translation": "Tamanho da Tela",
                "total": 30,
                "values": [
                    {
                        "value": "5\"",
                        "total": 1,
                        "priceRange": {
                            "min": 757.15,
                            "max": 757.15
                        }
                    },
                    {
                        "value": "5.0\"",
                        "total": 1,
                        "priceRange": {
                            "min": 1959.99,
                            "max": 1959.99
                        }
                    },
                    {
                        "value": "5.1\"",
                        "total": 1,
                        "priceRange": {
                            "min": 2518.5,
                            "max": 2518.5
                        }
                    },
                    {
                        "value": "5.2\"",
                        "total": 3,
                        "priceRange": {
                            "min": 1253.91,
                            "max": 1994.05
                        }
                    },
                    {
                        "value": "5.5 polegadas",
                        "total": 1,
                        "priceRange": {
                            "min": 1175.02,
                            "max": 1175.02
                        }
                    },
                    {
                        "value": "6.1\"",
                        "total": 1,
                        "priceRange": {
                            "min": 2944.05,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "6.2\"",
                        "total": 6,
                        "priceRange": {
                            "min": 914.3,
                            "max": 1099.0
                        }
                    },
                    {
                        "value": "6.4\"",
                        "total": 5,
                        "priceRange": {
                            "min": 1255.8,
                            "max": 1899.05
                        }
                    },
                    {
                        "value": "6.5\"",
                        "total": 6,
                        "priceRange": {
                            "min": 1049.0,
                            "max": 2055.0
                        }
                    },
                    {
                        "value": "6.7\"",
                        "total": 5,
                        "priceRange": {
                            "min": 2065.0,
                            "max": 2799.0
                        }
                    }
                ]
            },
            {
                "name": "ram_memory",
                "translation": "Memória RAM",
                "total": 29,
                "values": [
                    {
                        "value": "2 GB",
                        "total": 8,
                        "priceRange": {
                            "min": 914.3,
                            "max": 1757.41
                        }
                    },
                    {
                        "value": "6 GB",
                        "total": 7,
                        "priceRange": {
                            "min": 1899.05,
                            "max": 2799.0
                        }
                    },
                    {
                        "value": "4 GB",
                        "total": 7,
                        "priceRange": {
                            "min": 1349.0,
                            "max": 2055.0
                        }
                    },
                    {
                        "value": "3 GB",
                        "total": 5,
                        "priceRange": {
                            "min": 1049.0,
                            "max": 1994.05
                        }
                    },
                    {
                        "value": "8 GB",
                        "total": 1,
                        "priceRange": {
                            "min": 2944.05,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "1 GB",
                        "total": 1,
                        "priceRange": {
                            "min": 757.15,
                            "max": 757.15
                        }
                    }
                ]
            },
            {
                "name": "type_",
                "translation": "Tipo",
                "total": 27,
                "values": [
                    {
                        "value": "Smartphone",
                        "total": 26,
                        "priceRange": {
                            "min": 757.15,
                            "max": 2944.05
                        }
                    },
                    {
                        "value": "Smatphone",
                        "total": 1,
                        "priceRange": {
                            "min": 1499.0,
                            "max": 1499.0
                        }
                    }
                ]
            }
        ],
        "offers": [
            {
                "url": "https://www.shopfacil.com.br/smartphone-samsung-galaxy-a10s-32gb-preto-4g---octa-core-2gb-ram-62-quot--cam--dupla---selfie-8mp-1171379646/p",
                "name": "Smartphone Samsung Galaxy A10s 32GB Preto 4G - Octa-Core 2GB RAM 6,2&quot; Câm. Dupla + Selfie 8MP",
                "available": true,
                "img": "https://shopfacil.vteximg.com.br/arquivos/ids/54426209/7072954_1.jpg?v=637305317612800000",
                "sku": "7072954",
                "price": 949.05,
                "listPrice": 949.05,
                "priceDiscount": 0.0,
                "installments": 10,
                "installmentValue": 94.91,
                "score": 2.6605846881866455,
                "productId": "1171379646",
                "sellerName": "004443713",
                "categories": [
                    "/Celular e Smartphone/Celular/",
                    "/Celular e Smartphone/"
                ],
                "normalizedCategories": [
                    "/celular-e-smartphone/celular/",
                    "/celular-e-smartphone/"
                ],
                "clusters": [
                    "1815"
                ],
                "seller_list": [
                    "Magazine Luiza"
                ]
            }
        ]
    }
}
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

---
