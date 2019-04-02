---
title: "Iniciando com React - #2 Criando a Estrutura do Projeto"
description: "Montar a estrutura de um projeto React e configurar o build manualmente, pode ser um pouco confuso de início. Por esse motivo, iremos utilizar o comando..."
author: "Vinicius Dacal"
date: 2017-03-20
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "Arquitetura", "JS", "React", "Programação Funcional"]
aliases: [
    "/iniciando-com-react----2-criando-a-estrutura-do-projeto-2c3b0f8e9f9",
    "/iniciando-com-react-2-criando-a-estrutura-do-projeto-2c3b0f8e9f9",
]
---

Montar a estrutura de um projeto React e configurar o build manualmente, pode ser um pouco confuso de início. Por esse motivo, iremos utilizar o comando [create-react-app](https://github.com/facebookincubator/create-react-app), que gera por padrão uma estrutura básica, pronta para começarmos a desenvolver nossa aplicação. Dessa forma, você não precisa entender todo o processo do build antes mesmo de criar o seu primeiro componente.

Posteriormente, veremos cada parte do build para entender melhor com o que estamos lidando.

## Instalando Create React
> Para a instalação do CLI é necessário que você já tenha o Node e o NPM instalados na sua máquina. Para verificar se eles estão instalados, execute os seguintes comandos no terminal:
> $ node -v
$ npm -v
> É aconselhável que a versão do Node seja igual ou superior à 5.10.0 e do NPM seja igual ou superior à 3.0.0.
> Caso você ainda não tenha o Node e o NPM instalados, faça [download](https://nodejs.org/en/) pelo site e siga os passos do instalador: [https://nodejs.org/en/](https://nodejs.org/en/)

![output dos comandos node -v e npm -v](https://cdn-images-1.medium.com/max/3596/1*AXOcEwch-O1PSlGHExevug.png)*output dos comandos node -v e npm -v*

Com o **node** e o **npm** instalados, execute o seguinte comando:

    **$** npm install -g create-react-app

O comando acima instala globalmente o módulo *create-react-app*, para que o mesmo fique acessível pelo terminal. O Processo de instalação deve finalizar em alguns minutos.

## Criando o primeiro projeto

Com o comando abaixo, iremos criar um projeto chamado **my-app**:

    **$** create-react-app my-app

O comando acima gera a estrutura do nosso projeto e a coloca em uma nova pasta com o mesmo nome, **my-app**.

Vamos entrar no diretório do projeto utilizando o comando:

    **$** cd my-app

E por fim, iniciaremos o build da aplicação.

    **$** npm start

Além de iniciar o build da aplicação, o comando acima levanta um servidor e fica escutando por alterações nos arquivos.

Já é possível acessar a url [http://localhost:3000](http://localhost:3000/) no navegador e ver uma tela de boas vindas:

![](https://cdn-images-1.medium.com/max/3868/1*rGbFOCkAfwshq56M6_11Gw.png)

## Explorando a estrutura do projeto

Há inúmeras maneiras de organizar um projeto React, mas esse não é o foco desse post. Aqui, iremos explorar a estrutura default para entendermos como as partes se conectam, e o que realmente é necessário para o bootstrap do React.

O *create-react-app* cria a seguinte estrutura inicial:

    my-app/
      README.md
      node_modules/
      **package.json**
      .gitignore
      public/
        favicon.ico
        **index.html**
      src/
        **App.css
        App.js
        App.test.js
        index.css
        index.js**
        **logo.svg**

Vamos manter nosso foco nos arquivos **package.json**, **public/index.html** e nos arquivos da pasta **src**.

### Scripts e dependências

Observe abaixo o conteúdo do **package.json** que lista as dependências do projeto e possui alguns *aliases* para os scripts envolvidos no build:

{{< gist viniciusdacal 58eca7aab9f7796e69047c815455aa33>}}

Como podemos ver no arquivo acima, temos os módulos react e react-dom declarados como dependência, e o react-scripts declarado como uma dependência de desenvolvimento. O react-scripts é o módulo que encapsula todos os scripts e configs do build.

Também podemos observar que estão sendo declarados alguns scripts, que possuem funcionalidades conforme listadas abaixo:

* **start:** Inicia o build no modo de desenvolvimento.

* **build**: Executa o build do projeto otimizado para produção.

* **test**: Executa os testes do projeto.

* **eject**: Traz para dentro do nosso projeto, toda a configuração que o react-scripts abstrai. **Não use este comando por enquanto! **Utilizaremos mais a frente para estudar o processo do build.

### Index

Observe abaixo o código do arquivo **public/index.html**:

{{< gist viniciusdacal 5323369a80dc1dc2448632812a14287b>}}

O arquivo **index.html** vem com a marcação mínima necessária para iniciar nossa aplicação. Porém, é possível observar que não há tags de scripts ou estilos. Não se preocupe, as mesmas serão injetadas automaticamente no build.

Por ora a única coisa importante a observar é a tag div com o id** root** na linha #10, é ela que o *React* irá utilizar para renderizar nossa aplicação.

### Componente raiz

Vamos explorar o conteúdo da pasta **src**. Observe abaixo o arquivo **App.js**, que contém o componente raiz da aplicação:

{{< gist viniciusdacal b24adeb867b11dadc985cc1afb09cc55>}}

O componente acima é definido com a *class e extende a classe* Component do React. Existem duas formas de definir componentes, através de *functions* ou através de *class*. Em um futuro post veremos as diferenças entre uma e outra.

Um componente deve sempre implementar um método ***render***, que retorna um ***JSX*** do que deve ser mostrado na tela, ou **null** quando não deve mostrar nada.

### JSX

O que parece ser um HTML dentro do método **render**, é na verdade [JSX](https://facebook.github.io/react/docs/introducing-jsx.html), um [sintatic sugar](https://pt.wikipedia.org/wiki/A%C3%A7%C3%BAcar_sint%C3%A1tico) para a API do React. As principais diferenças no dia-a-dia entre HTML e JSX são:

* O **class** do html passa a se chamar **className**, porque o termo *class* é uma palavra reservada no Javascript.

* O **for** da tag *label passa a se chamar **htmlFor**, pelo mesmo motivo do *class*, de ser uma palavra reservada.

* O conteúdo que estiver entre chaves {}, será interpretado como Javascript.

* Todos os atributos são nomeados em [lower camelcase](https://pt.wikipedia.org/wiki/CamelCase).Sendo assim, atributos como onclick, passam a se chamar [onClick](https://facebook.github.io/react/docs/handling-events.html). O mesmo vale para atributos que utilizam hífen -, stroke-width por exemplo, passa a se chamar strokeWidth.

* Todo conteúdo do retorno de um render, deve estar dentro de um único wrapper, caso contrário o seguinte erro é apresentado: Adjacent JSX elements must be wrapped in an enclosing tag while parsing file.

É importante lembrar que o JSX é convertido em um código React, por esse motivo, devemos importar o módulo React em todo arquivo que utiliza a sintaxe.

> JSX já é utilizado por outras libs além do React e vem se tornando um padrão para definição de marcação de componentes.

> É possível seguir em frente com as informações apresentadas acima. Mas se você ficou instigado a saber mais sobre JSX, acesse: [facebook.github.io/react/docs/introducing-jsx.html](https://facebook.github.io/react/docs/introducing-jsx.html).

### Importação de arquivos de estilos e imagens

Junto com a base do projeto, já vêm configurados os [loaders](https://webpack.github.io/docs/loaders.html) para os formatos **svg** e **css.** Quando importamos um arquivo **css**, o conteúdo do mesmo é injetado na nossa aplicação, permitindo assim que utilizemos os estilos e classes em nossos componentes.

Já o **svg** funciona de uma maneira um pouco diferente. Ao importarmos um **svg**, obteremos uma referência, um caminho para o arquivo, e poderemos utilizar essa referência em tags como **<img>** e **<object>**. A importação de outros arquivos de imagem como **jpg**, **png** e **gif** funcionam dessa mesma maneira, porém, nessa estrutura inicial apenas o **svg** é suportado.

### Bootstrap da aplicação

Vamos observar abaixo o código do arquivo index.js, é nele que ocorre a inicialização da nossa aplicação:

{{< gist viniciusdacal 04a81b9bead72c3c97e8bc3dbddd0907>}}

Na linha #2 do arquivo acima, estamos importando o **ReactDOM**, o módulo do React responsável pela manipulação do *DOM*.

Na linha #3, importamos nosso componente raiz, o **App.js** que acabamos de ver mais acima.

O *bootstrap* do React se baseia em você dizer para ele, o que renderizar e onde injetar o que ele renderizou. Estamos fazendo isso na linha #6, através do método render do **ReactDOM**, que espera como primeiro parâmetro um componente, e como segundo parâmetro, um elemento do *DOM* que será utilizado para injetar todo o HTML renderizado.

![O React renderiza os componentes e gera um output HTML](https://cdn-images-1.medium.com/max/2000/1*2gZ3Z_hfr-9zaFkN474kUQ.png)*O React renderiza os componentes e gera um output HTML*

No nosso caso, estamos passando o componente **App** para ser o raiz e o elemento que possui o id=”root” para ser o que conterá toda a aplicação. Lembra da nossa tag no arquivo **public/index.html**? Ela mesmo!

> Não abordamos o conteúdo dos arquivos css e svg, pois os mesmos não possuem nenhuma particularidade quanto ao React. Também não abordamos a parte de testes, porque futuramente haverá um post específico sobre este tema. Mas nada impede que você os explore. Seja curioso!

## Conclusão

Aprendendo como criar a estrutura do projeto e a entendendo bem, torna os próximos passos muito mais fáceis. O **React** tem alguns princípios básicos, que bem aprendidos, permitem que construamos desde aplicações simples até as mais robustas.

No próximo post veremos como criar componentes e aprenderemos sobre o ciclo de vida de um componente React. **Siga-nos e não perca os próximos posts!**
