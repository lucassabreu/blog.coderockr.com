---
title: "Como Melhorar seus Códigos Usando Object Calisthenics"
description: "Em um dos primeiros projetos que a Coderockr participou tivemos o privilégio de trabalhar com um “dream team”..."
author: "Elton Minetto"
date: 2016-06-23
draft: false
categories: ["Coderockr Culture", "Desenvolvimento"]
tags: ["Desenvolvimento", "Clean Code", "PHP"]
---

Em um dos primeiros projetos que a Coderockr participou tivemos o privilégio de trabalhar com um “dream team”: Eduardo Shiota, Guilherme Blanco, Rafael Dohms e Otavio Ferreira (em ordem alfabética porque é impossível perfilá-los em qualquer ordem de relevância).

Neste projeto foi possível aprimorarmos vários pontos importantes como TDD, Scrum, trabalho remoto, análise, integração contínua, etc. Mas o que mais me marcou foram os conceitos de Clean Code e Object Calisthenics que eram aplicados ao projeto.

O Object Calisthenics é uma série de boas práticas e regras de programação que foram criadas pela comunidade de desenvolvedores Java. O Guilherme e o Rafael foram os responsáveis por adaptar estas regras para o ambiente PHP e são grandes evangelizadores destes conceitos. Se você tiver a oportunidade de ver alguma palestra deles sobre o assunto eu recomendo fortemente. Mas como eles estão vivendo fora do Brasil você pode começar olhando os slides da palestra [PHP para Adultos: Clean Code e Object Calisthenics](http://www.slideshare.net/guilhermeblanco/php-para-adultos-clean-code-e-object-calisthenics) e [You code sucks, let’s fix it](http://www.slideshare.net/rdohms/bettercode-phpbenelux212alternate).

Eu lembrei deste tópico esta semana, quando me deparei com o seguinte código em um projeto que estou dando manutenção/desenvolvendo novas features:

{{< gist eminetto 51c932a311d982dc0da5f5aa0bb44b0f >}}

É um código totalmente funcional mas analisando algumas regras do Object Calisthenics é possível refatorá-lo para algo assim:

{{< gist eminetto 52e77ad749e5cef7bcd6455579c66f68 >}}

Desta forma o código fica mais legível e de fácil manutenção. E o código torna-se mais performático pois possui uma complexidade bem menor.

Minha recomendação é revisar as dicas do Guilherme e do Rafael e tentar aplicá-las aos seus códigos. Ou pelo menos tentar aos poucos absorvê-las e adaptá-las a sua realidade. O resultado é muito recompensador e vale o pequeno investimento.

*Originally published at [eltonminetto.net](http://eltonminetto.net/blog/2016/06/24/como-melhorar-seus-codigos-usando-object-calisthenics/) on June 24, 2016.*
