# Requisição

A principal rota do SearchAPI é a `/search`, que é responsável por retornar as ofertas + filtros dado uma busca:

- Textual
- Por categorias
- Por campanhas/coleções

Ela é acessível tanto utilizando o método `POST` quanto `GET`, e em ambos os métodos os parâmetros de entrada seguem o mesmo modelo:

| Campo                  | Tipo                                      | Observação                                                                              |
| ---------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------- |
| token                  | string                                    | Fornecido pela Omnilogic                                                                |
| query                  | object[{ name: string, values: [string]}] | Metadados selecionados pelo usuário durante a busca                                     |
| Page (default: 1)      | int                                       | Número da página                                                                        |
| pageSize (default: 10) | int                                       | Número de itens por página sort String Valores definidos para ordenação de resultados\* |
| selectedSubstantive    | string                                    | Valores definidos para seleção dos metadados a serem retornados\*\*                     |
| source                 | string                                    | Valores definidos para decidir buscar ou não por uma sugestão\*\*\*                     |

## Valores possíveis para sort

- `score.desc` (default)- ordena por relevância / ”mais populares”
- `ord.desc` - ordena por mais vendidos
- `name.desc` - ordena por nome decrescente
- `name.asc` - ordena por nome crescente
- `price.desc` - ordena por preço decrescente
- `price.asc` - ordena por preço crescente

## Valores possíveis para selectedSubstantive

- `none` - (default) retorna os resultados e metadados de todos os substantivos encontrados pela busca
- `first` - retorna apenas os resultados e metadados do primeiro substantivo encontrado pela busca

## Valores possíveis para source

- `page` - (default) realiza a busca utilizando a primeira sugestão
- `input` - exibe as sugestões sem realizar a busca automática

## Resultado

| Campo               | Tipo     | Observação                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| total               | int      | Total de resultados encontrados                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| substantives        | string[] | Lista de substantivos(produtos) encontrados                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| selectedSubstantive | string   | Substantivo selecionado                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| suggestions         | string[] | Valores de busca sugeridos metadata Array(filtros) Filtros disponíveis baseados nos resultados encontrados, contendo: nome, total de resultados com o filtro, valores possíveis para o filtro( valor, total e variação de preço dos resultados para cada filtro)                                                                                                                                                                                                                                      |
| results             | offers[] | Resultados encontrados a partir da busca realizada, contendo: url, nome, disponibilidade, imagem, sku, preço, preço original(listPrice), número máximo de parcelas(installments), valor de cada parcela(installmentValue), relevância do produto na busca realizada(score) , id do produto(productId), descrição do desconto(teaser) ex: “-5% à vista”,porcentagem do desconto (teaser_discount) ex: “5.0”, etiqueta de metadado(label) - disponível apenas para tensão e tamanho - e nome do seller. |
