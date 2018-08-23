[link do curso](https://egghead.io/lessons/react-create-html-elements-with-react-s-createelement-api)

# EggHead the Begginer's Guide to React


## Aula 00: Introdução

  Estratégia do Curso: Começar com um html simples e ir divagarzin adicionando conceitos e coisas do React. Como o ```Reat.createElement``` funciona; como o JSX é só uma abstração em cima do React e como o babel funciona para fazer o JSX funcionar como se fosse parte do próprio JS.

  A ideia é mostrar que React é nada além de javascript, apenas objetos e funções. 

## Aula 01: Create HTML elements with React's createElement API

```html
  <div id="root"></div>
  <script type = "text/javascript">
    const rootElement = document.getElementById('root')
    const element = document.createElement('div')
    element.textContent = 'Hello World'
    element.className = 'container'
    rootElement.appendChild(element)
  </script>
```

com react é bem similar, vamos ter um createElement e vamos renderizar ele (não com o appendChild e sim com um método do react)

Primeiramente, precisamos colocar o react:

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

O código acima em React seria dessa forma:

``` javascript
  const rootElement = document.getElementById('root')
  const element = React.createElement('div', {className: 'container'}, 'Hello World')
  ReactDOM.renbder(element, rootElement)
```

O ```createElement``` retora um objeto JS que tem alguns atributos. Um dos mais importantes é o __props__ que é um merge dos parâmetros que passamos no ```createElement``` (sem contar o primeiro, lógico). Podemos passar quantos parâmetros quisermos dentro do createElement que estarão dentro do ```children``` deste ```props``` como um array

``` javascript
  const element = React.createElement('div', {className: 'container'}, 'Hello World', 'how are you?')
```


![](/img/multiParams.png "multi params on createElement")


outras forams de fazer a mesma coisa seria dentro do segundo parâmetro:


``` javascript
  const element = React.createElement('div', {className: 'container',
  children: ['Hello World', 'how are you?']})
```

No fim o ```React.createElement``` é simples: primeiro o elemento que quer criar, depois as propriedades que você quer que aquele objeto tenha e por fim filhos que aquele objeto deve ter.


## Aula 02: Replace React createElement Function Call with JSX


Criar toda nossa aplicação utilizando o ```React.createElement``` é possível, porém, não é muito ergonomico e de fácil entendimendo. Tendo isso em mente, o time do React criou o __JSX__ (javascript + XML). com o intuito de criar nossa UI de uma maneira que é um pouco mais familiar para nós.

Para o JSX funcionar precisamos transpilar nosso __JSX__ para o ```React.createElement```. Para isso vamos utilizar babel:

```html
  <script src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
  <!-- <script type = "text/javascript"> -->
  <script type = "text/babel">
```
> Lembre-se de mudar o o text/javascript para text/babel

Temos algumas diferenças. Em HTML usamos _class_ e no _JSX_ deve ser __className__.

```javascript
  //const element = React.createElement('div', {className: 'container'}, 'Hello World')
  const element = <div className = 'container'> Hello World </div>
```

Podemos também colocar variáveis, utilizando a interpolação (através de __\{\}__ podendo utilizar qualquer código javascript que quisermos desde que ele se resolva a uma expressão)

``` javascript
  const content = 'Hello World'
  const element = <div className = 'container'> {content} </div>
```

Podemos até colocar funções se quisermos


``` javascript
  const content = 'Hello World'
  const element = <div className = 'container'> {(() => content)} </div>
```
O mesmo vale para as propriedades


``` javascript
  const content = 'Hello World'
  const myClassName = 'container'
  const element = <div className = {myClassName}> {content} </div>
```

Isso nos da poder e flexibilidade para fazer qualquer tipo de interpolação que quisermos, como por exemplo:

``` javascript
  const content = 'Hello World'
  const myClassName = 'container'
  const element = <div className = {myClassName + '__hi-there'}> {content} </div>
```

> Isso ajuda bastante no BEM, porém, como o react tem o styled components ele supre a necessidade de fazer isso, já que o objetivo do BEM é isolar o escopo das coisas e o styled components já faz isso com os ids que ele cria

Outra coisa bem comum de fazer com _JSX_ é passar os props para o componente criado com _JSX_:


``` javascript 
  const props = {
    className: 'container',
    children: 'Hello World'
  }

  const element = <div {...props} />
```

Outra coisa legal é imaginar que este objeto com as propriedades está vindo para nós de algum outro lugar e queremos sobrescrevê-lo com algumas outras coisas (se não mandarem um className quero que tenha minha className por exemplo, ou mesmo que mandem quero que tenha uma minha)


``` javascript 
  const props = {
    className: 'container',
    children: 'Hello World'
  }

  const element = <div {...props} className = 'batata' children = 'Holla Mundo'/>
```

Desta forma estamos sobrescrevendo tudo que estiver no props (meio que o conceito de definir a variável duas vezes e ela ficar com o último valor). A propriedade __children__ também é alterada (podemos sobreescrever children ao fazer ```<div ...>{o_que_eu_quiser}<div/>``` também)

O jeito mais comum de trabalhar com JSX é este de colocar a tag HTML como se fosse o HTML mesmo, colocar as props com o _spread operator_ (```{...props}```) e sobrescrever qualquer coisa que queremos.

## Aula 03: Create a Simple Reusable React Component

Podemos utilizar a interpolação para não nos repetirmos

``` javascript
  const helloWorld = <div>Hello World</div>

  const element = (
    <div className="container">
      {helloWorld}
      {helloWorld}
    </div>
  )
```

Como é _javascript_ podemos utilizar uma função para o mesmo:

``` javascript
  const message = (props) => <div>{props.msg}</div>

  const element = (
    <div className="container">
      {message({msg: 'Hello World'})}
      {message({msg: 'Goodbye World'})}
    </div>
  )
```

Infelizmente, funções não "combinam" tão bem quanto o JSX então vamos tentar uma forma diferente. Podemos fazer com ``React.createElement``` e funcionará:

```javascript
  let message = (props) => <div>{props.msg}</div>
  let element = (
    <div className="container">
      {React.createElement(message, {msg: 'Hello World'})}
      {React.createElement(message, {msg: 'Goodbye World'})}
    </div>
  )
```

Porém, queremos agora utilizar como o _JSX_, geralmente criamos apenas uma tag com o que queremos: 

```javascript
  element = (
    <div className="container">
      <message />
      {React.createElement(message, {msg: 'Hello World'})}
      {React.createElement(message, {msg: 'Goodbye World'})}
    </div>
  )
```

Porém, ao fazer isso temos um problema. Como temos uma variável/função com o mesmo nome da tag que criamos o babel vai tentar compilar para um ```React.createElment``` e receberá as props como null e tentará criar um ```tag``` html ```"message"```. Para este caso em específico, isso é exatamente o que queremos, porém, em maioria dos casos podemos ter uma outra variável e acabar referenciando ela no momento da transpilação

![](/img/babel-message.png "Babel online compiler")


Para evitar este tipo problema de referenciar alguma variável que já temos em algum lugar (sem intenção de referenciá-la) tem-se uma convenção de utilizar a primeira letra em maíusculo:

![](/img/babel-message-correto.png "Babel online compiler correct")


Agora temos um erro no console por não ter o _Message_:

![](/img/erro-maiusculo.png "Babel online compiler correct")

Podemos então criar nosso componente:

```javascript

  const Message = (props) => <div>{props.msg}</div> // mesmma coisa do message anterior
  element = (
    <div className="container">
      <Message msg ='Hello World' />
      <Message msg ='Goodbye World' />
    </div>
  )

```

Agora podemos reutilizar o nosso componente em qualquer outro lugar e compor qualquer tipo de componente e tal.


É uma boa prática utilizar o children nestes casos onde queremos renderizar alguma coisa:

```javascript

  const Message = (props) => <div>{props.children}</div> // mesmma coisa do message anterior
  element = (
    <div className="container">
      <Message> 'Hello World' <Message/>
      <Message> 'Goodbye World' <Message/>
    </div>
  )

```
