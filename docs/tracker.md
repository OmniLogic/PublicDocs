# Tracker

![Diagrama Fluxo Tracker](/integration/integration-tracker.png)

O script abaixo deve ser incluído dentro do `<head></head>` de todas as páginas navegadas pelos usuários, inclusive checkout e finalização de pedido, trocando a variável `{{LOJA}}` peelo código da sua loja gerado pela Omnilogic.

```html
<script type="text/javascript">
  try {
    var oppuzJSProtocol =
      'https:' == document.location.protocol ? 'https://' : 'http://';
    var head = document.getElementsByTagName('head')[0];
    var script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = oppuzJSProtocol + 'www.oppuz.com/script/dito/{{LOJA}}.js';
    script.async = true;
    head.appendChild(script);
  } catch (e) {}
</script>
```

Após a inclusão ser feita com sucesso nossa equipe irá configurar o rastreio do comportamento dos usuários. Poderemos capturar os dados disponíveis nos elementos da página, url, query string ou cookies.

Caso existam dados necessários para o funcionamento de alguma das ferramentas contratadas, não disponíveis nas páginas, será realizada uma solicitação pela Omnilogic para a disponibilização dos mesmos.
