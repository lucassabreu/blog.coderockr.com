---
title: "Gerando Code Coverage com PHPUnit e phpgbg"
description: "Em um post anterior eu mostrei alguns truques para identificar testes que estão demorando muito para serem executados..."
author: "Elton Minetto"
date: 2016-08-29
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "PHP", "Ferramentas"]
---

Em um post anterior eu mostrei alguns truques para identificar testes que estão demorando muito para serem executados. Neste texto vou mostrar uma forma de melhorar a performance da geração do relatório de cobertura de códigos usando o PHPUnit.

É possível incluir configurações no arquivo *phpunit.xml* para que sejam gerados relatórios relativos aos testes que estão sendo executados. Por exemplo:

{{< gist eminetto c40bb8a5ed54c495b4ae1bfa0c43a41b >}}

Desta forma será criado o diretório *tests/_reports* com uma série de informações úteis. No diretório *coverage-html* podemos ver detalhes da cobertura de testes dos códigos, facilitando a análise. O arquivo *clover.xml* é uma versão desta mesma informação para ser processada por serviços como Jenkins, CodeCov, Coveralls, Codacy, etc, para automatizarmos alertas e scripts.

Para estas informações serem geradas além de alterarmos o *phpunit.xml* é necessário que instalemos a extensão *XDebug*. O problema é que ao fazermos isso temos uma queda considerável de performance. Confira o resultado da execução dos testes sem a geração dos relatórios:

{{< gist eminetto bce4c9a19e2785f3812d46244a3f9756 >}}

Habilitando o *XDebug* e rodando novamente teremos uma surpresa:

{{< gist eminetto 69004afb25070817317ae7653d21f463 >}}

O tempo de execução pulou de 1.08 para 22.26 minutos!

Depois de algumas pesquisas pela internet cheguei a [este post](http://blog.remirepo.net/post/2015/11/09/PHPUnit-code-coverage-benchmark) e resolvi testar o *phpdbg*.

Como estou usando o MacOS X para este teste eu executei os comandos abaixo para instalar todas as dependências que eu necessito.

{{< gist eminetto a6c0caadf01d7807db6070cb5cb23356 >}}

A diferença principal é o parâmetro *–with-phpdbg* usando na instalação do php7.

Seguindo [este post](https://thephp.cc/news/2015/08/phpunit-4-8-code-coverage-support) do Sebastian Bergmann, criador do PHPUnit eu cheguei a sintaxe para rodar o teste novamente:

*phpdbg -qrr ./vendor/bin/phpunit*

{{< gist eminetto a5a49e789b0670cbddfdf55ed89ef6d7 >}}

1.59 min é um tempo bem melhor do que os 22.26 usados pelo *XDebug*.

Na empolgação eu comentei isso no Twitter, antes mesmo de escrever este post:

![](https://cdn-images-1.medium.com/max/2000/0*8iruJz2kqgtjw3qQ.png)

Se você observar, quem respondeu foi ninguém menos do que o criador do *XDebug*! Levando isso em conta fiz a comparação entre os resultados gerados pelo *XDebug* e o *phpdbg*.

Abaixo a comparação do *coverage-html* gerado pelo *XDebug* (na esquerda) e o *phpdbg* (na direita da imagem).

![](https://cdn-images-1.medium.com/max/2800/0*gqeyhN4lr9cT0qDF.png)

Usei a ferramenta CodeCov para processar o arquivo *clover.xml* e o resultado também foi ligeiramente diferente:

![*phpdbg*](https://cdn-images-1.medium.com/max/2800/0*q9TaWLh0x4cvU0hO.png)**phpdbg**

![XDebug](https://cdn-images-1.medium.com/max/2800/0*Awn2wLOmelL5hHnh.png)*XDebug*

Segundo o relatório gerado pelo *phpdbg* o [Planrockr](http://planrockr.com) está com 66,05 % de cobertura de códigos. Já o *XDebug* apresenta o resultado de 65,92 %.

Algumas conclusões que posso tirar deste pequeno teste:

* O *XDebug* é uma ferramenta incrível e faz muito mais do que gerar cobertura de código, por isso não estou aqui dizendo que deveríamos parar de usá-la. Aqui estou apenas comparando um dos seus recursos

* Eu estou comparando o resultado “de fábrica”, sem fazer ajustes em configurações do *XDebug* ou do *phpdbg*, por isso resultados diferentes podem acontecer em outros cenários

* Apesar da diferença de resultados entre os dois relatórios a diferença de performance compensa o uso do *phpdbg* no meu caso.

Vou seguir usando o *phpdbg* por mais tempo e se algum novo resultado aparecer nas próximas semanas eu atualizo este post, ou gero outro relatando o aprendizado.

E se eu estou errado em minhas conclusões por favor me avisem que terei o maior prazer em me retratar :)

*Originally published at [eltonminetto.net](http://eltonminetto.net/post/2016-08-30-gerando-phpunit-code-coverage-with-phpdbg/) on August 30, 2016.*
