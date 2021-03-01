# Integração via navegação do usuário

O script da Omnilogic, incorporado no [passo anterior](tracker), também é capaz de obter os dados dos produtos da loja a partir da navegação dos usuários. Essa é a forma mais simples para realizar o sincronismo do catálogo com a Omnilogic.

Através deste tipo de integração as seguintes informações dos SKUs serão coletadas:

- Nome
- Imagens
- Descrição
- Preço
- Parcelamento
- Ficha Técnica

## Recomendações

Esse tipo de integração é recomendado apenas no início da implantação das soluções do Intelligent Store, e deve ser mantida apenas caso não seja possível implantar a integração via [Webhook](webhook) ou [XML](/xml). Como esse tipo de integração depende da navegação dos usuários para a obtenção dos dados, não é possível garantir que todos os produtos/SKUs da lojam estejam sincronizados, o que pode acarretar na exibição de imagens, nomes e preços desatualizados nas vitrines, e-mails ou resultados de busca.