# Busca por Coleção

![Result Coleção](search-api/collection.jpg)

Esse tipo de busca é muito utilizado em páginas de coleções. Estes são agrupamentos por SKUs, previamente configurados através do Dashboard da Omnilogic.

Para realizar esse tipo de busca, basta chamar o `SearchAPI` com o seguinte input

## Input

```json
{
  "token": "{{TOKEN_LOJA}}",
  "sort": "price.desc",
  "clusters": ["{{CLUSTER}}"]
}
```

Sendo a variável `CLUSTER`, um ID de cluster previamente cadastrado.
