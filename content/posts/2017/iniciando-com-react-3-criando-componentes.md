---
title: "Iniciando com React - #3 Criando Componentes"
description: "O React introduz uma nova maneira de construir interfaces, o que podemos chamar de “Interface baseada em componentes”. Componentes permitem que você divida sua interface em partes independentes..."
author: "Vinicius Dacal"
date: 2017-03-29
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "JS", "React", "Programação Funcional"]
---

O **React** introduz uma nova maneira de construir interfaces, o que podemos chamar de “Interface baseada em componentes”. Componentes permitem que você divida sua interface em partes independentes e reutilizáveis, e que você pense em cada parte de forma isolada.

> Nota: Este post faz parte da série “Iniciando com React”. Se você está começando com React agora, sugiro ler os outros posts primeiro.

[< Anterior](https://blog.coderockr.com/iniciando-com-react-2-criando-a-estrutura-do-projeto-2c3b0f8e9f9) | [Proximo >](https://blog.coderockr.com/iniciando-com-react-4-armazenando-estado-e-entendendo-o-lifecycle-bbb92b27da83)

Componentes podem possuir diversos formatos e ter diferentes responsabilidades. Um componente pode ser desde um botão, um formulário ou até uma página completa.

O React nos permite definir e reutilizar componentes de maneira simples, conforme veremos ao longo desse post.

## Functional Components

A forma mais fácil de definir componentes é através de funções Javascript, conforme o exemplo abaixo:

{{< gist viniciusdacal 5d73a9b3823a139a97de7d5e8531b5ad>}}

Quando o componente é definido em forma de função, o React cria o componente executando essa função e passando as **props** como primeiro parâmetro. Ele espera um retorno em **JSX** do que deve ser renderizado, ou **null** no caso de não mostrar nada. Componentes React definidos da forma descrita acima, ganham o nome de *Functional Components.*

## Props

A forma principal de comunicação entre componentes é utilizando as **props. **Elas são propriedades/atributos passados para os componentes filhos. Observe o exemplo abaixo:

{{< gist viniciusdacal 84d0089114a77ea7861cd3bd3c2eb12e>}}

No exemplo acima, na linha #4, estamos novamente definindo o componente Welcome. Na linha #11, estamos utilizando esse componente e passando para ele a prop **name** que possui como valor, a string “**Stranger**”.

### Valores Dinâmicos

Podemos também passar variáveis para as props, utilizando chaves {}:

{{< gist viniciusdacal b9109e1096fee4b50b806f701ec38fff>}}

Observe as linhas #10 e #13 do exemplo acima. Primeiro definimos uma const **strangerName** e depois passamos ela para a props **name**. No JSX, todo o conteúdo que estiver dentro de chaves será interpretado como Javascript.
> O ES6 trouxe uma feature chamada [Object Detructuring](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Atribuicao_via_desestruturacao#Desestruração_de_objeto), e é ela que estamos utilizando na linha #4 ({ name }). Basicamente, o que ela faz é extrair os atributos de um objeto e criar variáveis com o mesmo nome. Dessa forma, podemos utilizar **name** aos invés de **props.name**.

### Funções callbacks

É possível também, passar funções através das **props**. Normalmente fazemos isso quando precisamos passar um **callback** para um componente.
> Um callback é em suma, uma função que passamos como parâmetro para outra função, que por sua vez poderá executar o callback quando algum determinado evento ocorrer, como o final de um request, um evento de click do usuário e etc.

<iframe src="https://medium.com/media/7569c189f1727e93d69fb6db6d7cdb77" frameborder=0></iframe>

No arquivo acima, não estamos definindo nenhum novo componente, apenas utilizando o elemento button, comum do HTML.

De acordo com o exemplo acima, cada vez que o usuário clicar no botão, um alerta será mostrado na tela.

O React suporta [alguns eventos básicos](https://facebook.github.io/react/docs/events.html) de interação de acordo com o componente HTML que utilizarmos, como onClick, onChange, onKeyPress, etc…

### Children

A **prop** “**children” **é um pouco diferente das outras.** **Ela** **é o conteúdo que passamos dentro da tag:

    <div>**Esse conteúdo será a prop children**</div>

Conforme o exemplo acima, todo o conteúdo que está dentro da div, será o valor de **props.children**.

Conforme o exemplo que veremos logo abaixo, é possível passar **children** explicitamente, porém, esse modo é pouco utilizado. A forma abaixo é equivalente à anterior:

    <div children='**Esse conteúdo será a prop children**' />

## Button

Vamos rever um exemplo similar ao que foi apresentado no primeiro post da série, explorando em detalhes o conteúdo:

<iframe src="https://medium.com/media/3b684b5593b7a9cbfac399ece376e750" frameborder=0></iframe>

Como podemos ver no exemplo acima, na linha #4 estamos definindo um componente chamado **Button**. Mais abaixo na linha #13 estamos utilizando ele, passando o children “**Example” **e uma função para **onClick**, que quando executada, exibirá “You just clicked” no console do navegador.

Na definição do componente na linha #4, estamos esperando as props **children**, **onClick** e **type**, e especificamos a string **“button”**, para ser o [valor padrão](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Functions/Parametros_Predefinidos) da prop **type**.

Na linha #6, estamos utilizando o elemento button, passando para ele a prop **type** e passando a prop **className**, que é equivalente ao atributo [**class** do HTML](https://developer.mozilla.org/pt-BR/docs/Web/HTML/Global_attributes/class). No **className**, estamos passando a classe “**btn”**, que seria utilizada no CSS para definirmos o estilo do nosso botão. Além disso, repassamos a prop **onClick**.

Na linha #7, apenas repassamos o children que recebemos.
> **Dica: **Sempre inicie o nome de componentes com uma letra maiúscula.
> Por exemplo, <**b**utton /> representa um elemento HTML padrão, mas <**B**utton /> representa um componente e exige que a variável **Button** esteja no escopo.

## Composição de componentes

É possível criar componentes que reutilizam outros. Dessa forma, podemos criar pequenos componentes que são úteis em várias partes de uma aplicação e então, compor esses componentes quando precisamos criar componentes mais complexos.

<iframe src="https://medium.com/media/7f6e5ef49c5d452709e6c81daa18ba5b" frameborder=0></iframe>

Conforme é possível ver no exemplo acima, possuímos dois componentes: **Button** e **GroupButton**. Primeiro temos o componente mais simples, **Button**, que pode ser reaproveitado em qualquer parte da aplicação. Depois temos o **ButtonGroup**, onde reutilizamos **Button** para criarmos um componente maior.
> Dica: Componentes devem retornar um único elemento raiz. Esse é um dos motivos de termos colocado uma*<div>* para conter todos os componentes *<Button />*.

## Props são Read-Only

De qualquer maneira que você declare componentes, seja através de classes ou funções, você nunca deve alterar o valor de **props**. Os componentes devem ser escritos como ***funções puras, ***de modo que o valor de suas **props** permaneça intacto:

Observe abaixo um exemplo de função pura:

    function sum(**a**, **b**) {
      return **a** + **b**;
    }

Na função acima, nós recebemos os parâmetros **a** e **b **e retornamos a sua soma. Funções como essa, são consideradas ***funções puras***, pois chamadas com os mesmos parâmetros, sempre retornarão os mesmos resultados, e elas também não alteram o valor dos seus parâmetros ou valores que estão fora de seu escopo.

Já o exemplo abaixo é considerado uma função impura, porque na linha #2, estamos reatribuindo o valor de **result.a**, objeto que foi criado fora do escopo atual:

    1  function sum(result, b) {
    2    **result**.**a** += **b**;
    3    return **result**;
    4  }

## Conclusão

***Functional components*** como vimos até agora, não guardam muita lógica dentro de si. Eles normalmente só guardam uma marcação específica do componente. Quando possuem lógica, é a mínima possível.

Sendo assim, *functional components* são perfeitos para construir componentes simples de apresentação, como botões, labels, listas e etc. Quando precisamos construir componentes que envolvem lógica e precisam guardar estado, precisamos criar componentes utilizando ***class**, *e é isso que veremos no próximo post. **Siga-nos e não perca!**

Gostou do post e achou útil? Dê um **like **❤️ abaixo para ajudar na divulgação e para que mais pessoas tenham acesso :)
