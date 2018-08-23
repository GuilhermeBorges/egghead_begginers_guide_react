[link do curso](https://egghead.io/lessons/react-create-html-elements-with-react-s-createelement-api)

# EggHead the Begginer's Guide to React


## Aula 1: Introdução

  Estratégia do Curso: Começar com um html simples e ir divagarzin adicionando conceitos e coisas do React. Como o ```Reat.createElement``` funciona; como o JSX é só uma abstração em cima do React e como o babel funciona para fazer o JSX funcionar como se fosse parte do próprio JS.

  A ideia é mostrar que React é nada além de javascript, apenas objetos e funções. 

## Aula 2: Create HTML elements with React's createElement API

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


![](/img/multiPrams.png "multi params on createElement")


outras forams de fazer a mesma coisa seria dentro do segundo parâmetro:


``` javascript
  const element = React.createElement('div', {className: 'container',
  children: ['Hello World', 'how are you?']})
```

No fim o ```React.createElement``` é simples: primeiro o elemento que quer criar, depois as propriedades que você quer que aquele objeto tenha e por fim filhos que aquele objeto deve ter


