---
title: "Iniciando com React - #4 Armazenando Estado e Entendendo o Lifecycle"
description: "Quando estamos desenvolvendo uma aplicação, é comum a necessidade de guardarmos estado. Juntamente com essa necessidade, é frequente precisarmos tomar uma ação..."
author: "Vinicius Dacal"
date: 2017-04-10
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "JS", "React", "Programação Funcional"]
---

Quando estamos desenvolvendo uma aplicação, é comum a necessidade de guardarmos estado. Juntamente com essa necessidade, é frequente precisarmos tomar uma ação quando um componente acabou de aparecer na tela, por exemplo, ou quando dados são atualizados. Nesse post, veremos como React pode suprir essas necessidades de forma simples.
> Nota: Este post faz parte da série “Iniciando com React”. Se você está começando com React agora, sugiro ler os outros posts [primeiro.
< Anterior](https://blog.coderockr.com/iniciando-com-react-3-criando-componentes-97f7023ca5ab)

## Class Components

*Funcional components*, como os que vimos anteriormente, não possuem as funcionalidades que listamos acima. Para podermos utilizá-las, precisamos criar componentes baseados em classes, ou ***Class Components*** como são mais conhecidos.

Para criarmos tais componentes, conforme abaixo, precisamos utilizar o [class](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes), que foi introduzido no [ES6](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes):

<iframe src="https://medium.com/media/72eac84e4775edb89b971076e509eba8" frameborder=0></iframe>

No exemplo acima, estamos definindo um componente chamado **Welcome,** que extende de **React.Component. **Esse componente retorna apenas uma **div** com o texto** Welcome **e o valor que for passado na **prop** name**. **Como podemos observar, em** Class Components **as** props **são acessadas através do** this.props.**

## Estado

O estado pode ser qualquer conjunto de informações que serão utilizadas em algum momento pela interface. Por exemplo: As informações do usuário, uma lista de itens, o resultado de um request ou até informações específicas de UI, como veremos abaixo:

<iframe src="https://medium.com/media/6ed3f1a01d6e0cca106cdeecbf6edcc7" frameborder=0></iframe>

O exemplo acima, traz várias coisas novas, mas não se preocupe, vamos explorá-lo em detalhes.

### Método constructor

No **constructor**, nós recebemos as props como primeiro parâmetro, chamamos **super(props)** para executar o construtor da classe pai, que no nosso caso é a class **Component.** Também estamos inicializando o estado do componente, através de:

    constructor(props) {**
      **super(props);
      **this.state = {
        activeIndex: props.initialIndex,
      };**
    }

Especificamos que o valor inicial de **activeIndex**, será a prop **initialIndex. **Uma vez que o estado foi inicializado, a nossa propriedade **activeIndex** poderá ser acessada em toda a classe através de **this.state.activeIndex.** Essa forma direta de atribuir um valor ao estado, só poderá ser utilizada dentro do construtor da classe.

### Método onSelectButton

Abaixo do método constructor, estamos definindo outro método, que será utilizado como [*callback](https://blog.coderockr.com/iniciando-com-react-3-criando-componentes-97f7023ca5ab#8aea)*, o **onSelectButton. **Ele será executado cada vez que houver um click em um dos nossos botões.

Nesse método, recebemos o parâmetro **index, **e atualizamos o estado do componente, através de:

    onSelectButton(index) {
      **this.setState({
        activeIndex: index,
      });**
    }

Você deve estar se perguntando: “Por que não fazer uma atribuição direta, utilizando: this.state.**activeIndex** = **index**;”? A reposta é simples: É dessa forma que o React tomará conhecimento que o estado do componente mudou, e ele tomará as medidas necessárias para atualizar a interface se for necessário.

O método **setState**, por sua vez, espera como primeiro parâmetro um objeto. Ele mesclará o valor desse objeto ao estado atual do componente. Sendo assim, não adianta executarmos this.setState(**{}**) achando que isso removerá todo o estado do componente.

A regra geral do **setState** é bem simples: Dentro do método **constructor**, inicializamos utilizando this.state **=** {...}, em qualquer outro lugar, sempre utilizamos this.setState({...}) a fim de atualizar o estado.

### Método isActiveIndex

O método **isActiveIndex**, espera como primeiro parâmetro o index e retorna **true** se o index fornecido for o ativo naquele momento.

Você deve ter notado que estamos acessando o valor de **activeIndex** diretamente, utilizando this.state.**activeIndex**. Nesse caso não precisamos utilizar nenhum método especial, pois estamos apenas **lendo** o valor do estado atual.

### Método render

No método **render**, estamos retornando o [***JSX](https://blog.coderockr.com/iniciando-com-react-2-criando-a-estrutura-do-projeto-2c3b0f8e9f9#a705)*** do nosso componente. No **className** dos nossos botões, estamos utilizando um ternário com o resultado da função **isActiveIndex**, para incluirmos a classe **btn-active **no botão que estiver selecionado. Na prop **onClick** dos nossos botões, estamos passando um *callback* para atualizar o estado. Sendo assim, toda vez que houver um click no botão, será executada a função **onSelectButton, **com o respectivo index.

## Component Lifecycle

*Class components*, possuem o que chamamos de *lifecycle methods*, que são métodos que serão executados em determinamos momentos da vida de um componente. Vamos separar esses métodos em três diferentes momentos: criação, atualização e remoção.

### Criação

No momento da criação de um componente, quatro métodos são executados:

[**constructor](https://facebook.github.io/react/docs/react-component.html#constructor) - **Esse é o método construtor do nosso componente, executado logo quando o componente é instanciado. Normalmente, esse método é utilizado para inicializarmos valores dentro e também quando precisamos fazer [bind dos métodos](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md#es6-classes) da nossa classe. Observe o exemplo abaixo:

    **constructor(props) {
      **super(props);
      this.state = {
        collection: [
          { name: 'Default Option'},
          ...props.collection
        ],
      };
      this.onClickButton = this.onClickButton.bind(this);
    **}**
> Estamos utilizando o [spread operator](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Spread_operator) no exemplo acima, funcionalidade que foi introduzida no ES6. Caso você ainda não esteja familiarizado com o mesmo, confira [esse link do MDN](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Spread_operator).

No exemplo acima, estamos inicializando nosso **state** com base em uma collection que recebemos, porém, adicionamos como primeiro item dessa collection, um objeto para ser o nosso item default. Também fazemos um **bind** do método **onClickButton**, para que seja possível utilizar o **this** dentro dele, independente do contexto que ele estiver sendo executado.

[**componentWillMount](https://facebook.github.io/react/docs/react-component.html#componentwillmount) - **Esse método é executado imediatamente antes do componente ser montado, e antes do método **render. **Nesse método, é possível alterar o state através do **this.setState**. Porém, é preferível fazer o mesmo no **constructor**, já que os dois possuem funcionalidade similar.

[**render](https://facebook.github.io/react/docs/react-component.html#render)** - No ciclo de montagem do componente, esse método é executado logo após **componentWillMount**, e o mesmo deve retornar o JSX do componente. Esse é o único método obrigatório. É importante manter o método **render** como uma função pura, uma vez que dados os mesmos *state* e *props*, ele retorne sempre o mesmo resultado. Não faça alterações ao estado de dentro desse método, utilize os outros métodos do *lifecycle* para o fazê-lo.

    **render()** **{**
      return <div>Hello there</div>
    **}**

[**componentDidMount](https://facebook.github.io/react/docs/react-component.html#componentwillmount) - **Esse método é chamado imediatamente após a montagem do componente. Em casos que precisamos fazer alguma operação que precise de elementos do DOM, é aqui o lugar certo. Aqui também é um bom lugar para inicializarmos requests quando necessário.

    **componentDidMount() {**
      request('some/endpoint').then((response) => {
        console.log('Request have finished');
      });
    **}**

### Atualização

Todos os métodos acima são executados no momento de montagem de um componente. Além do momento de montagem, temos o momento de atualização, que pode ser por exemplo, quando as *props* ou o *state* do componente são atualizados. No momento de atualização, cinco métodos são executados:
> **Nota**: Com excessão do método render, nenhum dos métodos abaixo é executado no momento de montagem do componente.

[**componentWillReceiveProps**(nextProps)](https://facebook.github.io/react/docs/react-component.html#componentwillreceiveprops)** - **Esse método é o primeiro método executado no ciclo de atualização, sempre com as novas props do componente. Sendo assim, se o **state** do componente depende do valor das **props**, é aqui que você deve atualizá-lo. Observe abaixo o exemplo:

    **componentWillReceiveProps(nextProps){
    **  this.setState({
        collection: [
          { name: 'Default Option'},
          ...nextProps.collection
        ],
      };
    **}**

No exemplo acima, estamos atualizando nosso **state** **collection**, com base na nova **collection** que recebemos através das **props**.

[**shouldComponentUpdate**(nextProps, nextState)](https://facebook.github.io/react/docs/react-component.html#shouldcomponentupdate) **- **Esse método é chamado antes de o componente se atualizar. Ele recebe como parâmetros, as novas **props** e o novo **state** do componente, e deve retornar um boolean, indicando se o componente deve ou não ser atualizado. Quando retornado **false**, o React interrompe o ciclo de atualização do componente, para economizar processamento. Algumas abordagens para resolver problemas de performance, são focadas nesse método. Observe o exemplo abaixo:

    **shouldComponentUpdate(nextProps, nextState) {**
      return !equals(nextProps, this.props) ||
        !equals(nextState, this.state)
    **}**

No exemplo acima, estamos comparando se o próximo state e props são iguais aos que já temos, se eles forem iguais nós retornamos false e assim é interrompido o ciclo de atualização.
> **Dica:** Há uma classe base de componentes que já implementa essa comparação para otimizar a performance, a classe [***React.PureComponent](https://facebook.github.io/react/docs/react-api.html#react.purecomponent)**. *Basta utilizá-la da mesma maneira que já fazemos com React.Component:
> class **MyComponent** extends **React.PureComponent **{
> Porém, para ela funcionar da maneira esperada, você precisa estar aplicando **imutabilidade** em seus dados.

O comportamento default do componente é sempre executar todo o ciclo quando houve alguma mudança na árvore de componentes, e para a maioria dos casos, você pode confiar nesse comportamento.
> **Nota:** Nenhum dos métodos abaixo será executado se o retorno do método **shouldComponentUpdate **for **false.**

[**componentWillUpdate**(nextProps, nextState)](https://facebook.github.io/react/docs/react-component.html#componentwillupdate) **-** Esse método é executado logo antes do componente ser atualizado e recebe como parâmetros, as próximas **props** e o próximo **state** do componente. **Você não pode chamar this.setState dentro desse método**. Se você precisa atualizar o state baseado em uma props, utilize o método **componentWillReceiveProps, **que já vimos mais acima.

[**render()](https://facebook.github.io/react/docs/react-component.html#render) - **Esse método é chamado tanto no momento da montagem, como já vimos acima, como em cada ciclo de atualização do componente, se o mesmo não for interrompido. Nos ciclos de atualização, esse método é executado após o método **componentWillUpdate.**

[**componentDidUpdate**(prevProps, prevState)](https://facebook.github.io/react/docs/react-component.html#componentdidupdate) . Esse método é um substituto ao método **componentDidMount**, no ciclos de atualização. Porém, esse método recebe como parâmetros, as *props* e o *state* anteriores ao último update. Aqui é um ótimo lugar para você aplicar atualizações no DOM se necessário. Também é possível inicializar requisições dentro desse método, contanto que você coloque uma condição qualquer para as mesmas, como no exemplo abaixo:

    **componentDidUpdate(prevProps, prevState) {**
      if(prevState.activeIndex !== this.state.activeIndex) {
        ...perform action
      }
    **}**

No exemplo acima, verificamos se realmente foi atualizado o **activeIndex** antes de efetuarmos uma ação.
> Alguns métodos recebem as próximas **props** e o próximo **state** e outro método recebe o **state** e **props** anteriores. Caso você precise acessar o **state** e **props** **atual** dos componentes, basta utilizar **this.state** ou **this.props**, como já vimos anteriormente.

### Remoção

No ciclo de vida de um componente, há também o ciclo de remoção, quando o mesmo é removido do DOM e não pertencerá mais a árvore de componentes.

[**componentWillUnmount](https://facebook.github.io/react/docs/react-component.html#componentwillunmount) - **Esse método é executado imediatamente antes do componente ser destruído. Esse é o lugar perfeito para você limpar timers, cancelar requests ou remover qualquer elemento do DOM que foi criado manualmente dento do método **componentDidMount**.

    **componentWillUnmount() {**
      **clearTimeout**(currentTimerId);
    **}**

Na maioria dos componentes, você utilizará somente os métodos **constructor** e **render**, mas é importante conhecer todos eles para utilizá-los quando necessário.

Esses métodos permitem a criação de componentes complexos de uma maneira clara e simples, assim fica fácil saber onde colocar cada parte da sua lógica.

No próximo post, veremos como criar nossos componentes em um ambiente controlado, o **storybook**. Uma ferramenta que ajuda muito a melhorar nossa produtividade no dia dia. **Siga-nos e não perca!**

Gostou do post e achou útil? Dê um **like **❤️ abaixo para ajudar na divulgação e para que mais pessoas tenham acesso :)
