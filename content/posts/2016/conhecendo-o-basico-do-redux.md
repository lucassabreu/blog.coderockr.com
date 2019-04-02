---
title: "Conhecendo o Básico do Redux"
description: "Redux é um container de estados previsível para aplicações javascript, mas o que isso significa?"
author: "Vinicius Dacal"
date: 2016-06-22
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "JS", "React", "Redux", "Programação Funcional"]
aliases: [
    "/conhecendo-o-b-sico-do-redux--7315bcf09cef",
    "/conhecendo-o-basico-do-redux--7315bcf09cef",
    "/conhecendo-o-básico-do-redux--7315bcf09cef",
    "/conhecendo-o-basico-do-redux-7315bcf09cef",
    "/conhecendo-o-básico-do-redux-7315bcf09cef",
]
---

Redux é um container de estados previsível para aplicações javascript, mas o que isso significa?

![](https://cdn-images-1.medium.com/max/2000/1*deNSjC7XD-DA3w4wNK7XEQ.jpeg)

Concebido por [Dan Abramov](undefined), redux é um padrão de arquitetura de aplicação, inspirado no [Flux](http://facebook.github.io/flux/) do Facebook e simplificado com ajuda de algumas práticas do [Elm](https://github.com/evancz/elm-architecture-tutorial/). Redux lhe da o poder de construir aplicações consistentes e controlar a forma como os dados fluem e são transformados, seguindo três princípios:

### 1. Single source of truth [(SSOT)](https://en.wikipedia.org/wiki/Single_source_of_truth)

Todo o estado da sua aplicação é armazenado em uma única árvore de objetos, dentro de uma única store. Qualquer acesso ao state, é feito através de referência ao dado armazenado na store. Essa prática evita que você tenha dados duplicados, e uma vez que um dado é atualizado, a alteração se propaga para toda a aplicação.

### 2. State is Read-only

A única forma de alterar o estado da sua aplicação é emitindo uma **action**, um objeto descrevendo o que aconteceu.

Para acessar o state, você pode utilizar o método **getState** da store, o mesmo retorna todo o estado da aplicação, mas somente para a leitura.

Para escutarmos as alterações do state, utilizamos o método **subscribe** da store, passando como parâmetro um listener, da seguinte forma:

    let unsubscribe = store.**subscribe**(() => {
      //executa a cada alteração no state
      const state = store.**getState**();
    });

    unsubscribe(); //canceling listener
> Como podemos ver no exemplo acima, quando quisermos cancelar o listener, basta chamar o retorno do método **subscribe** como uma função.

Quando precisamos fazer uma alteração no **state**, a mesma precisa ser “solicitada” através de uma **action**, um objeto contendo obrigatoriamente um **type** e opcionalmente um **payload**(conteúdo). O **type** é um identificador para a **action** e o **payload** é o conteúdo a ser enviado junto a ela, conforme o exemplo abaixo:

    {
      type: 'ADD_CONTACT',
      name: 'Jon Snow',
      email: 'youknownothing@jonsnow.com'
    }

como é possível observar acima, essa é uma **action** to tipo ADD_CONTACT, e leva com ela os dados necessários para incluir esse contato ao nosso **state**.

É comum utilizarmos **action creators** para criarmos nossas actions, que são simplesmente funções que esperam parâmetros específicos e devolvem um objeto formatado, conforme o exemplo abaixo:

    const addContact = (name, email) => {
      return {
        type: 'ADD_CONTACT',
        name,
        email
      }
    };

para disparar uma **action**, utilizamos o método **dispatch** da store:

    dispatch(addContact('Jon Snow', 'youknownothing@jonsnow.com'));

### 3. Changes are made with Pure functions

Para descrever como o **state** da aplicação será alterado pelas **actions**, nós escrevemos pure [reducers](http://redux.js.org/docs/Glossary.html#reducer).

**Reducers** são funções que são chamadas toda vez que uma action é disparada e recebem como parâmetros o **state** atual e a **action**, e devolvem um novo **state.**

É importante ressaltar que um **reducer** deve ser, obrigatoriamente, uma função pura. Dado os mesmos parâmetros, um reducer deve sempre devolver o mesmo resultado e não pode se basear em nada que esteja além do seu próprio escopo para chegar nesse resultado. Para quem quiser se aprofundar um pouco mais em conceitos funcionais, eu indicaria dar uma conferida no post do [Matheus Lima](undefined) sobre [programação funcional em Javascript](https://medium.com/@matheusml/entendendo-programa%C3%A7%C3%A3o-funcional-em-javascript-de-uma-vez-c676489be08b#.9l123fghs).

Considerando que nossos reducers devem ser funções puras, vamos implementar nossa lógica de adicionar um novo contato ao nosso **state**:

    const reducer = (state = {}, action) => {
      if (action.type === 'ADD_CONTACT') {

        return {
          ..state,
          contactCollection: [
            ...state.contactCollection,
            {
              name: action.name,
              email: action.email
            }
          ]
        };
      }
    }

como é possível ver no exemplo acima, nosso reducer verifica se a action é tipo ADD_CONTACT, e se ela realmente for, ele retorna um novo state, incluindo o novo contato.

É importante que, para poder aproveitar ao máximo do redux, você trabalhe com [**imutabilidade**](https://en.wikipedia.org/wiki/Immutable_object). No exemplo acima, nós não fazemos um **push** do novo contato dentro da **contactCollection**, e sim retornamos uma nova contactCollection. Esta possui os mesmos contatos da anterior, mas contém o novo contato. Desta forma preservamos o state anterior e nos mantemos dentro da premissa de **read-only state**. Como **Imutabilidade** é um dos princípios base dos redux, aconselho assistir a palestra do William Huang no RSJS2016 [Quando os dados imutáveis mudam tudo](https://www.youtube.com/watch?v=8-R9C3yerPo&list=PLg2lQYZDBwORWkiAe6L9Ah-L2JxJ6froI&index=8), que explica de uma forma prática imutabilidade em Javascript.

Seguindo esses três princípios da forma correta, você conseguirá escrever aplicações previsíveis de uma forma simples.

Nesse post arranhamos apenas a superfície do redux, nos próximos posts vou aprofundar mais em cada conceito.

O [ReduxJS](http://redux.js.org/) é uma lib em JS que implementa o redux em apenas 2kb. Ele conta com uma ótima documentação, onde é descrito em detalhe cada feature do redux. Se você pretende se aprofundar, a documentação é um bom lugar para começar.

No próximo post vou abordar uma forma de trabalhar com actions assíncronas, para fazer requisições para APIs.

A princípio, redux pode parecer complicado por ser diferente dos padrões de arquitetura que estamos acostumados, mas assim que você aprende, o céu é o limite.
