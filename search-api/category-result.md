# Busca por Categoria

![Resultado Categorias](/search-api/categories.jpg)

Esse tipo de busca é muito utilizado em páginas de departamentos ou categorias. Para realizar esse tipo de busca, basta chamar o `SearchAPI` com o seguinte input

## Input

```json
{
  "token": "{{TOKEN_LOJA}}",
  "sort": "price.desc",
  "categories": ["{{CATEGORIA_LOJA}}"]
}
```

Sendo a variável `CATEGORIA_LOJA` um nome de departamento/categoria do e-commerce.
