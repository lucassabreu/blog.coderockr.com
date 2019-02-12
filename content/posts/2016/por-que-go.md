---
title: "Por que Go?"
description: "estes quase 20 anos de carreira como desenvolvedor eu já trabalhei com várias linguagens de programação, desde C, passando por Cobol, Java, Python e PHP..."
author: "Elton Minetto"
date: 2016-04-28
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "GO", "Ferramentas"]
---

A melhor ferramenta para cada necessidade

Nestes quase 20 anos de carreira como desenvolvedor eu já trabalhei com várias linguagens de programação, desde C, passando por Cobol, Java, Python e PHP, que tornou-se minha principal ferramenta.

Uma das coisas que aprendi nesse processo foi sempre tentar encontrar a melhor ferramenta para cada situação e evitar cair na máxima:

> Se a única ferramenta que você tem é um martelo, para você tudo começa a se parecer com um prego.

E no último ano incluimos uma nova linguagem na nossa caixinha de soluções aqui na Coderockr: Go. Neste post vou explicar para que estamos usando a linguagem e porque escolhemos ela em detrimento de outras opções.

### Para que estamos usando?

Basicamente para duas situações: micro-serviços e algoritmos que necessitam de performance.

Um exemplo clássico de micro-serviço é o fato de um grande número de projetos precisar realizar a consulta dos dados de um endereço a partir de um CEP. Esta é uma funcionalidade que pode ser isolada na forma de um serviço e ser compartilhada entre todos os projetos, o que fizemos com o [goCep](https://github.com/eminetto/gocep). Unindo este pequeno serviço com as vantagens do Docker podemos simplesmente executar o comando *docker pull eminetto/gocep *e temos um container com o serviço pronto para ser usado em qualquer servidor/máquina.

Quanto a segunda situação, ela vem surgindo cada vez mais durante o desenvolvimento do [Planrockr](http://planrockr.com). O exemplo mais recente é o de um serviço em que usamos o [Método de Monte Carlo](https://pt.wikipedia.org/wiki/Método_de_Monte_Carlo) para realizar simulações e indicar a probabilidade de datas de término de um determinado projeto. Usamos o recurso de *goroutines* para gerar mais de 1000 simulações para cada execução, fornecendo um resultado bem mais preciso. E como este serviço vai ficar disponível para os usuários acessarem via interface web é importante que ele possa ser performático ao extremo.

### Mas afinal, por que Go e não outra linguagem?

Alguns dos motivos pelos quais optamos por Go:

**Pedigree**. A linguagem foi criada dentro do Google, por ninguém menos do que [Robert Griesemer](https://en.wikipedia.org/wiki/Robert_Griesemer) (que trabalhou no desenvolvimento da engine JS V8), [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike) (que faz parte da equipe que desenvolveu o Unix) e [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson) (também parte do time original do Unix e criador da linguagem B, precursora do C)

**Boa documentação**. Existe uma boa documentação no [site oficial](https://golang.org/doc/) além de um bom número de livros e blogs. Este é um ponto que ainda deve melhorar bastante, principalmente a documentação em português, mas é possível ver um aumento considerável nos últimos anos.

**Evolução constante**. Como o código da linguagem [está no Github](https://github.com/golang/go/milestones?state=open) é fácil acompanhar o roadmap e ver as novas funcionalidades que estão sendo desenvolvidas.

**Curva de aprendizado moderada**. Por ser uma linguagem compilada e estaticamente tipada talvez quem esteja acostumado com códigos em PHP ou JavaScript vai enfrentar alguns problemas para se acostumar, mas nada assustador. Go consegue tornar fácil técnicas complexas como concorrência, sincronização, ponteiros, etc. E para quem tem alguma experiência em C vai se sentir confortável rapidamente.

**Divertida**. Isso é um bônus! Realmente venho me divertindo bastante com a linguagem, construindo coisas rapidamente, encontrando soluções, melhorando a cada compilação.

Estamos aos poucos aumentando nossa experiência com a linguagem e com isso novos posts e códigos devem surgir no nosso Github. Tem sido uma jornada divertida :)

P.S.: se ficou curioso quanto ao serviço que implementamos no Planrockr, cadastre-se [gratuitamente no site](http://planrockr.com) que em breve teremos novidades…
