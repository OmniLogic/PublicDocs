# Autorização

A API possui dois tipos de autenticação, um para acessar os resultados de busca e outro para administrar as configurações específicas de cada loja, como sinônimos, redirecionamento e campanhas.

Ambos devem ser solicitados através do nosso e-mail comercial sales@omnilogic.ai. Eles te ajudaram a configurar a [integração](/integration) e liberar todos os acessos necessários

## Token Resultado de Busca

Estas rotas possui dois métodos de autenticação. Dependendo do tipo de integração com a sua loja, ela pode ser realizada através do envio de uma propriedade no payload da requisição:

```json
{
    "token": {{TOKEN_LOJA}},
    ...
}
```

ou no header da requisição:

```
Authorization: Bearer {{TOKEN_LOJA}}
```

## Token Administração

As rotas que manipulam configurações de cada loja são protegidas e devem ser acessadas utilizando o header `Authorization`, como explicado no exemplo anterior. Uma diferença importante é que o `TOKEN LOJA` nesse caso não é o mesmo token que foi utilizado para obter os resultados de busca. Caso você não possua esse token, entre em contato com o nosso suporte, para solicitar a sua criação
