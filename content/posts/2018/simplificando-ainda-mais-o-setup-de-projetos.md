---
title: "Simplificando Ainda Mais o Setup de Projetos"
description: "Ano passado nós criamos um script simples para auxiliar no setup de projetos no GitHub, ele atendeu bem as nossas necessidades para os novos repositórios que criamos no GitHub..."
author: "Lucas Abreu"
date: 2018-06-06
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "GitHub", "Ferramentas"]
aliases: [
    "/simplificando-ainda-mais-o-setup-de-projetos-e6a482b25b8a"
]
---

# Simplificando ainda mais o Setup de Projetos

Agora simplificando GitHub, GitLab e Trello

*English version: [click here](https://blog.coderockr.com/simplifying-project-setup-even-further-da5ef60a1ec9)*

Ano passado nós criamos um script simples para auxiliar no [setup de projetos no GitHub](https://blog.coderockr.com/simplificando-o-setup-de-projetos-no-github-f29b76c83194), ele atendeu bem as nossas necessidades para os novos repositórios que criamos no GitHub.

Porém também trabalhamos com outras ferramentas para gerir os nossos projetos, como o GitLab e o Trello, mas não fizemos um script semelhante para eles.

Agora no inicio do ano criei um novo script para realizar o [setup dos projetos no GitLab](https://github.com/Coderockr/coderockr-way-github-setup#gitlab), que é basicamente uma cópia com os endpoints do GitLab.

Ao começar a trabalhar no script para o Trello acabei me deparando com um problema, diferente do GitLab e GitHub ele não oferece um sistema de tokens para serem usados em scripts. Mas isso não impede integrações com o Trello, apenas é necessário usar o processo de autenticação deles ([lembra OAuth, mas muito mais simples](https://developers.trello.com/page/authorization)).

Eu poderia criar um script que usasse o token retornado por esse processo, porém isso exigiria que o usuário usasse o navegador para autenticar e depois informasse no script ou abrir a tela durante a execução do script.

Não gostei muito da ideia parecia muito com uma gambiara, digo, artifício técnico.

Procurando uma outra solução resolvi criar uma SPA para fazer o setup dos projetos com base no Coderockr Way, dessa forma quando fosse necessário fazer o setup de um projeto não precisaria nem da minha máquina, poderia fazer direto do celular por exemplo.

Aproveitei e além do Trello essa SPA também iria cobrir o GitHub e o GitLab, sendo uma solução completa para as ferramentas que usamos hoje.

Então encontrei outro problema, quando comecei a desenvolver a integração com o GitHub e GitLab seria necessário usar a autenticação por OAuth que exige um callback e tratamento em um backend. Isso queria dizer que precisaria também de um servidor para realizar as autorizações, não poderia ser simplesmente um GitHub Pages ou S3 servindo a SPA.

Para minha sorte eu já estava usando um outro serviço de gerenciamento de páginas estáticas, o Netlify! E convenientemente ele possui um feature chamada [OAuth Providers](https://www.netlify.com/docs/authentication-providers/) que pode ser usada para se autenticar contra o GitHub e GitLab (BitBucket também, mas não vem ao caso), além de servir as páginas por HTTPs que é muito bom.

Agora com todos os problemas resolvidos, faltava apenas terminar o SPA e publicar 😄

Agora quando for necessário fazer o setup é só entrar no “Coderockr Way Project Setup”, se autorizar, escolher o projeto na lista (com base na sua permissão) e Aplicar !

De uma olhada em como ficou: [https://cwps.lucassabreu.net.br](https://cwps.lucassabreu.net.br/)

![](https://cdn-images-1.medium.com/max/2702/1*3i2vq5LRI-jMOamXQQASog.png)

Para quem estiver interessado em customizar a ferramenta com as próprias labels basta fazer um fork e alterar o arquivo [Labels.js](https://github.com/lucassabreu/coderockr-way-project-setup/blob/master/src/Labels.js) e aproveitar 😉

Aqui esta o [fonte](https://github.com/lucassabreu/coderockr-way-project-setup).
