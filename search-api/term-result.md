# Busca Textual

Esse tipo de busca é muito utilizado em caixas de busca localizadas no topo das páginas do e-commerce. Para utilizar a pesquisa que o usuário está realizando, basta chamar o `SearchAPI` com o seguinte input

```json
{
  "token": "{{TOKEN_LOJA}}",
  "sort": "price.desc",
  "query": [{ "name": "other_details", "values": ["{{BUSCA_DO_USUÁIO}}"] }]
}
```

Esta tipo de busca irá retornar um payload no seguinte formato:

```json
{
  "total": 4,
  "substantives": ["Celular"],
  "selectedSubstantive": null,
  "suggestions": [],
  "metadata": [
    {
      "name": "substantive",
      "total": 4,
      "unit": null,
      "values": [
        {
          "value": "Celular",
          "total": 4,
          "priceRange": {
            "min": 2359.35,
            "max": 2549
          }
        }
      ]
    },
    {
      "name": "brand",
      "total": 4,
      "unit": null,
      "values": [
        {
          "value": "Samsung",
          "total": 4,
          "priceRange": {
            "min": 2359.35,
            "max": 2549
          }
        }
      ]
    },
    {
      "name": "capacity",
      "total": 4,
      "unit": null,
      "values": [
        {
          "value": "32GB",
          "total": 4,
          "priceRange": {
            "min": 2359.35,
            "max": 2549
          }
        }
      ]
    }
  ],
  "results": [
    {
      "url": "https://omnilogic.com.br/smartphone-samsung-123",
      "name": "Smartphone Samsung Galaxy S7 Edge",

      "available": true,
      "img": "https://omnilogic.com.br/produto/celular-123.jpg",
      "sku": "12555",
      "price": 2549,
      "listPrice": 3499,
      "installments": 10,
      "installmentValue": 254.9,
      "score": 7.787473201751709,
      "productId": "4186737",
      "teaser": null,
      "teaser_discount": null,
      "sellerName": "seller1",
      "label": null
    }
  ]
}
```

Com este retorno, é possível, por exemplo, exibir todos os filtros presentes no campo `metadata`, possibilitando que ele especifique mais a sua busca utilizando característica reais dos produtos. Para executar esse filtro, basta requisitar novamente o `SearchAPI` informando o filtro selecionado:

```json
{
  "token": "{{TOKEN_LOJA}}",
  "sort": "price.desc",
  "query": [
    { "name": "other_details", "values": ["{{BUSCA_DO_USUÁIO}}"] },
    { "name": "{{FILTRO}}", "values": ["{{VALOR_SELECIONADO}}"] }
  ]
}
```
