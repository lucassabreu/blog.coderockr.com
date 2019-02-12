---
title: "Definindo APIs com o API Blueprint"
description: "Uma das melhores decisões que tomamos na Coderockr foi adotarmos a abordagem “API First” para todos os projetos que iniciamos..."
author: "Elton Minetto"
date: 2017-06-28
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "API", "Ferramentas"]
---

# Definindo APIs com o API Blueprint

Uma das melhores decisões que tomamos na Coderockr foi adotarmos a abordagem “API First” para todos os projetos que iniciamos, desde 2010. Mas nos últimos meses percebemos uma necessidade: melhorar o processo de definição e documentação das APIs.

Já usávamos [outras abordagens](http://eltonminetto.net/2016/06/01/gerando-documentacao-de-apis/), mas a maioria delas envolvia documentar a API no próprio código, usando *annotations*. Esta abordagem funciona, mas tem alguns problemas, principalmente quando a documentação precisa ser alterada por alguém de negócios. E gerar “mocks” e testes destas anotações também é um desafio complexo.

Com isso em mente fizemos algumas pesquisas e chegamos a duas alternativas: Swagger e API Blueprint. Ambos são padrões de documentação de APIs e tem suas vantagens e desvantagens:

* Swagger: é o [padrão de mercado](https://www.openapis.org/) e vem sendo adotado por várias empresas como a Amazon. Para descrever a API é necessário criar arquivos JSON, o que facilita bastante para os programadores, mas é um pouco complexo para visualizar e alterar seu conteúdo por pessoas não tão envolvidas com código. Existe uma série de ferramentas que podem ajudar neste processo, mas isso tornou-se uma pequena barreira para nós. (bom, pelo menos não é YML… Já comentei que odeio YML?)

* API Blueprint: é uma [especificação](https://apiblueprint.org/) mais recente e foi criada por uma empresa chamada [apiary](https://apiary.io/), comprada pela Oracle. A grande vantagem do API Blueprint é ser descrita em Markdown, o que facilita bastante a edição dos documentos, mesmo por quem não tem familiaridade com código. Além disso, existe uma série de ferramentas disponíveis que permitem gerar documentos no padrão Swagger, “mock servers” e testes.

Optamos pelo API Blueprint pela facilidade de uso e agilidade que isso nos trouxe. Vou demonstrar com um pequeno exemplo.

A definição é escrita em um arquivo no formato Markdown, que pode ser nomeado como “api.md” ou “api.apib”. Ambos funcionam, mas se usarmos a extensão .apib podemos aproveitar plugins para editores como o SublimeText que auxiliam na escrita. Os plugins podem ser encontrados no site oficial da especificação.

Nosso exemplo:

{{< gist eminetto c8b2a6a8e726d36c202eaae41eef8d73>}}

No site da especificação é possível ver os detalhes, mas basicamente o que fazemos é definir as URLs, o formato das requisições e das respostas. Podemos definir estruturas de dados simples e complexas e usá-las para descrever o que a API espera de entradas e o que deve gerar de saída. O documento é relativamente simples de entender e alterar, o que foi um dos pontos de maior peso para nossa escolha. Mesmo assim, podemos melhorar a apresentação.

## Gerando documentação

Dentre as ferramentas disponíveis no site oficial a [aglio](https://github.com/danielgtaylor/aglio) é uma das mais interessantes para geração de uma apresentação HTML da nossa definição. Ele pode ser instalado via:

    npm install -g aglio

Para gerar a documentação podemos usar o comando:

    aglio -i api.apib --theme-full-width --no-theme-condense -o index.html

No site da ferramenta é possível ver todas as opções de customização de temas e apresentação. Outro comando útil é o:

    aglio -i api.apib --theme-full-width --no-theme-condense -s

Ele gera um servidor local, na porta 3000, que fica observando alterações no arquivo .apib e atualiza automaticamente a página da documentação. Isso facilita bastante a manutenção do documento. Um exemplo da documentação gerada:

![](https://cdn-images-1.medium.com/max/2800/0*qX1oOdJe2z_9DgdB.png)

A documentação ajuda muito no processo de desenvolvimento dos clientes da API, mas podemos ir além.

## Gerando um mock server

Com a API definida as equipes de frontend (web, mobile, etc) e backend (quem vai desenvolver a API) podem trabalhar em paralelo. Para facilitar ainda mais podemos criar um “mock server” que vai gerar dados falsos baseados na definição da API. Assim a equipe de frontend pode trabalhar sem precisar esperar a equipe de backend terminar a implementação. Para isso vamos usar outra ferramenta, a [drakov](https://github.com/Aconex/drakov).

Para instalar a ferramenta basta executar:

    npm install -g drakov

E para gerar o servidor:

    drakov -f api.apib -p 4000

Desta forma temos uma API funcional que pode ser usada para testes e desenvolvimento.

O passo final é definirmos uma forma de validarmos nossa API.

## Testando

Podemos usar uma ferramenta chamada [apib2swagger](https://github.com/kminami/apib2swagger) para gerar um arquivo Swagger da nossa API e realizarmos testes usando algum recurso do Swagger. Optamos por usar o [dredd](https://github.com/apiaryio/dredd) que automatiza os testes, tanto usando API Blueprint quanto Swagger.

Para instalá-lo:

    npm install -g dredd

E para executar os testes:

    dredd api.apib [http://localhost:4000](http://localhost:4000)

Neste exemplo estou usando o dredd para testar nosso “mock server”, por isso o resultado deve ser positivo. Podemos colocar o dredd na execução do nosso servidor de integração contínua para garantir que a implementação da API sempre esteja de acordo com a documentação, evitando surpresas e documentos abandonados.

Com o conjunto API Blueprint + aglio + drakov + dredd conseguimos mapear todo o ciclo de vida de uma API: definição, documentação, desenvolvimento e testes. Os resultados estão sendo bem positivos e devemos adotar essa solução em todos os novos projetos.

*Originally published at [http://eltonminetto.net/post/2017-06-29-definindo-apis-com-api-blueprint/](http://eltonminetto.net/post/2017-06-29-definindo-apis-com-api-blueprint/) on June 29, 2017.*
