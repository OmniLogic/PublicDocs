# Seller API

API tem como objetivo facilitar o cadastro/atualização de produtos feita pelo seller nos sistemas de integração dos Marketplaces.

Através de buscas textuais, o seller obterá, de forma estruturada, os atributos/características do produto que ele deseja cadastrar. Desta forma, os produtos já serão cadastrados com os atributos normalizados/enriquecidos.

## Parâmetros de entrada

A [API]("/api-doc/seller-api") recebe os seguintes parâmetros como entrada:

| propriedade | tipo   | opcional |
| ----------- | ------ | -------- |
| name        | string | sim      |
| description | string | sim      |
| ean         | string | sim      |
| brand       | string | sim      |

Todos são opcionais e devem ser enviado na forma de um JSON. Segue exemplo:

```json
{
  "name": "Relógio",
  "brand": "Condor"
}
```

ou uma busca direto pelo EAN

```json
{
  "ean": "7891530575327"
}
```

## Retorno

O retorno da API apresenta todos os atributos extraídos do texto de entrada do usuário, e uma lista de todos os atributos relevantes para aquela entidade.

| propriedade             | tipo     | descrição                                         |
| ----------------------- | -------- | ------------------------------------------------- |
| niche                   | string   | nicho identificado                                |
| metadata                | object   | atributos extraídos da oferta                     |
| technical_specification | string[] | atributos que serão utilizados como ficha técnica |

Segue um exemplo de payload de retorno:

```json
{
  "niche": "vestuario",
  "metadata": {
    "Modelo": "CO2035KYH",
    "Marca": "Condor",
    "Cor": "Prata",
    "Material": "Aço",
    "Gênero": "Feminino",
    "Entidade": "Relógio",
    "Características": "Resistente à água"
  },
  "technical_specification": [
    "Entidade",
    "Marca",
    "Linha de Produto",
    "Modelo",
    "Cor",
    "Gênero",
    "Mecanismo",
    "Características",
    "Estilo",
    "Classificação",
    "Material",
    "Tipo"
  ]
}
```

## Exemplos

Acesse a página de [especificações]("/api-doc/seller-api") para mais exemplos e um detalhamento melhor com relação à URL de acesso, autenticação e retornos.
