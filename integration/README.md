# Integração E-commerce

O tipo de integração com a plataforma da Omnilogic depende das suites contratadas. De forma macro, existem de dois tipos de dados:

- Dados do catálogo: produtos, SKUs e dados enviados pelos sellers;
- Dados de usuários: como produtos visitados, compras, manipulação de carrinho, etc.

## Dados do catálogo

Independente da suite, sendo elas Intelligent Store ou Product Cloud, a Omnilogic precisa possuir o catálogo do cliente em constante sincronia. Para isso recomendamos o tipo de integração via [WebHook](integration/webhook).

Caso esse tipo de integração não atenda o seu caso de uso, disponibilizamos outras formas como:

- XML;
- Tracker capturando produtos visitados;
- Planilhas;

Entretanto não se recomenda nenhum dos métodos acima, e devem ser utilizados em último caso.

## Dados de usuários

Para alimentar o motor de inteligência da Omnilogic, também é necessária a coleta do comportamento dos usuários nas lojas. Isso envolve a obtenção dos seguintes dados:

- Produtos visitados;
- Produtos clicados;
- Produtos colocados no carrinho;
- Produtos comprados;
- Dados do usuário.

Entre outros, dependendo do tipo de integração contratado.

Esses dados são de extrema importância para o correto funcionamento das ferramentas presentes no Intelligent Store.

Para a captura desses dados, recomendamos a instalação de um JavaScript da Omnilogic nas lojas, que chamamos de [Tracker](integration/tracker)
