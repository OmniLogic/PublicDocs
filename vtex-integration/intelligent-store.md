# Integração Intelligent Store

## Afiliados

Para iniciarmos a integração com a VTEX é necessário possuirmos alguns usuários no portal administrativo com permissão de acesso ao módulo de OMS. 

Este módulo nos possibilitará receber notificações indicando a criação ou atualização de um SKU na loja. Fluxo de extrema importância, uma vez que precisamos garantir que temos todos os SKUs atualizados em nossa estrutura interna, para a realização da recomendação correta das ofertas disponíveis vindas dos sellers.

Para nos dar acesso à esse módulo basta cadastrar uma URL interna nossa. Basta seguir este tutorial: 
[https://help.vtex.com/pt/tutorial/como-configurar-afiliado](https://help.vtex.com/pt/tutorial/como-configurar-afiliado)

Utilizando os seguintes parâmetros:

Name: Omnilogic
E-mail for notification: techadmin@omnilogic.com.br                                                     
Endpoint: https://vtex.oppuz.com/api/notification                                                                     
Use my payment method: desmarcado

## Chaves de acesso VTEX

Precisamos também de duas chaves chamadas de appKey e appToken, da parte do License Manager. 

Elas podem ser geradas seguindo os seguinte tutorial: 

[https://help.vtex.com/pt/tutorial/criar-appkey-e-apptoken-para-autenticar-integracoes](https://help.vtex.com/pt/tutorial/criar-appkey-e-apptoken-para-autenticar-integracoes)