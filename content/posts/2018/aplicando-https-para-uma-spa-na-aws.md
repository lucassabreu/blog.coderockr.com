---
title: "Aplicando HTTPS para uma SPA na AWS"
description: "Recentemente passamos a servir a nossa landing page e o SPA do Planrockr sobre HTTPS, inicialmente apenas estamos usando HTTPS no nosso backend..."
author: "Lucas Abreu"
date: 2018-03-31
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "Criptografia", "Software Security", "AWS"]
---

Recentemente passamos a servir a nossa landing page e o SPA do [Planrockr](https://planrockr.com/) sobre HTTPS, inicialmente apenas estamos usando HTTPS no nosso backend, mas percebemos que seria melhor que nosso frontend também usasse.

Alguns dos motivos por traz disso seriam para melhorar o [ranking em sites de pesquisa](https://webmasters.googleblog.com/2014/08/https-as-ranking-signal.html), para garantir ainda mais a segurança nas comunicações, e também para passar mais segurança para os nossos usuários.

Como estamos servindo nosso frontend usando o S3 da AWS, é apenas uma questão de colocar um CloudFront na frente e alterar a rota no Route 53 e tudo passa a funcionar, mas acabou dando alguma dor de cabeça, não por ser uma tarefa difícil, mas simplesmente por termos encontrado instruções confusas e errôneas quando pesquisamos como executar a migração.

A maioria dos tutoriais que existem na internet sobre habilitar HTTPS no AWS para um SPA passam a instrução errada de que não podemos usar o link facilitado do S3 para vincular com o CloudFront. Isso acabou em um conjunto problemas de comunicação com o S3, e o fez rejeitar as chamadas vindas do CloudFront; e passar a simplesmente redirecionar para a URL pública do bucket, quebrando algumas funcionalidades do Planrockr, principalmente no on-boarding.

Para evitar que outros acabem passando por problemas semelhantes e para servir de registro para projetos futuros, abaixo vou descrever a forma correta (e fácil) de habilitar HTTPS usando o S3 e CloudFront da AWS.

Para usar HTTPS em um bucket do S3, primeiro é necessário possuir um bucket (😜), para esse tutorial, criei um bucket com o nome simple.planrockr.com, e adicionei um arquivo index.html bem simples:

{{< gist lucassabreu dada4cc6636fcbf0f8ca77d43224f4ae>}}

Habilitei o mesmo para funcionar como *Static website hosting*, então posso acessar a URL [http://simple.planrockr.com.s3-website-sa-east-1.amazonaws.com/](http://simple.planrockr.com.s3-website-sa-east-1.amazonaws.com/) e verei o seguinte:

![](https://cdn-images-1.medium.com/max/2000/1*lg-VvNZLAZAHMEuyDj93vw.png)

Com esse bucket podemos simular a migração de uma “SPA” no S3 sem HTTPS para uma usando CloudFront para servir via HTTPS.

O primeiro é acessar o dashboard do CloudFront no AWS, nele acesse o botão **Create Distribution**:

![](https://cdn-images-1.medium.com/max/2000/1*m0jPSL0EFd1Nm9UArqykYQ.png)

Ir na opção para Web:

![](https://cdn-images-1.medium.com/max/2572/1*FrTcAmT0-mh1LYrmdfP00Q.png)

Na tela **Create Distribution**, informe o nome do bucket que deseja usar, e selecione-o quando aparecer na lista.

![](https://cdn-images-1.medium.com/max/2000/1*orNer9hYh20vjnXSzUd3Ag.png)

Eu recomendo marcar a opção “Redirect HTTP to HTTPS” em **Viewer Protocol Policy**, para que o seu site/SPA sempre seja acessado via HTTPS, mesmo que o usuário tenha um link com HTTP apenas.

O resto é bem simples, pode deixar tudo no padrão, e apenas informar o certificado e os “CNAMEs” para o seu serviço.

Como normalmente um SPA usa algum framework JavaScript para gerenciar as rotas (como no nosso caso o react-routes), então é necessário configurar algumas regras na distribution do CloudFront para que ele direcione todas as chamadas para o seu index.html base que ira lidar com as rotas.

Para isso entre na distribution, na aba “Error Pages”, vamos adicionar duas regras para que todas as chamadas para arquivos que não existam no bucket sejam direcionadas para o index.html do SPA.

Fica assim:

![](https://cdn-images-1.medium.com/max/2000/1*JE2At1orxD9044RD0AX31w.png)

O S3 retorna os Status Codes 403 e 404 quando não consegue achar um arquivo ou não permite acesso a ele, desse modo criando a regra acima para esses dois Status Codes todas as requisições (que não forem de assets) serão direcionados ao index.html.

Depois destes ajustes você tem um bucket do S3 sendo servido com HTTPS pelo CloudFront sem quaisquer problemas.

É importante dizer que essa solução é muito boa para SPAs, mas se possuir regras mais complexas de redirecionamentos, ou mais “páginas principais” para o mesmo site, então provavelmente não vai lhe atender, pois não há suporte no CloudFront para isso, seria necessário tratar na origem que o CloudFront estiver lendo.

*Originally published at [www.lucassabreu.net.br](http://www.lucassabreu.net.br/post/aplicando-https-para-uma-spa-na-aws/) on April 1, 2018.*
