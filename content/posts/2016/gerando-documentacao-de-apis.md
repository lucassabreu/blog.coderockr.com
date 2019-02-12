---
title: "Gerando documentação de APIs"
description: "Uma das melhores decisões técnicas que tomei na minha carreira foi investir pesado nas arquiteturas baseadas em serviços..."
author: "Elton Minetto"
date: 2016-05-31
draft: false
categories: ["Coderockr Culture", "Desenvolvimento"]
tags: ["Desenvolvimento", "API", "Ferramentas"]
---

Uma das melhores decisões técnicas que tomei na minha carreira foi investir pesado nas arquiteturas baseadas em serviços. Meu primeiro post sobre isso data de 2011 e desde então esta decisão só se provou um acerto.

Uma das tarefas mais importantes, e chatas, é manter a documentação das APIs sempre atualizadas pois elas são consumidas por cada vez mais camadas: frontend, mobile, outros serviços e sistemas. Existem várias ferramentas para esta tarefa, sendo uma das mais completas, e complexas, o [Swagger](http://swagger.io), além de alguns serviços pagos. Estamos usando o Swagger em um [grande projeto](http://compufacil.com.br), mas eu estava a procura de algo mais simples para ser usado no [Planrockr](http://planrockr.com) e encontrei o [APIDOC](http://apidocjs.com).

Trata-se de um aplicativo escrito para o NodeJS que lê anotações nos seus códigos e gera uma documentação em HTML bem simples mas eficaz. Além das linguagens que usam o formato *Javadoc Style* ( C#, Go, Dart, Java, JavaScript, PHP, TypeScript) ele consegue gerar documentação em arquivos CoffeeScript, Elixir, Erlang, Perl, Python e Ruby.

Para instalar a ferramenta é necessário ter instalado o gerenciador de pacotes do NodeJS, o *npm* e rodar o comando:
> npm install apidoc -g

No site oficial é possível ver todas as anotações disponíveis, mas abaixo um exemplo de uma documentação do Planrockr:
{{< gist eminetto 9d02d3aa6f88a12906cff0cb4e23d0ba >}}

O próximo passo é criar na raiz do seu projeto um arquivo chamado *apidoc.json* com algumas configurações básicas:

{{< gist eminetto 0dc68c8f9e63fe0a47984ac2042c21f5 >}}

E para gerar a documentação basta executar:

    apidoc -c apidoc.json -i . -e backend/vendor -e planrockr-backend/frontend/bower_components -o ./apidoc

Para ver as opções do comando é só usar o comando
> apidoc — help

Alguns exemplos da documentação gerada:

![](https://cdn-images-1.medium.com/max/2800/0*uYXELHo3aMZqQ5Fj.png)

![](https://cdn-images-1.medium.com/max/2800/0*PyKQTawW_py_NIvq.png)

Além de ser bem bonita e fácil de acessar é possível incluir a sua geração em um script de commit, build ou deploy. E por suportar várias linguagens é fácil manter uma documentação unificada e gerada automaticamente.

O que você vem usando para esta tarefa? Se tiver outra sugestão por favor complemente aqui nos comentários.

*Originally published at [eltonminetto.net](http://eltonminetto.net/blog/2016/06/01/gerando-documentacao-de-apis/) on June 1, 2016.*
