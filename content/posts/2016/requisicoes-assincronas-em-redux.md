---
title: "Requisições Assíncronas em Redux"
description: "Quando se está começando a aprender Redux, uma das dúvidas mais comuns é como fazer requisições assíncronas."
author: "Vinicius Dacal"
date: 2016-07-02
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "JS", "React", "Redux"]
---

Quando se está começando a aprender **Redux**, uma das dúvidas mais comuns é como fazer requisições assíncronas.

Observando os exemplos básicos, que implementam apenas actions comuns, não fica tão óbvio qual abordagem utilizar ao fazer uma integração com uma API.

Neste post, mostrarei como implementar um **middleware,** que ficará responsável por interpretar um padrão de específico de **action** (async action) e gerenciar nossas requisições, disparando uma **action** correspondente para cada momento das mesmas.

### Actions

Quando estamos fazendo uma requisição assíncrona há dois momentos importantes que comumente alteram o estado a aplicação: O momento em que uma requisição se inicia, e o momento que você recebe um resultado.

Esses dois momentos podem ser divididos em três **actions** distintas: Uma action para o inicio da requisição, outra para o caso de sucesso e uma terceira para o caso de falha. Poderíamos chamá-las de **CALL_API_REQUEST**, **CALL_API_SUCCESS** e **CALL_API_FAILURE.**

De uma maneira simples, mas não muito elegante, poderíamos aplicar esse conceito sem utilizar um middleware, como no exemplo abaixo:

{{< gist viniciusdacal 1a31f2a14342773a3ba74e27b8e7e13c >}}

O código acima teria que ser utilizado em um local onde tivéssemos acesso ao método **dispatch**, que normalmente, é dentro de um **container component.** Poderíamos tentar melhorar esse código abstraindo as actions para um **action creator**, mas é notável que esse modelo não escala. Para cada endpoint da nossa API, precisaríamos ter três **action creators** diferentes, o que é impraticável.

Poderíamos tentar levar toda essa lógica para dentro do **action creator**, mas não teríamos acesso ao método **dispatch**.

Uma forma muito mais simples, seria termos uma **action** que descrevesse como será o request, um padrão de action diferente que chamaremos de **async action:**

{{< gist viniciusdacal 760a7108c4084fdc67b820a36b135a70 >}}

No exemplo acima, as principais diferenças que podemos notar entre uma action comum e nossa **async action**, é o fato de termos **types** ao invés de **type**, e de retornarmos uma função **callApi** junto com o objeto.

Os **types** serão utilizados nos três momentos distintos da requisição e é importante que eles sejam fornecidos na seguinte ordem:

    types[0] = **action** **type** indicando inicio do request
    types[1] = **action** **type** indicando sucesso
    types[2] = **action** **type** indicando falha

A **callApi** será utilizada futuramente para fazer o request.

Para interpretar essa action e gerenciar as requisições, nós utilizaremos um **Middleware.**

### **Middlewares**

Em suma, **[middlewares](http://redux.js.org/docs/advanced/Middleware.html)** são funções que ficam entre o disparo de uma **action** e a chegada dela no **Root Reducer,** sendo capazes de alterar o seu conteúdo ou até mesmo cancelá-la.

Abaixo segue um exemplo básico de um middleware de logs:

{{< gist viniciusdacal 7d50b0d1c1f20cb9239bfcb45e59952a >}}

No exemplo acima, nós não modificamos o conteúdo da **action**, apenas fazemos log dela e do **state** da aplicação após o seu disparo. É importante observar que nós chamamos o **método** next passando para ele a **action** atual, a fim de deixar a **action** seguir seu fluxo, até o próximo **middleware** ou até o **Root Reducer.**

O middleware que implementaremos irá interceptar as **async actions,** e as substituirá por três **actions** diferentes, correspondendo os três momentos do request.

Dadas estas informações, vamos para a implementação do nosso middleware:

{{< gist viniciusdacal 3852493497e23afbd3b711371cac34d6 >}}

Logo no início da função, na linha 13, nós verificamos se a action recebida se trata de uma **async action**, verificando se existe a propriedade **types** dentro dela. Caso se trate de uma action comum, nós apenas passamos ela a diante, sem efetuar nenhuma alteração.

Caso se trate de uma **async action, **nós extraímos os **types** para respectivas variáveis e disparamos uma **action, **contendo o primeiro **type**, indicando que o request irá iniciar.

Em seguida, nós chamamos a função **callApi**, que veio junto com a **async action**, e passamos para a promise as funções handlers, que lidarão com os responses em caso de sucesso e em caso de falha.

Dentro dos handlers, nós disparamos uma **action** indicando que foi finalizado o request, colocando no atributo **body** o resultado do request.

Em caso de sucesso, nós incluímos um atributo **lastFetched,** com o timestamp do momento em que o request foi finalizado.

Junto a cada action, foi incluído o **payload** da **async action**, para que os reducers tenham acesso ao mesmo.

### Incluindo o Middleware na store

Com o middleware implementado, nós precisamos incluí-lo na nossa store. Para isso, nós utilizaremos o método **applyMiddleware** do redux, conforme o exemplo abaixo:

{{< gist viniciusdacal 3a98de0a5ec3876bdd27bf28d7a7fbe7 >}}

Feito isso, todas as actions passarão pelo nosso **callAPIMiddleware**, as que forem **async actions** serão interceptadas e convertidas em actions comuns.

Dessa forma, conseguimos deixar por conta do middleware o disparo das requisições e podemos manter apenas um **action creator** para cada endpoint.

Com o **middleware**, agora é possível termos actions assim:

{{< gist viniciusdacal d8c51a96af2c39988cb049a4faead6e0 >}}

Async action que irá disparar uma requisição para o endpoint “user/contacts” passando como parâmetro um userId

Os **middlewares** não se limitam a controlar requisições, eles são uma das features mais poderosas do redux e pretendo abordá-los em futuros posts. Para quem tem interesse em se aprofundar, sugiro que comece pela [documentação](http://redux.js.org/docs/advanced/Middleware.html).

Se você está começando com Redux agora e chegou direto neste post, indico ler meu post anterior: [Conhecendo o básico do Redux](https://medium.com/coderockr-way/conhecendo-o-b%C3%A1sico-do-redux-7315bcf09cef#.fdvpjz4g3).

Estou começando uma série de posts sobre **Redux** e **React**, gostaria de receber um feedback de vocês sobre as dúvidas mais comuns de quem está começando. Pode ser enviando para o email: [viniciusldacal@gmail.com](mailto:viniciusldacal@gmail.com)
