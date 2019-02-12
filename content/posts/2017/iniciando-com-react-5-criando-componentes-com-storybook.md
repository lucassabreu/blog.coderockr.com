---
title: "Iniciando com React - #5 Criando Componentes com Storybook"
description: "Quando estamos desenvolvendo um novo componente para um projeto, é comum o fazermos diretamente na aplicação. Essa abordagem pode nos levar à um processo exaustivo..."
author: "Vinicius Dacal"
date: 2017-11-03
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "JS", "React", "Programação Funcional"]
---

Iniciando com React - 5 Criando componentes com Storybook

Quando estamos desenvolvendo um novo componente para um projeto, é comum o fazermos diretamente na aplicação. Essa abordagem pode nos levar à um processo exaustivo, onde as vezes não temos os dados necessários a nossa disposição, ou situações onde temos que preencher todos os campos de um formulário, só para ver se a mensagem de erro está correta. O ***React Storybook** *vem para solucionar esses problemas e deixar o nosso workflow muito mais fluído.
> Nota: Este post faz parte da série “Iniciando com React”. Se você está começando com React agora, sugiro ler os outros posts primeiro.
[< Anterior](https://blog.coderockr.com/iniciando-com-react-4-armazenando-estado-e-entendendo-o-lifecycle-bbb92b27da83)

## O que é o Storybook?

O storybook é uma ferramenta que nos permite desenvolver e testar componentes fora da nossa aplicação. É possível criar componentes completos utilizando mocks de dados para simular diferentes estados e diferentes comportamentos. Observe o exemplo abaixo:

![Exemplo do Storybook funcionando](https://cdn-images-1.medium.com/max/4312/1*TxuoKupMwNqsEKyrCdB9cQ.gif)*Exemplo do Storybook funcionando*

O exemplo acima da um bom overview de como é a ferramenta e qual é a sua proposta. Sem mais delongas, vamos para o processo de instalação.

## Instalação

O Storybook possui um CLI para ser utilizado no terminal, que funciona de forma muito similar ao [create-react-app](https://github.com/facebookincubator/create-react-app). Os passos abaixo levam em consideração um ambiente com *Node* e *NPM* instalados, conforme já vimos no [segundo post](https://blog.coderockr.com/iniciando-com-react-2-criando-a-estrutura-do-projeto-2c3b0f8e9f9#b6a7). Também é importante que você já possua um projeto com o ambiente configurado, conforme explicado também no [segundo post](https://blog.coderockr.com/iniciando-com-react-2-criando-a-estrutura-do-projeto-2c3b0f8e9f9).

O primeiro passo, é instalarmos o nosso CLI globalmente, utilizando o comando abaixo:

    npm i -g @storybook/cli

Após terminar a instalação, basta irmos para a pasta do nosso projeto:

    cd path/to/my-app

Dentro da pasta do projeto, executamos o comando abaixo, que criará toda a configuração necessária e instalará todas as dependências:

    getstorybook

O comando acima, também insere o script **storybook **no nosso arquivo **package.json.**

Terminada a execução do comando, é possível testar o storybook executando:

    npm run storybook

E acessar a url indicada, que no meu caso é [http://localhost:9009/](http://localhost:9009/)

![](https://cdn-images-1.medium.com/max/4100/1*2PBQEmkAm5hc8Uy5ChjCaw.png)

Ao acessar a url indicada, é possível visualizar o storybook com algumas stories de exemplo:

![](https://cdn-images-1.medium.com/max/5568/1*1d3PtybwXhSjopYjnTZ89A.png)

É possível navegar no menu lateral e ver o componente **Button** funcionando.

Se acessarmos nosso projeto, podemos observar que foi criado uma pasta .storybook dentro do diretório raiz da aplicação. Essa pasta contém arquivos referente as configurações do storybook.

O arquivo .storybook/config.js é o responsável por carregar as stories do projeto e ele contém o seguinte código:

<iframe src="https://medium.com/media/f05ffbaa27b8e98d22e125eab3dc44ca" frameborder=0></iframe>

A função loadStories importa o arquivo src/stories/index.js, que por sua vez, contém a definição das stories, conforme abaixo:

<iframe src="https://medium.com/media/449a766bcf133e21ff727ddbd50d0be0" frameborder=0></iframe>

Cada componente tem suas próprias stories, de modo que cada uma represente um estado dele. Com isso, podemos construir e testar cada componente de forma isolada, o que nos leva a ter mais controle no processo de desenvolvimento.

Seguindo o código acima, o método **storiesOf** é responsável por inicializar as stories de um componente, e ele espera como primeiro parâmetro o nome do componente (que é o nome que aparecerá naquele menu lateral) e como segundo parâmetro, a variável global **module**. A funcão **storiesOf** retorna um objeto que contém o método **add**, que será utilizado para adicionar novas stories ao componente.

No exemplo acima, está sendo definido uma story para o componente **Welcome**, com a descrição “to Storybook”. Também estão sendo defininidas as stories para o componente Button: “with text” e “with some emoji”.

Outra coisa que podemos ver no arquivo de exemplo, é o uso da função **action** e **linkTo**. A funcão **action **é utilizada para criar funções mocks que podem ser passadas como callbacks através das props. Quando os mocks são executados, eles geram um log que aparece no storybook, o que é útil para debugging. A função **linkTo** por sua vez, cria links para outras stories.

Agora vamos ver exemplos que se aproximam mais de casos reais.

## Button

Vamos criar um componente Button, que possui algumas variações de estado:

Primeiramente, crie uma pasta **Button** dentro de **src**. Dentro dela crie um arquivo **Button.jsx** e outro arquivo **Button.css.** A estrutura desses arquivos deve estar da seguinte maneira:

    src/
      Button/
        Button.jsx
        Button.css

O código para o componente será o seguinte:

<iframe src="https://medium.com/media/03f6d304fc529d69683f5137259aca2d" frameborder=0></iframe>

No código acima, nós definimos o componente Button, que aceita as props: **children**, **onClick** e **disabled**.

Essas três props nós repassamos para o elemento button do html. Porém, nós também utilizamos a prop **disabled** para inserir ou não o class name: button--disabled que será utilizado para estilizar o componente no respectivo estado.

Para estilizar o componente acima, o arquivo **Button.css** deve conter o seguinte código:

<iframe src="https://medium.com/media/7e9732dd7b5379f251e2a5b604f8c730" frameborder=0></iframe>

O arquivo acima possui um estilo padrão para o componente através da classe .button e possui um estilo adicional para o estado **disabled, **através da classe .button--disabled

Por último, criaremos as stories do nosso componente. Substitua o código do arquivo src/stories/index.js pelo seguinte:

<iframe src="https://medium.com/media/7786bdb5cb2157e0ec99d88dc756eff0" frameborder=0></iframe>

No arquivo acima, nós importamos o componente **Button** e criamos duas stories para o mesmo. Cada story possui uma utilização básica do componente. A primeira story nós só passamos um callback para a prop **onClick**, e sendo assim, o button assumirá seu estado padrão. Na segunda story, além da prop **onClick**, nós passamos a prop **disabled**, para que nessa story o componente assuma o respectivo estado.

Feito isso, acesse novamente o storybook no seu navegador e veja o resultado.

![](https://cdn-images-1.medium.com/max/5568/1*sF0FMX51biXcNGJ4OHYc9A.png)

![](https://cdn-images-1.medium.com/max/5568/1*_1kGCyQo7DWgmpxJS1Vz8Q.png)

No geral, as stories funcionam basicamente assim: Você importa o componente, define uma story e utiliza o mesmo passando props e dados mocados para para simular um estado.

## Carregando stories dinamicamente

Até aqui nós vimos como criar uma story. Porém, seria falta de organização e pouco produtivo manter as stories dos componentes todas em um único arquivo. O ideal é que possamos criar um arquivo de stories para cada componente e que não precisemos importar cada arquivo de stories manualmente. Vamos ver como podemos chegar nesse cenário.

Primeiramente, crie o arquivo Button.stories.jsx dentro da pasta do componente Button e mova para ele o conteúdo do arquivosrc/stories/index.js, corrigindo o caminho do import do componente. O arquivo em questão ficará com o seguinte código:

<iframe src="https://medium.com/media/a69d80d1c33e932d25a5dd81f29e79e3" frameborder=0></iframe>

Feito isso, podemos remover completamente a pasta src/stories

Em seguida, iremos alterar o arquivo .storybook/config.js para carregar dinamicamente todos os arquivos que possuem .stories.jsx em seu nome. O arquivo final ficará da seguinte maneira:

<iframe src="https://medium.com/media/805b5fceecd3ae4dd2c15051ba3a2615" frameborder=0></iframe>

No arquivo acima, nós utilizamos require.context para carregar todos os arquivos que respeitem o pattern que definimos. Com isso, para criar um novo arquivo de stories, basta criá-lo dentro da pasta src ou de qualquer sub-pasta dela e incluir .stories.jsx no final do nome do arquivo

## Conclusão

Vimos que é possível criar cada componente de forma independente, isso nos dá mais controle do desenvolvimento e também nos compele a construir componentes mais resilientes, pensando em como eles irão se comportar em cada situação.

Aproveite que as stories estão configuradas e carregando automaticamente e crie um novo componente com novas stories. A melhor maneira de aprender e fixar o conhecimento, é praticando.

Gostou do post e achou útil? Ajude a divulgar para que mais pessoas tenham acesso! ❤️ ️
