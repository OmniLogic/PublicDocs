# Seller API

Esta API tem como objetivo facilitar o cadastro/atualização de produtos feita pelo seller nos sistemas de integração dos Marketplaces.

Através de buscas textuais, o seller será capaz de encontrar ofertas similares às que ele deseja cadastrar podendo, por exemplo, aproveitar diversos atributos já existentes. Desta forma, os produtos já serão cadastrados com as melhores imagens, títulos padronizados, atributos normalizados/enriquecidos e até o matching já realizado.

## Parâmetros de entrada

A [API]("/api-doc/seller-api") recebe os seguintes parâmetros como entrada:

| propriedade | tipo                                    | opcional |
| ----------- | --------------------------------------- | -------- |
| name        | string                                  | sim      |
| description | string                                  | sim      |
| ean         | string                                  | sim      |
| metadata    | object[{ name: string, value: string }] | sim      |

Todos são opcionais e devem ser enviado na forma de um JSON. Segue exemplo:

```json
{
  "name": "Relógio",
  "metadata": [
    {
      "name": "Marca",
      "value": "Condor"
    }
  ]
}
```

ou uma busca direto pelo EAN

```json
{
  "ean": "7891530575327"
}
```

## Retorno

O retorno da API apresenta uma lista das ofertas mais similares à busca realizada. Cada oferta apresenta as seguintes propriedades:

| propriedade             | tipo     | descrição                                         |
| ----------------------- | -------- | ------------------------------------------------- |
| name                    | string   | nome da oferta                                    |
| description             | string   | descrição da oferta                               |
| ean                     | string   |                                                   |
| images                  | string[] | lista de imagens da oferta                        |
| metadata                | object   | atributos extraídos da oferta                     |
| technical_specification | string[] | atributos que serão utilizados como ficha técnica |

Segue um exemplo de payload de retorno:

```json
[
  {
    "name": "Relogio Feminino Prata Condor Pequeno CO2035KYH/3K",
    "description": "Informações Básicas\nMarca: Condor \nReferência: CO2035KYH/3K\nGênero: Feminino\nGarantia: 1 Ano em Toda Rede de Assistência Técnica Condor no Brasil\n \nCaracterísticas\nComposição do Vidro: Cristal Mineral\nMaterial da Caixa: Metal \nCor da Caixa: Prata \nMaterial da Pulseira: Aço Inoxidável\nCor da Pulseira: Prata \nMostrador: Fundo Prata\n \nMedidas\nDiâmetro da Caixa: 2,5 cm\nEspessura da Caixa: 0,8 cm\n \nResistência a Água: 5 ATM/ 50 Mts\nPode Lavar as Mãos: Sim\nPode Usar em Água Corrente: Sim \nPode Usar na Piscina: Não\nPode Usar em Mergulho Profundo: Não\n \nAcompanha\nNota Fiscal\nManual de Instruções\nCertificado de Garantia Condor \nCaixa Original da Condor",
    "ean": "7891530575327",
    "images": [
      "https://olist-v2-dev.s3.amazonaws.com/products-images/bb6f5b058fca47d50849a499bf086002269f7638.jpeg"
    ],
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
  },
  {
    "name": "Relogio Condor CO2035KRJ/4C",
    "description": "Informações :\r\n- Marca: Condor\r\n- Referência: CO2035KRJ/4C\r\n- Genêro: Feminino\r\n- Garantia: 1 Ano em Toda Rede de Assistência Técnica no Brasil\r\n\r\nCaracterísticas\r\n\r\n- Composição do Vidro: Cristal Mineral\r\n- Material da Caixa: Metal\r\n- Cor da Caixa: Dourado\r\n- Material da Pulseira: Aço \r\n- Cor da Pulseira: Dourado\r\n\r\nMedidas\r\n\r\n- Diâmetro da Caixa: 4,0 cm\r\n\r\nResistência a Água\r\n\r\n- WR: 5 ATM\r\n- Pode Lavar as Mãos: SIM\r\n- Pode Usar em Água Corrente: SIM\r\n- Pode Usar na Piscina: NÃO\r\n- Pode Usar em Mergulho Profundo: NÃO",
    "ean": "7891530431142",
    "images": [
      "https://olist-v2-dev.s3.amazonaws.com/products-images/e5700df6-115e-46b2-9c6b-44931d0f3c7d.jpg"
    ],
    "metadata": {
      "Modelo": "CO2035KRJ",
      "Marca": "Condor",
      "Cor": "Dourado",
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
]
```

## Exemplos

Acesse a página de [especificações]("/api-doc/seller-api") para mais exemplos e um detalhamento melhor com relação à URL de acesso, autenticação e retornos.
