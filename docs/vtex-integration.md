# Integração VTEX

Para iniciarmos a integração com a VTEX é necessário possuirmos alguns usuários no portal administrativo com permissão de acesso ao módulo de OMS. 
Este módulo nos possibilitará receber notificações indicando a criação ou atualização de um SKU na loja. Fluxo de extrema importância, uma vez que precisamos garantir que temos todos os SKUs atualizados em nossa estrutura interna, para a realização da recomendação correta das ofertas disponíveis.

## Configuração Afiliados

Para nos dar acesso à esse módulo basta cadastrar uma URL interna nossa, seguindo o passo-a-passo deste tutorial: 
https://help.vtex.com/pt/tutorial/como-configurar-afiliado

Utilizando os seguintes parâmetros:
- Name: Omnilogic                                                                                                                        
- E-mail for notification: techadmin@omnilogic.com.br                                                     
- Endpoint: https://vtex.oppuz.com/api/notification                                                                     
- Use my payment method: desmarcado

## Chaves VTEX

Precisamos também de duas chaves chamadas de `appKey` e `appToken`, da parte do License Manager. Elas podem ser geradas seguindo os seguinte tutorial: 
https://help.vtex.com/pt/tutorial/criar-appkey-e-apptoken-para-autenticar-integracoes

## Sincronismo

Para garantir que todos os produtos/SKUs estejam sincronizados, recomendamos a realização de uma `reindexação completa` do catálogo da vtex. Para realizar esta operação, basta seguir as instruções presentes no item 6 do seguinte tutorial https://help.vtex.com/pt/tutorial/entendendo-a-manutencao-da-base-de-dados

## Suggestions e Catálogo (Product Cloud)

De posse do `appKey` e `appToken` seremos capazes de realizar tanto a configuração de um novo `Matcher` para o módulo do `Suggestions` quanto começar o trabalho de enriquecimento do catálogo da loja