---
title: "Como Gerenciamos Projetos na Coderockr"
description: "A Coderockr é, simplificando bastante, uma empresa de projetos de software. Para conseguirmos cumprir nosso propósito precisamos gerenciar projetos..."
author: "Elton Minetto"
date: 2015-06-25
draft: false
categories: ["Coderockr Culture", "Desenvolvimento"]
tags: ["Gerenciamento de Projetos", "Metodologias"]
---

A Coderockr é, simplificando bastante, uma empresa de projetos de software. Para conseguirmos cumprir nosso propósito precisamos gerenciar projetos com qualidade e eficiência. Neste post vamos comentar como fazemos isso.

## Metodologia

Desde o nascimento da Coderockr sempre tivemos como foco o uso de metodologias ágeis, principalmente devido a natureza dos projetos e dos clientes que atendemos, que precisam responder rápido a mudanças. Após muito estudo e experiências chegamos a conclusão que a melhor metodologia para nossos projetos é o Kanban. Na nossa experiência o Scrum mostrou-se um pouco burocrático, com suas reuniões e sprints de tamanho fixo. Nós quebramos as tarefas em tamanhos similares, para podermos calcular o tempo médio de entrega, e criamos quadros para cada projeto, com as fases:

* **To Do**: lista de tarefas a serem realizadas. O cliente pode adicionar tarefas a qualquer momento e reordená-las de acordo com a prioridade.

* **In Progress**: são as tarefas que estão sendo trabalhadas, seja código, análise, arquitetura, criação de servidores, design, etc.

* **Review**: todas as tarefas passam por revisão de outra pessoa. Se é uma tarefa de código cada tarefa gera um *branch* no *git* e quando o desenvolvedor move para Review ele cria um *Pull Request* para outra pessoa avaliar. As *branches* sempre são criadas a partir da *branch master *e quando aprovadas o *merge* é feito também para a *master*. Usamos algo bem similar ao [Github Flow](https://guides.github.com/introduction/flow/) como metodologia de desenvolvimento e escrevemos um [post](https://medium.com/@coderockr/a-import%C3%A2ncia-da-revis%C3%A3o-de-c%C3%B3digo-a1a8b41ed7ff) sobre a importância da revisão de código.

* **Testing**: após o *Pull Request* ser aprovado fazemos o *deploy* da tarefa para um servidor de testes/homologação onde outra pessoa, geralmente o cliente, faz mais uma sessão de testes. Os testes unitários além de serem executados pelos desenvolvedores são feitos novamente pelo servidor de integração contínua.

* **Done**: após todos os testes e correções terem sido feitos, o *deploy* é realizado para o servidor de produção.

Encontramos diversas vantagens neste processo. O cliente agora não precisa esperar até o final do *sprint* para ver o resultado da tarefa pois fazemos entregas contínuas. Para os desenvolvedores é motivador ver o processo evoluindo e mais coisas sendo entregues. Como as tarefas são pequenas (geralmente tentamos quebrar em tarefas que não demorem mais do que 1.5 dias para serem feitas) as mudanças são mais fáceis de serem realizadas.

## Equipe

Em cada projeto além dos desenvolvedores temos duas figuras importantes:

* **O gerente de projetos**. Ele é responsável por coordenar a equipe, conversar com o cliente e acompanhar o andamento das tarefas. Como a maioria dos contratos da Coderockr é no formato “banco de horas”, onde o cliente compra X horas/mês é papel do gerente acompanhar o consumo destas horas e manter o cliente atualizado.

* **O líder técnico**. Geralmente é uma pessoa que tem mais experiência nas tecnologias usadas no projeto ou com o próprio negócio do cliente. É seu papel auxiliar o gerente na definição das tarefas, ajudar a equipe a resolver problemas mais complexos, auxiliar na definição da arquitetura das soluções, escalabilidade, performance e qualidade.

Em alguns projetos menores estes papéis podem ser desempenhados por uma mesma pessoa e um desenvolvedor de um projeto pode ser líder técnico de outro.

## Ferramentas

Dentre todos os tópicos desde post este é o mais volátil. Todos os dias surgem ótimas novas ferramentas para auxiliar no desenvolvimento de software, análise de qualidade e na gestão de projetos. Atualmente na nossa caixa de ferramentas temos:

* **Bitbucket** para gerenciar os repositórios privados.

* **Github** para gerenciar os [repositórios públicos.](https://github.com/coderockr)

* **Jira** para os projetos mais complexos. Ele possui ótimos relatórios como o *Control Chart* que nos permite acompanhar o tempo médio de entrega das tarefas, entre outros. E a integração com o Bitbucket é bem interessante.

* **Trello** para projetos menores. O Trello é uma ferramenta muito útil e versátil mas faltam relatórios estatísticos que são importantes para a gestão de projetos maiores.

* **Codeship** como servidor de integração contínua. Também estamos testando o [PHPCI](https://www.phptesting.org/) em alguns projetos. O deploy fazemos com scripts PHP criados com o [deployer.](http://eltonminetto.net/blog/2015/03/18/usando-o-deployer/)

* **PHPCS, PHPMD, PHPCPD, PHPUnit**. Usamos ferramentas para auxiliar no processo de revisão de código e testes. Também estamos avaliando ferramentas como o [Codacy](https://www.codacy.com) e o [SensioLabsInsight.](https://insight.sensiolabs.com/)

* **Tiempo** para anotarmos as horas que estamos trabalhando em cada projeto.

O mais importante conceito que tentamos manter sempre em nossas mentes é a evolução. A cada erro aprendemos como fazer melhor, com cada nova ferramenta ou nova ideia que surge tentamos chegar mais próximo da perfeição.
