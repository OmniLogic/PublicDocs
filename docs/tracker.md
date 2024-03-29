# Tracker

Este breve documento explica como instalar o script de tracker e evoluir a inteligência da sua loja. Através da análise do comportamento do consumidor em sua loja, as ferramentas do Intelligent Store da Omnilogic mostrarão automaticamente produtos com maior probabilidade de compra, podendo atuar tanto nas páginas onde ele navega, quanto na captação de vendas por e-mail. 

![Diagrama Fluxo Tracker](assets/integration-tracker.png)

A instalação é bastante simples, bastando seguir o único passo abaixo:
O script abaixo deve ser incluído dentro do <head></head> de todas as páginas navegadas pelos usuários, inclusive checkout e finalização de pedido, alterando `loja.js` pelo código da sua loja gerado pela Omnilogic. Este código foi enviado para você juntamente com este documento. Quando o usuário estiver logado, devem ser informados também nome e e-mail através da variável oppuzUser:

<br />

```js
<script type="text/javascript">
var oppuzUser = {name: "nome_do_usuario", email: "email_do_usuario"};
try
{
    var oppuzJSProtocol = (("https:" == document.location.protocol) ? "https://" : "http://");
    var head= document.getElementsByTagName('head')[0]; 
    var script= document.createElement('script'); 
    script.type= 'text/javascript';
    script.src= oppuzJSProtocol + "www.oppuz.com/script/loja.js";    
    script.async = true; head.appendChild(script); 
} 
catch (e) {}
</script>
```

<br />

Após a inclusão ser feita com sucesso nossa equipe irá configurar o rastreio do comportamento dos usuários. Poderemos capturar os dados disponíveis nos elementos da página, url, query string ou cookies. Caso existam dados necessários para o funcionamento de alguma das ferramentas contratadas, não disponíveis nas páginas, será realizada uma solicitação pela Omnilogic para a disponibilização dos mesmos. 
