---
title: "Como React e Redux me Fizeram um Programador Melhor"
description: "Há uns dias atrás, no primeiro hangout do React Cast, apareceu o assunto sobre como React e Redux nos faziam ser programadores melhores. Nesse post vou esclarecer os pontos que me levam a acreditar nisso..."
author: "Vinicius Dacal"
date: 2016-10-19
draft: false
categories: ["Desenvolvimento", "Carreira"]
tags: ["Carreira", "JS", "React", "Redux", "Programação Funcional"]
---

Há uns dias atrás, no primeiro hangout do React Cast, apareceu o assunto sobre como React e Redux nos faziam ser programadores melhores. Nesse post vou esclarecer os pontos que me levam a acreditar nisso.

![](https://cdn-images-1.medium.com/max/3840/1*SgIzRKXaGHYYpQzYuVLStQ.jpeg)

Quando começamos a trabalhar como programadores, é muito comum aprendermos a trabalhar com abstrações que facilitam nosso caminho e nossa curva de aprendizado. Um grande exemplo disso é o [*jQuery*](https://jquery.com/), que provavelmente é a primeira lib que os desenvolvedores Frontend tem contato.

Abstrações são ótimas, elas podem facilitar muito as nossas vidas e aumentar nossa produtividade. Porém, é muito comum programadores ficarem presos às abstrações e esquecerem de aprender os fundamentos básicos referentes a tecnologia abstraída. Os programadores se prendem aos vícios das abstrações e ficam completamente dependente delas.

Existe esse meme clássico que exemplifica claramente isso:

![](https://cdn-images-1.medium.com/max/2048/1*sfHJGjVXCvqQEm_O2ddNFQ.gif)

É importante você saber pensar por fora das abstrações, para saber quando ou não utilizá-las.

> Se a única ferramenta que você tem é um martelo, para você tudo começa a se parecer com um prego.

Como é o caso de inúmeras animações ainda feitas em *jQuery*, mas que facilmente poderiam ser substituídas por CSS.

![](https://cdn-images-1.medium.com/max/2000/1*7ZjdFUDfP4X_OdzsrRyXFw.jpeg)

> **Disclaimer**: Antes de continuar este artigo, deixo claro que eu trabalhei mais de um ano com Angular, em grandes aplicações, antes de começar a trabalhar com React, e ainda trabalho com o mesmo em aplicações já em produção. Falo aqui como alguém que antes defendia muito o Framework e adorava trabalhar com ele.

Trabalhando com [*Angular*](https://angularjs.org/) e outros Frameworks, nós acabamos seguido p0r esse mesmo caminho, de pensar como resolver problema **X** com o framework, antes mesmo de pensar como poderíamos resolver sem ele.

Um claro exemplo que eu poderia dar, seria o **angular**.[forEach](https://docs.angularjs.org/api/ng/function/angular.forEach). Nós poderíamos simplesmente utilizar **Array**.[forEach](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach), nativo do Javascript.

Eu sei que há casos em que você precisa iterar sobre um objeto e não um *array*, para isso, bastaria utilizar [Object.keys](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)(Array).**forEach**, também nativo. Mas com o Angular é mais cómodo, não é?

Nós não paramos por aí, utilizamos factory, service, session, value, constant, dependency Injection, *tudo isso específico do angular. Quando precisamos fazer uma integração específica, como integrar a aplicação com um serviço de [*websocket*](https://developer.mozilla.org/pt-BR/docs/WebSockets), utilizamos módulos como [*Angular Socket IO*](https://github.com/btford/angular-socket-io), ao invés de utilizar diretamente o [*socket.io-client*](https://github.com/socketio/socket.io-client). Aos poucos você olha para a sua aplicação e percebe que ela está totalmente acoplada e dependente do framework.

Você pode vir com o argumento que é possível deixar a aplicação menos acoplada e que esses módulos na maioria das vezes só lidam com a natureza assíncrona do *Angular*. Mas tentar deixar o *Angular* menos acoplado, utilizando funções construtoras ao invés de factories e ai por diante, implica em ainda ter que implementar inúmeros workarounds ao longo do código, como por exemplo: [*$scope.$apply*](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$apply) para sincronizar o [*digest cycle*](https://docs.angularjs.org/guide/scope). Sem contar que esse não é o *Angular* way de trabalhar e seria considerado uma má prática.

E mesmo contornando essas dependências de inúmeras formas, no final do dia você ainda não conseguiria rodar nenhum teste unitário sem ter o *Angular* como dependência, pelo menos, não de uma forma viável.

Eu sei, peguei bastante no pé do *Angular*, mas justamente por ele ser um dos frameworks mais populares, por eu ter mais experiência com o mesmo, e porque utilizando-o como exemplo, é fácil expor os problemas que é se amarrar a um framework.

Ter um framework que cobre de ponta a ponta da sua aplicação é ruim, porque toda a aplicação tende a ser desenvolvida da maneira dele. Se amanhã surge uma lib que resolve uma parte do problema de uma maneira melhor que o seu framework, dificilmente você vai conseguir migrar somente uma responsabilidade para a lib, pelo menos, não de forma fluída.

Vemos esse mesmo tipo de opinião também no Backend. No post [**O fim da era dos frameworks full stack**](http://eltonminetto.net/2016/03/15/o-fim-da-era-dos-frameworks-full-stack/) o [Elton Minetto](undefined) fala sobre o assunto.

O problema em si não é o *Angular*, mas todo framework que te força a trabalhar da maneira dele e que você precisa “comprar o pacote completo” para se beneficiar apenas de uma feature ou outra.

## Arquitetura desacoplada

Agora que conseguimos entender o que é um cenário totalmente acoplado, fica mais fácil compreender as vantagens de uma arquitetura desacoplada.

### Separation of concerns(SoC)

![Brinquedo de Lego](https://cdn-images-1.medium.com/max/2048/1*QaJLbOURQAugLzPHLqreuQ.jpeg)*Brinquedo de Lego*

[*Separation of Concerns*](https://en.wikipedia.org/wiki/Separation_of_concerns), ou *Separação de conceitos*, termo cunhado por [Edsger W. Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra), em 1974.

No básico, se refere a identificar as responsabilidades que existem dentro de uma aplicação, isolá-las e delegá-las, de uma forma que fique claro o que cada parte do seu software faz e que fique claro onde deve ser feito cada coisa.

A adoção desse princípio traz inúmeras vantagens como: Facilidade na manutenção, facilidade para encontrar e resolver bugs e facilidade para compreender o software como um todo.

Utilizando corretamente esse conceito, fica fácil substituir determinada parte do seu software quando necessário.

> ## Seu software deve parecer com um brinquedo de lego, onde você pode tirar uma peça amarela, colocar uma azul e ele continua funcionando, se limitando apenas a possuir as mesmas entradas e saídas.

## Single Page Applications

Vamos tentar distinguir os aspectos e as responsabilidades dentro de uma [**SPA**](https://en.wikipedia.org/wiki/Single-page_application).

Nós podemos separar um SPA em três partes: **State**, **View** e **Actions.**

### State

Todos os dados que existem dentro da nossa aplicação, desde os dados buscados por requests, até os dados definidos estaticamente no código.

### View

Basicamente, é nosso HTML e CSS, que são renderizados de acordo com os dados que existem dentro da nossa aplicação.

### Actions

São os eventos que ocorrem dentro da nossa aplicação, que poderão ou não alterar o *State*.

Podemos dividir esses eventos em duas categorias:

**— Interação do usuário:** Click em botão, entrada de dados em um formulário, etc…

**— IO:** Eventos de rede, como início e fim de um request.

A forma como cada uma dessas partes se integram é bem simples. O **State** fica responsável por armazenar os dados e entregá-los para a **View.** A **View** por sua vez, irá se renderizar de acordo com os dados que recebe do **State**. As **Actions,** ao ocorrerem, poderão causar uma alteração no **State**, que entrega os novos dados para **View** e assim segue esse ciclo.

Essas características podem ser vistas em qualquer **SPA**, independente da tecnologia em que seja desenvolvida. O problema é que muitos frameworks deixam essas três características totalmente acopladas, não sendo possível substituir o “responsável” por cada uma.

## React

Entre as três características que vimos, o React se propõe a ser o responsável por apenas uma, a **View.** O React não se importa em como você controla seu **State** ou suas **Actions**, ele só recebe os dados através de suas props e interage com o [*DOM*](https://pt.wikipedia.org/wiki/Modelo_de_Objeto_de_Documentos) para renderizar a **View.**

## Redux

Ao contrário do *React*, o **Redux** é alheio a sua camada de **View**, ele fica responsável apenas pelo **State** e pelas **Actions** da sua aplicação, dando a você total poder de escolha em relação a **View**.

![](https://cdn-images-1.medium.com/max/2000/1*9i0TmROGf25Lqpbc98PnJQ.jpeg)

A [**Store**](http://redux.js.org/docs/api/Store.html) do *Redux* disponibiliza três métodos para interação com o *State* através de sua [API pública](http://redux.js.org/docs/api/Store.html#store-methods):

### [*subscribe*](http://redux.js.org/docs/api/Store.html#subscribe)

Esse método permite que você adicione um listener à *Store*, que será chamado, toda vez que houver uma alteração no *State.*

### [*dispatch*](http://redux.js.org/docs/api/Store.html#dispatch)

Através desse método, você dispara as *Actions*.

### [*getState*](http://redux.js.org/docs/api/Store.html#getState)

Executando esse método, você receberá o *State* atual. Esse método é normalmente utilizado dentro dos listeners, para capturar o *State* após uma alteração ter ocorrido.

Com esses três métodos, é possível integrar o *Redux* com qualquer lib de renderização da *View*.

Analisando essas duas libs, nós conseguimos enxergar a forma como as suas se complementam e se integram uma com a outra e também conseguimos ver que mesmo se complementando, elas conseguem trabalhar de forma independente, deixando a aplicação muito menos acoplada.

### Programação Funcional

O *Redux* é baseado em conceitos de programação funcional, como imutabilidade e funções puras. É através dessas funções que nós manipulamos o *State. As funções em questão são os [reducers](http://redux.js.org/docs/basics/Reducers.html), que possuem a seguinte assinatura:*

    (currentState, action) => newState

Observando a estrutura acima, percebemos que não precisamos depender do *Redux* para escrever nossos testes unitários, uma vez que você só precisaremos chamar um *Reducer*, passando um objeto para simular o *State* atual e um outro objeto que será a *Action*, e fazer uma asserção com o valor de retorno.

O mesmo vale para o teste de [Action Creators](http://redux.js.org/docs/basics/Actions.html#action-creators), que também devem ser funções puras.

Na documentação do *Redux*, há uma [sessão dedicada a escrita de testes](http://redux.js.org/docs/recipes/WritingTests.html), vale a pena conferir.

Os conceitos de programação funcional que o *Redux* traz, ajuda muito a expandir nosso conhecimento e nossa forma de pensar como escrevemos nossa lógica, além de ser porta de entrada para muita gente nesse novo paradigma.

O Redux e o React se baseiam numa arquitetura de fluxo dados uni-direcional, o que nos dá um controle bem maior, e não cria dificuldades na hora de integrarmos uma nova lib ao projeto.

## Ainda sobre React

O *React* nos induz a desenvolver nossa aplicação orientada a componentes, fazendo com que o reaproveitamento de código seja bem efetivo e tornando nossa aplicação muito mais modular.

Além disso, podemos contar com inúmeras ferramentas que melhoram nosso workflow, como o [React Hot Loader](https://github.com/gaearon/react-hot-loader), que nos concede uma experiência de desenvolvimento muito rica, e o [React Storybook](https://github.com/kadirahq/react-storybook), que nos permite desenvolver nossos componentes de forma isolada, nos dando um controle que até então não tínhamos no Frontend.

## Por que React e Redux nos fazem ser programadores melhores?

Em um resumo geral, *React* e o *Redux* nos ensinam a pensar em nossas aplicações de forma modular nos induzindo a identificar e separar as responsabilidades de uma forma bem intuitiva. A facilidade que o *React* traz para trabalharmos com componentes, nos conduz a os separá-los de uma forma melhor, o que por si só já resulta em um reaproveitamento maior do código. A forma como cada lib se contém a sua responsabilidade, nos permite aprender a desenvolver aplicações de uma forma desacoplada. Os conceitos que o *React* e o *Redux* introduzem, traz um ganho sem tamanho para nós desenvolvedores.
