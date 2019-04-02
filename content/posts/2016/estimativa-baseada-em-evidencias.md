---
title: "Estimativa Baseada em Evidências"
description: "As pessoas, salvo alguns místicos, não são muito boas em prever o futuro..."
author: "Elton Minetto"
date: 2016-06-02
draft: false
categories: ["Coderockr Culture", "Desenvolvimento"]
tags: ["Desenvolvimento", "Metodologias", "Gerenciamento de Projetos", "Ferramentas"]
aliases: [
    "/estimativa-baseada-em-evid-ncias-c156d5527427",
    "/estimativa-baseada-em-evidencias-c156d5527427",
    "/estimativa-baseada-em-evidências-c156d5527427",
]
---

As pessoas, salvo alguns místicos, não são muito boas em prever o futuro.

![[https://www.flickr.com/photos/pasukaru76/3998273279/](https://www.flickr.com/photos/pasukaru76/3998273279/)](https://cdn-images-1.medium.com/max/2560/1*I7neRm3n2G9LgGEwOObahg.jpeg)*[https://www.flickr.com/photos/pasukaru76/3998273279/](https://www.flickr.com/photos/pasukaru76/3998273279/)*

Portanto é bem provável que as suas estimativas de entregas de tarefas e projetos não sejam tão precisas quanto seu cliente espera, certo? Felizmente temos outros recursos a nossa disposição para suprir essa nossa deficiência: fatos, números e computadores rápidos o suficiente para processá-las.

Vamos fazer uma simples "conta de padaria" e imaginar o seguinte cenário:

> Dado um projeto a ser entregue o gerente de projetos, junto com a equipe de preferência, dividiu as funcionalidades em um número de tarefas, digamos 200. Sabe-se que a equipe consegue entregar 20 tarefas por semana, então o projeto seria finalizado em 10 semanas.

Fácil! Calma jovem Padawan, calma, a vida não é tão simples…

O que pode acontecer nesse cenário:

* Novas tarefas podem surgir durante o desenvolvimento

* Bugs podem ser encontrados

* As tarefas não são todas do mesmo tamanho/complexidade

* Novas pessoas podem entrar ou sair da equipe, mudando a velocidade das entregas

* Um apocalipse zumbi pode acontecer e os backend developers tornam-se consumidores de cérebro humano

Quanto ao último item pouco podemos fazer, além de estocar armas e comida em casa, mas os outros podem ser mitigados.

Como sabemos que novas tarefas/bugs podem surgir durante o desenvolvimento, vamos considerar uma margem de erro e assumir que o número de tarefas seja entre 200 e 240, por exemplo.

Para o problema das tarefas não serem todas da mesma complexidade podemos adotar uma regra simples: nenhuma tarefa deve demorar mais do que X horas para ser entregue. Na [Coderockr](http://coderockr.com) adotamos 16 horas, ou seja, nenhuma tarefa demora mais do que 2 dias para ser finalizada. Claro que algumas ainda são bem menores do que 2 dias, mas desta forma temos uma diferença pequena entre elas e não uma tarefa que demora 1 hora e outra 3 dias.

Quanto a velocidade do time, também podemos assumir uma margem de erro. Digamos que a equipe entrega entre 20 e 30 tarefas por semana, assim temos uma variação que contempla coisas como computadores com problemas, faltas por doença, feriados, etc.

Ok, mas agora como usar isso tudo no nosso cálculo? Simples:

![[https://www.flickr.com/photos/lisabrideau/with/5213615948/](https://www.flickr.com/photos/lisabrideau/with/5213615948/)](https://cdn-images-1.medium.com/max/5184/1*i4Q-IyYPJdjCerhCU7QGjA.jpeg)*[https://www.flickr.com/photos/lisabrideau/with/5213615948/](https://www.flickr.com/photos/lisabrideau/with/5213615948/)*

Não, não vou sugerir que você largue tudo e vá tentar a sorte em Las Vegas, apesar de eu já ter cogitado isso algumas vezes…

Vamos usar uma técnica chamada "Método de Monte Carlo". Segundo a [Wikipedia](https://pt.wikipedia.org/wiki/Método_de_Monte_Carlo) são:

> … métodos estatísticos que se baseiam em amostragens aleatórias massivas para obter resultados numéricos, isto é, repetindo sucessivas simulações um elevado numero de vezes, para calcular probabilidades heuristicamente, tal como se, de facto, se registrassem os resultados reais em jogos de casino (daí o nome)

Vamos pegar estes dados e realizar diversas simulações de possíveis cenários, digamos 1000, e contamos a probabilidade deles acontecerem. Com isso podemos ter algo similar a isso:

![](https://cdn-images-1.medium.com/max/2398/1*ezArcqj65E2BHD4DseC7KQ.png)

Conforme as semanas vão passando e as tarefas sendo entregues, a equipe pode ir atualizando este gráfico usando os dados mais precisos e acompanhando os resultados. Quanto mais dados ou evidências tivermos, mais preciso este método vai se tornando e as equipes podem ir acompanhando sua evolução.

Quer testar esse método? Criamos um site para facilitar isso, o [http://www.estimeoprazo.com.br](http://www.estimeoprazo.com.br). Nele é possível inserir os dados de número de tarefas e entregas e a tabela acima é apresentada com as suas estimativas. E se você quiser tornar isso ainda mais automático estamos implementando essa funcionalidade na nova versão do [iMasters Planrockr](http://planrockr.com), por isso [cadastre-se agora](https://app.planrockr.com/app/#/signup) e em breve você vai ter acesso aos seus dados na forma de um widget:

![](https://cdn-images-1.medium.com/max/2000/1*EI0NS5YgMd-xrYETtkBMXw.png)

O que acha deste método? Você já vem usando ele ou tem alguma sugestão de melhoria? Adoraríamos ouvir suas experiências.
