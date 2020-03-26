# Busca por Campanha

![Resultado Campanha](search-api/campaign.png)

Esse tipo de busca é muito utilizado para a criação de campanhas customizadas. Estas são URLs especiais que são atreladas à uma busca previamente configurada através do Dashboard da Omnilogic.

No exemplo da imagem acima, por exemplo, o e-commerce configurou que a página `/iphone` está relacionado à busca textual por `celular` + o filtro por marca `Apple`.

Para realizar esse tipo de busca, basta chamar o `SearchAPI` com o seguinte input

## Input

```json
{
  "token": "{{TOKEN_LOJA}}",
  "sort": "price.desc",
  "searchPath": "{{URL_PATH}}"
}
```

Sendo a variável `URL_PATH` a URL configurada no Dashboard
