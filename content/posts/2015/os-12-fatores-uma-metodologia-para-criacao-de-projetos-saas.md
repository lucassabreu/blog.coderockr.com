---
title: "Os 12 Fatores: Uma Metodologia para Criação de Projetos SaaS"
description: "Todo desenvolvedor já deve ter ouvido falar do Heroku, plataforma de cloud computing que revolucionou o desenvolvimento, ajudou a criar o movimento..."
author: "Elton Minetto"
date: 2015-04-28
draft: false
categories: ["Coderockr Culture", "Desenvolvimento"]
tags: ["Desenvolvimento", "Metodologias", "Ferramentas"]
---

Todo desenvolvedor já deve ter ouvido falar do Heroku, plataforma de cloud computing que revolucionou o desenvolvimento, ajudou a criar o movimento “devops” e que foi vendida por diversos milhares de dólares para a Salesforce. Se tem algo que eles tem muita experiência é na criação e suporte de aplicativos SaaS (software-as-a-service), principalmente aplicativos web.

Eles usaram toda essa experiência para criar o [12factor](http://12factor.net/) que é uma espécie de “manifesto” com os 12 fatores que uma aplicação deveria seguir para ter sucesso nesse formato.

Vou citar elas abaixo e fazer alguns comentários, principalmente usando analogias e exemplos do mundo PHP, mas os conceitos se aplicam a qualquer plataforma.

## I. Codebase

Todos os seus códigos devem ser gerenciados por um sistema de controle de versões como o Git ou o Subversion. Não deve haver mais de uma base de códigos no projeto e desta base são gerados diversos “deploys”. Um deploy é uma versão do código executando em máquinas de desenvolvedores, testes ou produção. Podem haver pequenas diferenças de versão entre os deploys, por exemplo um desenvolvedor possui códigos ainda não commitados em sua máquina, mas a base de códigos, o repositório deve ser sempre o mesmo.

## II. Dependencies

As dependências do projeto devem ser explicitamente declaradas e isoladas do código. Em se tratando de PHP podemos usar o Composer para declarar e gerenciar as dependências. Caso o projeto dependa de alguma ferramenta externa, do sistema operacional por exemplo, o ideal é que uma versão dessa ferramenta esteja contida dentro de algum diretório do projeto. Desta forma o deploy pode ser feito para diversos servidores sem se preocupar com a falta de alguma dependência, o que facilita a escalabilidade.

## III. Config

As configurações devem ser armazenadas fora do código. Aqui uma opinião minha. O 12factor sugere que todas as configurações deveriam ser feitas em variáveis de ambiente, que seriam configuradas em cada servidor e o código iria lê-las no momento da execução. Por exemplo, no Apache iríamos incluir algo como:

*`SetEnv DN_NAME “project_db”`*

E no PHP usaríamos a função *```getenv()```* para recuperar essa informação e usá-la para conectar ao banco de dados, por exemplo. E em cada máquina, de desenvolvimento ou servidor, este valor pode ser configurado de maneira diferente.
Eu prefiro uma abordagem um pouco mais simples ao incluir no Apache algo como:

*`SetEnv APPLICATION_ENV “development”`*

E criaríamos arquivos de configuração diferentes para cada ambiente, como *config/development.php*, *config/production.php*, etc. São abordagens similares, mas o conceito é o mesmo: não colocar configurações em códigos-fonte.

## IV. Backing Services

Um “backing service” é um serviço que o aplicativo consome, geralmente via rede, como parte necessária para sua operação normal. Exemplos são banco de dados, servidores de cache, repositórios de arquivos, etc. O seu código deve ser criado considerando que estes recursos podem tanto estar instalados na máquina local quanto em servidores remotos, sem nenhuma mudança além de configurações. O modelo ideal é usar abstrações como um ORM e classes bem construídas para que a mudança de um banco de dados do MySQL para o PostgreSQL seja imperceptível para o projeto. Ou mesmo a mudança de armazenamento de arquivos local para o S3, mudança de cache de arquivos para o Memcached e assim por diante.

## V. Build, Release, Run

Uma base de códigos é transformada em um deploy geralmente por três estágios:

* **build**, que consiste em pegar uma versão específica do repositório com suas dependências e transformá-la em uma versão executável, seja por compilação ou empacotamento

* **release**, que consiste no envio de um build para um servidor e a execução de todas as configurações necessárias para que ele entre no terceiro estágio

* **execução**, que é a inicialização de todos os processos necessários para que ele esteja disponível para os usuários acessarem.

Ter estas três fases bem separadas e definidas facilita a criação de scripts e procedimentos a serem executados em cada uma delas. Ferramentas como Capistrano, Ant, Deployer, Grunt, etc, podem ser usadas em conjunto ou em separado para tornar estes procedimentos automatizados e versáteis.

## VI. Processes

Pense em seu aplicativo como um ou mais processos, que sejam “stateless” e “share-nothing”. A ideia é diminuir o acoplamento entre os componentes do projeto para facilitar a escala. Um exemplo bem comum é armazenarmos as sessões dos usuários no próprio servidor Apache, ou usarmos o APC como cache. O problema é que esta abordagem mantém o código atrelado a apenas um servidor e com isso torna complexo a escalabilidade, o fato de novos servidores serem adicionados ou removidos da arquitetura. O ideal é usar algo como um Memcached, Redis ou mesmo um banco de dados para diminuir o acoplamento.

## VII. Port binding

A ideia é que o projeto não dependa de um servidor externo como o Apache para ser executado, mas que possa ser auto-contido e executar em uma porta específica que seria acessado por outras partes do projeto ou mesmo pelos usuários. Em um projeto Python poderíamos usar o Tornado, em um projeto Ruby o Thin, e assim por diante.
Trazendo um pouco esse conceito para o ambiente PHP eu faria uma pequena mudança indicando para que o projeto dependa de uma estrutura de nomes e endereços que podem ser configurados em arquivos de configuração. Por exemplo, em uma máquina de desenvolvimento os componentes do projeto poderiam ser:

```http://coderockr.dev | http://api.coderockr.dev | http://admin.coderockr.dev```

Ou

```http://coderockr.dev | http://coderockr.dev:8080 | http://coderockr.dev:8888```

Sendo que os três endereços podem ser configurados dentro do Apache usando o conceito de VirtualHost ao invés de criarmos um servidor próprio para cada componente, como é comum em projetos Python ou NodeJS.
E no servidor de produção o arquivo de configuração apontaria para endereços diferentes, sem alteração de código, como comentado no item III.

## VIII. Concurrency

Tente pensar em seu projeto na forma de processos, como citado anteriormente, que podem ser executados em paralelo. Por exemplo, uma requisição HTTP normal pode ser executada por um processo do Apache enquanto que tarefas que demoram mais tempo podem ser encaminhadas a uma fila de processos e executadas em backgroud. Exemplos como Gearman e RabbitMQ são comuns para executar este tipo de procedimento.

## IX. Disposability

Um projeto que segue os 12 fatores possui diversos processos facilmente descartáveis, ou seja, que podem ser iniciados ou parados a qualquer momento. Desenvolva-os de forma a facilitar este processo, permitindo início rápido e tornando o processo de finalização simplificado e sem traumas como perda de dados.

## X. Dev/prod parity

Mantenha os ambientes de desenvolvimento, homologação e produção o mais similares possíveis. Os 12 fatores auxiliam no processo de deploy contínuo e esta similaridade evita erros durante o processo de envio do desenvolvimento para produção. Ferramentas como o Vagrant, Ansible, Puppet e Docker são muito úteis e importantes neste processo.

## XI. Logs

Os logs são uma importante ferramenta para entender o comportamento de um projeto, identificando erros e pontos de melhoria. Normalmente tratamos os logs simplesmente escrevendo as mensagens em arquivos para futura análise, mas a dica aqui é que o código não deve se importar tanto com o formato de armazenamento. O projeto deve apenas enviar as mensagens para a saída padrão e esta deve ser redirecionada para locais específicos de acordo com o ambiente onde o projeto está executando. Em uma máquina de desenvolvimento isso pode ser um arquivo mas em um servidor de produção podemos usar algo mais complexo, como um Hadoop ou um serviço externo. Ferramentas como o Monolog permitem que os logs sejam tratados como streams de dados e redirecionados para diferentes localizações.

## XII. Admin processes

Tarefas adiminstrativas devem ser tratadas de forma automatizada. Como agora nosso projeto pode rodar em diversos servidores, com diversos processos, coisas como limpar caches, carregar dados, atualizar bases de dados, etc, também devem ser facilitadas. O PHP possui uma interface CLI (Command Line Interface) que nos permite criar scripts poderosos e que facilitam esse processo. E frameworks como o Doctrine, Symfony, CakePHP, etc, fornecem uma série de facilidades para a criação destas tarefas.

## Conclusão

Estes fatores não são uma verdade absoluta e tampouco devem ser seguidos cegamente, mas são um ótimo ponto de partida para a criação de projetos web modernos e que podem escalar mais facilmente para atender as demandas das empresas.
