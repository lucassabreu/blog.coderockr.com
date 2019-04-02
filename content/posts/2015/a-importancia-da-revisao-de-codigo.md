---
title: "A Importância da Revisão de Código"
description: 'Em seu famoso artigo “A catedral e o bazar” Eric S. Raymond proferiu: "Dados olhos suficientes, todos os erros são óbvios"...'
author: "Elton Minetto"
date: 2015-06-05
draft: false
categories: ["Coderockr Culture", "Desenvolvimento"]
tags: ["Desenvolvimento", "Metodologias", "Clean Code"]
aliases: [
    "/a-import-ncia-da-revis-o-de-c-digo-a1a8b41ed7ff",
	"/a-importancia-da-revisao-de-codigo-a1a8b41ed7ff",
	"/a-importância-da-revisão-de-código-a1a8b41ed7ff",
]
---

Em seu famoso artigo “A catedral e o bazar” Eric S. Raymond proferiu:

> Dados olhos suficientes, todos os erros são óbvios

Esta frase, que ficou conhecida com a [“Lei de Linus”](http://pt.wikipedia.org/wiki/Lei_de_Linus), resume bem o fenômeno do código aberto e como ele mudou a forma como desenvolvemos software.

Na Coderockr adotamos a política de Code Review em todos os projetos. Para cada tarefa/melhoria/bug/feature o desenvolvedor cria uma nova branch no Bitbucket (ou [Github](https://github.com/Coderockr) se o projeto for open source) e ao final do desenvolvimento ele abre um Pull Request para que os outros membros da equipe ajudem a analisar o código. E o Pull Request só é aprovado quando uma ou mais pessoas aprovam a alteração.

Nós percebemos duas vantagens nesta abordagem. A primeira foi a melhoria do código. Sabendo que outras pessoas vão avaliar seu código a tendência é que o desenvolvedor preste mais atenção aos detalhes. E com mais pessoas analisando o código geralmente encontram-se melhorias ou possíveis bugs antes mesmo do código ir para a fase de testes/homologação.

A segunda melhoria foi a difusão do conhecimento pois agora os desenvolvedores podem acompanhar o que está sendo feito em diferentes partes do sistema, o que é importante em projetos grandes e também ajuda no processo de manutenção futura.

Para facilitar o processo de review nós criamos um [Checklist](https://github.com/Coderockr/CodeReview) que usamos para analisar os códigos. Além disto estamos sempre avaliando ferramentas que automatizem o processo como Scrutinizer, SensioLabs Insight, Codacy, Code Climate, etc.

A melhoria é um processo contínuo por isso estamos sempre tentando tornar a revisão ainda mais fácil e rápida mas já tivemos ótimos resultados desde que adotamos esta prática.
