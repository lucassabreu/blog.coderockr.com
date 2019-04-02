---
title: "Tratamento de Erros em Go"
description: "Um dos primeiros pontos que causam estranheza para quem está começando em Go é a forma como os erros são tratados"
author: "Elton Minetto"
date: 2016-07-13
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "GO", "Ferramentas"]
aliases: [
    "/tratamento-de-erros-em-go-cf4e4c23e813",
]
---

Um dos primeiros pontos que causam estranheza para quem está começando em Go é a forma como os erros são tratados, principalmente quando viemos de outras linguagens orientadas a objetos. Em Go os erros são “first class citizens”, ou seja, eles não são ocultos ou delegados e são considerados parte importante do código.

Hoje passei por uma situação onde isso fez diferença. Revisando/debugando o código em PHP (alterado do original):

{{< gist eminetto e6fb9c415fc86db28f6018cc3533ca97 >}}

Assume-se que o *$response*, por exemplo, deve retornar um valor correto, que qualquer erro seria tratado pelo método *transmit* e *exceptions* seriam geradas caso contrário.

Em Go o mesmo código poderia ser escrito da seguinte forma:

{{< gist eminetto dccc686a54ce563647940ec7bbb2ee8e >}}

Como todo método pode retornar dois resultados, um de sucesso e outro de erro, podemos capturar e tratar o problema explicitamente.

A primeira impressão é que o código está mais burocrático e ferindo algum princípio como os defendidos pelo SOLID, mas o erro e seu tratamento está bem mais claro. E em uma linguagem de programação que tem como foco o desenvolvimento de aplicativos concorrentes isso pode ser uma grande vantagem.

Também é possível ignorar o erro usando:

{{< gist eminetto a318e490dbaf1779f6243873e73387e7 >}}

E desta forma estamos delegando o controle para o método *transmit*, como no exemplo em PHP.

A “moral da história” aqui é entender as vantagens e desvantagens de cada abordagem para podermos escolher a melhor forma para implementá-la em nossos projetos.

Mais detalhes sobre o tratamento de erros em Go pode ser vista no [blog oficial](https://blog.golang.org/error-handling-and-go)

*Originally published at [eltonminetto.net](http://eltonminetto.net/2016-07-14-tratamento-erros-go/) on July 14, 2016.*
