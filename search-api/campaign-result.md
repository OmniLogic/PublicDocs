# Busca Textual

Esse tipo de busca é muito utilizado em caixas de busca localizadas no topo das páginas do e-commerce. Para utilizar a pesquisa que o usuário está realizando, basta chamar o `SearchAPI` com o seguinte input

## Input

```json
{
  "token": "{{TOKEN_LOJA}}",
  "sort": "price.desc",
  "query": [{ "name": "other_details", "values": ["{{BUSCA_DO_USUÁIO}}"] }]
}
```
