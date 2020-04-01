# Webhook

A integração via Webhooks é a mais indicada, por ser a mais confiável com relação ao sincronismos em "tempo real" entre os produtos da loja para com a base de dados da Omnilogic. Ela basicamente consiste em três componentes

- Webhook Omnilogic para receber notificações de criação/atualização de produtos;
- API da loja para pesquisar todos os dados do produto previamente notificado;
- Webhook de retorno da loja, para o recebimento dos produtos enriquecidos pelo Product Cloud\*

Este ultimo componente só é necessário para os clientes que contrataram os serviços do Product Cloud.

O seguinte diagrama resume bem os sistemas envolvidos nesta integração:

![Integração Webhook](integration/integration-webhook.png)

Um detalhe importante é que o Webhook da Omnilogic, preferencialmente, recebe apenas o ID do produto/oferta que sofreu alguma modificação, para logo após buscar as suas informações em uma API da loja. Esse fluxo foi arquitetado recebendo um ID e não os dados inteiros para possibilitar possíveis ressincronizações sem a necessidade de solicitar um reenvio por conta do cliente.

Entretanto, mudanças nessa estrutura são possíveis, caso essa arquitetura não atenda ao caso de uso da loja.

## Payload de Notificação

O cliente deverá notificar a Omnilogic sobre qualquer tipo de criação/atualização de ofertas, de modo a garantir o sincronismo das bases de dados.

Para isso será necessário realizar uma requisição do tipo POST para a seguinte URL:

https://catalog-integration.omnilogic.com.br/{{STORE}}/offer?key={{KEY}}

Contendo no body um JSON com as seguintes propriedades:

| Propriedade | Tipo   | Descrição    |
| ----------- | ------ | ------------ |
| store       | string | Nome da loja |
| id          | string | ID da oferta |

### Exemplo de payload

```json
{
  "store": "{{STORE}}",
  "id": "{{ID}}"
}
```

## API de detalhamento

Possuindo o ID, a Omnilogic utilizará uma API pública do cliente para obter todas as informações da oferta. Alguns dados que são esperados/desejados dessa API:

| Propriedade       | Tipo     | Descrição                               |
| ----------------- | -------- | --------------------------------------- |
| sku               | string   | ID do SKU                               |
| seller_id\*       | string   | ID do seller                            |
| seller_offer_id\* | string   | ID da oferta no seller                  |
| title             | string   | nome da oferta                          |
| description       | string   | descrição da oferta                     |
| url               | string   | url da oferta                           |
| images            | string[] | lista de imagens da oferta              |
| price             | float    | preço da oferta                         |
| list_price        | float    | preço, sem desconto, da oferta          |
| attributes        | any      | atributos informados pelo seller        |
| ean               | string   | código de barras                        |
| active            | boolean  | indicador se a oferta está ativa ou não |

### Exemplo de payload

```json
{
  "sku": "{{SKU_ID}}",
  "seller_id": "seller",
  "seller_offer_id": "218008100",
  "title": "iPhone 7 Apple 32GB Ouro Rosa 4G Tela 4.7",
  "price": 2999,
  "list_price": 3499.9,
  "description": "...",
  "categories": [{ "id": "TE", "subcategories": [{ "id": "TEIP" }] }],
  "attributes": [],
  "reference": "Câm. 12MP + Selfie 7MP iOS 11 Proc. Chip A10",
  "ean": "0190198067876",
  "images": ["..."],
  "active": true
}
```

## Payload de Retorno

No caso do Product Cloud, existe uma integração inversa, onde o Omnilogic retornar para o cliente uma oferta/produto enriquecido. Para isso, será utilizado um Webhook do cliente para o envio das seguintes informações, _podendo sofrer alterações de acordo com a necessidade do cliente_:

| Propriedade               | Tipo     | Descrição                                                       |
| ------------------------- | -------- | --------------------------------------------------------------- |
| store                     | string   |                                                                 |
| sku                       | string   | ID do SKU                                                       |
| seller_id                 | string   | ID do seller                                                    |
| seller_offer_id           | string   | ID da oferta no seller                                          |
| entity                    | string   | tipo do produto                                                 |
| metadata                  | string   | atributos extraídos                                             |
| category_id               | string   | categoria pai                                                   |
| subcategory_ids           | string[] | sub-categorias                                                  |
| product_hash              | string   | hash agrupador de produtos                                      |
| product_name              | string   | nome do produto                                                 |
| sku_hash                  | string   | hash agrupador de sku                                           |
| sku_name                  | string   | nome do sku                                                     |
| product_matching_metadata | string[] | atributos que são utilizados para o matching de produto         |
| product_name_metadata     | string[] | atributos que são utilizados para a formação do nome do produto |
| sku_metadata              | string[] | atributos que são utilizados para o matching de sku             |
| sku_name_metadata         | string[] | atributos que são utilizados para a formação do nome do sku     |
| filters_metadata          | string[] | atributos que estão configurados como filtros                   |

### Exemplo de payload

**Lembrando que o payload de retorno pode ser adaptado de acordo com as necessidades do cliente**

```json
{
  "store": "{{STORE}}",
  "sku": "{{SKU_ID}}",
  "seller_id": "{{SELLER}}",
  "seller_offer_id": "{{SELLER_OFFER_ID}}",
  "entity": "Microondas",
  "metadata": {
    "Modelo": "MEF41",
    "Marca": "Electrolux",
    "Cor": "Branco",
    "Voltagem": "220V",
    "Capacidade": "31 L"
  },
  "category_id": "{{CATEGORY}}",
  "subcategory_ids": ["{{SUB_CATEGORY}}", "{{SUB_CATEGORY}}"],
  "product_hash": "c7000c500becee4940ae3e225ff2ac67",
  "product_name": "Micro-ondas Electrolux 31L MEF41 Branco",
  "product_matching_metadata": ["Capacidade", "Marca", "Modelo", "Cor"],
  "product_name_metadata": [
    "Material",
    "Capacidade",
    "Tipo",
    "Marca",
    "Características",
    "Linha",
    "Modelo Nominal",
    "Modelo",
    "Cor"
  ],
  "sku_metadata": ["Voltagem"],
  "filters_metadata": [
    "Material",
    "Capacidade",
    "Tipo",
    "Marca",
    "Características",
    "Linha",
    "Modelo Nominal",
    "Modelo",
    "Cor",
    "Voltagem",
    "Acabamento"
  ]
}
```
