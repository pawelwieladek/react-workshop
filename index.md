title: React Workshop
author:
  name: Pawel Wieladek
  twitter: pawelwieladek
  url: http://pawelwieladek.com
style: style/style.css
output: index.html
controls: false
progress: true

--

# Szkolenie React

--

### Agenda

1. ES6
2. React
3. Flux

--

# ES6

<img src="images/js-logo.png" width="200" />

--

### Przydatne linki

[JS Bin](http://jsbin.com/?js,console) - Playground (Zmień język na ES6 / Babel)

[Learn Babel](http://babeljs.io/docs/learn-es2015/) - Dokumentacja Babel

[MDN](https://developer.mozilla.org/en-US/) - Mozilla Developer Network

[2ality](http://www.2ality.com/) - Blog o nowinkach w ES6

--

### Deklaracje zmiennych

```js
// ES5
var hoisted;
```

```js
// ES6
let blockScoped;
```

```js
// ES6
const blockScoped;
```

#### Var

```js
var a = 1;

if (a === 1) {
  var b = 2;
}

console.log(a);
// 1
console.log(b);
// 2
```

#### Let

```js
let c = 1;

if (c === 1) {
  let d = 2;
}

console.log(c);
// 1
console.log(d);
// ReferenceError: d is not defined

for (let i = 0; i < 10; i++) {
    console.log(i);
}

console.log(i);
// ReferenceError: i is not defined
```

#### Const

```js
const PI = 3.14;
PI = 3.141593;
// throws "PI" is read-only
```

--

### Template strings

```js
`This is a plain string.`
```

```js
`Multiline strings
 in ES5 are
 not legal.`
```

```js
let name = 'world';
`Hello ${name}!`;     
// Hello world!
```

--

### Enhanced Object Literals

#### Properties

```js
let name = 'Pawel';

// ES5
var me = { 
    name: name
};

// ES6
let me = { 
    name
};

console.log(me.name);
// Pawel
```

#### Methods

```js
// old
var me = {
    sayHello: function(name) {
        return 'Hi, ' + name;
    }
};

// new
let me = {
    sayHello(name) {
        return `Hi, ${name}`;
    }
};

console.log(me.sayHello('everyone'));
// Hello, everyone!
```

#### Getter

```js
let me = {
    firstName: 'Pawel',
    lastName: 'Wieladek',
    get name() {
        return `${this.firstName} ${this.lastName}`;
    }
};

console.log(me.name);     // Pawel Wieladek
```

#### Setter

```js
let me = {
    counter: 0,
    firstName: 'Pawel',
    lastName: 'Wieladek',
    get greeting() {
        return `Hello, I'm ${this.firstName} ${this.lastName}`;
    }
    set name(value) {
        this.firstName = value;
        this.counter++;
    }
};

me.name = 'Pablo';

console.log(me.greeting);
// Hello, I'm Pablo Wieladek!

console.log(me.counter);
// 1
```

#### Computed property names

```js
function createIconClassNames(name, size) {
    return {
        [`icon-${name}`]: !!name,
        [`icon-${size}`]: !!size
    });
}

let iconClassNames = createIconClassNames('square', 'large');

console.log(iconClassNames);
// { 'icon-square': true, 'icon-large': true }
```

--

### Arrow functions

#### Single argument

```js
[1, 2, 3, 4].map(x => x * 2);    

// [2, 4, 6, 8]
```

#### Multiple arguments

```js
['a', 'b', 'c', 'd'].map((value, index) => {
    index = index + 1;
    return [index, value];
}); 

// [[1, 'a'], [2, 'b'], [3, 'c'], [4, 'd']]
```

#### Multiple arguments + destructuring

```js
[[1, 'a'], [2, 'b'], [3, 'c'], [4, 'd']].map(([index, value]) => {
    return {
        [value]: index 
    };
});

// [ { a: 1 }, { b: 2 }, { c: 3 }, { d: 4 } ]
```

#### ```this``` autobinding

```js
function Exp(base, exponentsList) {
    this.base = base;
    this.exponentsList = exponentsList;
}

Exp.prototype.calculate = function() {
    return this.exponentsList.map(n => {
        return Math.pow(this.base, n);
    });
};

let exp = new Exp(2, [1, 2, 3, 4]);
console.log(exp.calculate());
// [2, 4, 8, 16]
```

--

### Destructuring

#### List matching
```js
var [a, b, c] = [1, 2, 3, 4];
console.log(a);   // 1;
console.log(b);   // 2;
console.log(c);   // 3;
```

#### List matching with empty element

```js
let [a, , c] = [1, 2, 3, 4];
console.log(a);   // 1;
console.log(c);   // 3;
```

### Destructuring

#### Object matching

```js
function createPerson() {
    return { 
        firstName: 'Pawel',
        lastName: 'Wieladek'
    };
}

let { firstName, lastName } = createPerson();

console.log(firstName);
// Pawel
console.log(lastName);
// Wieladek
```

#### Object matching with rename

```js
function createPerson() {
    return { 
        firstName: 'Pawel',
        lastName: 'Wieladek'
    };
}

let { firstName: f, lastName: l } = createPerson();

console.log(f);
// Pawel
console.log(l);
// Wieladek
```

#### Object matching in function parameter

```js
function sayHello({ first, last }) {
    // first and last are variables from now
    return `Hello, I'm ${first} ${last}!`;
}

let greeting = sayHello({ 
    first: 'Pawel', 
    last: 'Wieladek'
});

console.log(greeting);
// Hi, I'm Pawel Wieladek!
```

--

### Default arguments

```js
function multiply(a, b = 1) {
    return a * b;
}

console.log(multiply(5));
// 5
console.log(multiply(2, 3));
// 6
```

--

### Rest arguments

```js
function append(a, ...b) {
    // b is an Array
    return a + ', ' + b.join(', ');
}

console.log(append('apple', 'orange', 'plum', 'kiwi'));
// apple, orange, plum, kiwi
```

--

### Spread operator

```js
function add(x, y, z) {
  return x + y + z;
}

let args = [1, 2, 3];
console.log(add(...args));
// 6
```

--

### Klasy

- dziedziczenie oparte na prototypach
- konstruktor
- metody klasy bazowej
- metody statyczne i metody instancji

ES6 **nie udostępnia** poniższych funkcji języka:

- interfejsy
- wielodziedziczenie
- przeciążanie konstruktora

### Klasa bazowa

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    
    distance() {
        reutrn Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }
  
    toString() {
        return `x = ${this.x}, y = ${this.y}`;
    }
}

let p = new Point(3, 4);

console.log(p.distance());      
// 5

console.log(p.toString());      
// x = 3, y = 4

// toString overridden
console.log('' + p);            
// x = 3, y = 4

console.log(`${p}`);            
// x = 3, y = 4
```

### Klasa pochodna

```js
class ColorPoint extends Point {
    constructor(x, y, color) {
        super(x, y);
        this.color = color;
    }
  
    static createDefault() {
        return new ColorPoint(0, 0, 'black');
    }
  
    toString() {
        return `${super.toString()}, color = ${this.color}`;
    }
}

let p = new ColorPoint(2, 3, 'red');

console.log(p.toString());      
// x = 2, y = 3, color = red

let q = ColorPoint.createDefault();

console.log(q.toString());      
// x = 0, y = 0, color = black
```

--

### Moduły

#### Named exports

```js
// math.js

export const PI = 3.14;

export function square(x) {
    return x * x;
}
```

```js
// main.js

import { square, PI } from './math';

console.log(PI);
// 3.14
console.log(square(8));
// 64
```

```js
// main.js

import * as math from './math';

console.log(maths.PI);
// 3.14
console.log(maths.square(8));
// 64
```

--

### Moduły

#### Default exports

```js
// square.js

export default function square(x) {
    return x * x;
}
```

```js
// main.js

import square from './square';

console.log(square(8));                     // 64
```

[ECMAScript 6 modules: the final syntax](http://www.2ality.com/2014/09/es6-modules-final.html)

--

### Promise

```js
function fetch() {
    return new Promise((resolve, reject) => {
        request
            .get('https://api.github.com/zen')
            .end((error, response) => {
                if (error) reject();
                resolve(response);
            });
    });
}

function main() {
    return fetch()
        .then(response => {
           console.log(response);
        })
        .catch(error => {
           console.log(error);
        });
}
```

--

# React

<img src="images/react-logo.png" width="200" />

--

### Przydatne linki

[CodePen](http://codepen.io/pawelwieladek/pen/BjGNxX/?editors=0010)

[React Docs](https://facebook.github.io/react/docs/getting-started.html)

--

### Dlaczego React?

#### Tylko UI

Najważniejszym celem jest renderowanie. Pozostałe elementy aplikacji można dowolnie skonfigurować. 

#### Łatwy do zrozumienia przepływ danych

Wynik renderowania jest funkcją stanu i danych wejściowych. 

#### Wysoka wydajność - Virtual DOM

Dodatkowa warstwa abstrakcji nad DOM upraszcza komunikację z przeglądarką.

--

### Komponenty

- Layout podzielony na małe kawałki - **komponenty**

- Komponenty składają się z innych komponentów tworząc **drzewo komponentów**

- Kiedy problem staje się zbyt złożony komponent powinien być podzielony na mniejsze komponenty (**Single Responsibility Principle**)

- Mały komponent jest łatwy do **zrozumienia**, **utrzymania** i **testowania**

--

### Virtual DOM

- Operacje na DOM są kosztowne

- Rozwiązaniem jest wirtualne drzewo komponentów (**Virtual DOM**)

- Podczas renderowania React porównuje stare i nowe drzewo i **modyfikuje tylko niezbędne elementy w DOM**

- Znajdowanie minimalnej liczby modyfikacji pomiędzy dwoma dowolnymi drzewami ma złożoność *O(n<sup>3</sup>)*

- React używa kilku heurystyk optymalizując złożoność do *O(n)*

--

### Diff algorithm

#### Klasy komponentów

Dwa komponenty o różnych klasach na tych samych miejscach w drzewie są różne.

![Components](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/3.png "Components")

#### Listy

Komponenty są porównywane za pomocą podanych kluczy.

![List](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/2.png "List")

#### Poziom po poziomie

Porównywane są komponenty tylko z tych samych poziomów.

![Level by Level](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/1.png "Level by Level")

--

### Punkt wejścia aplikacji

```js
// <div id="main"></div>

import ReactDOM from 'react-dom';

import App from './app';

ReactDOM.render(<App />, document.querySelector('#main'));
```

--

### Przykładowy komponent

```js
// ES5
var React = require('react');

var App = React.createClass({
    render: function() {
        return (
            <div className="app">
                App
            </div>
        );
    }
});
```

```js
// ES6
import { Component } from 'react';

class App extends Component {
    render() {
        return (
            <div className="app">
                App
            </div>
        );
    }
});
```

--

### JSX

#### Input

```xml
<Nav>
    <Header className="primary">Hello world</Header>
    <Link active>Home</Link>
    <Logo />
</Nav>
```

#### Output

```js
React.createElement(Nav, null,
    React.createElement(Header, { className: "primary" }, "Hello world"),
    React.createElement(Link, { active: true }, "Home"),
    React.createElement(Logo, null),
);
```

--

### Props

 - Dane komponentu na wejściu
 
 - **Jednokierunkowy przepływ danych** z góry do dołu drzewa komponentów
  
 - Dostępne za pomocą ```this.props```

```js
var Hello = React.createClass({
    render() {
        return (
            <h1>Hello, {this.props.name}!</h1>
        );
    }
});

ReactDOM.render(<Hello name="Pawel" />, document.querySelector('#main'));
```

--

### State

 - Każdy komponent może mieć swój **stan**.
 
 - Stan powinien być modyfikowany tylko za pomocą metody ```setState```. Wywoływanie ```setState``` wyzwala przerenderowanie komponentu i wszystkich jego dzieci.

```js
// ES5

React.createClass({
    getInitialState() {
        return {
            count: 5
        };
    },
    render() {
        return (
            <h1>{this.state.count}</h1>
        );
    }
});
```

```js
// ES6

class CountComponent {
    constructor() {
        this.state = {
            count: 5
        };
    }
    
    render() {
        return (
            <h1>{this.state.count}</h1>
        );
    }
});
```

--

### Props vs State

 - O komponentach można myśleć jak o **maszynach stanu**
 
 - Jak najwięcej komponentów powinno być **bezstanowych**
 
 - Dobra praktyka: Korzeń drzewa powinien zawierać stan i implementować logikę biznesową, a jego dzieci powinny używać tylko propsów do wyrenderowania.

![Unidirectional flow](./images/unidirectional-flow.png "Unidirectional flow")

--

### Interaktywny UI

```js
React.createClass({
    getInitialState() {
        return {
            count: 5
        }
    },
    increment() {
        this.setState({
            count: this.state.count + 1
        });
    },
    render() {
        return (
            <button onClick={this.increment}>{this.state.count}</button>
        );
    }
});
```

**Autobinding:** Każda funkcja ma automatycznie dowiązany kontekst ```this```

**Dobra praktyka:** Stan powinien zawierać tylko te dane, które mogą być dynamicznie modyfikowane przez użytkownika.

--

### State: złe praktyki
 
#### Przypisywanie ```props``` do stanu
 
```js
// so bad!
getInitialState() {
    return {
        name: this.props.name  
    };
}
```

#### Nadmiarowe dane
 
```js
// so bad!
handleChange(firstName, lastName) {
    return {
        firstName: firstName,
        lastName: lastName,
        fullName: `${firstName} ${lastName}`
    };
}
```

```js
// instead modify in render
render() {
    let fullName = `${firstName} ${lastName}`;
    return (
        <div>{fullName}</div>
    );
}
```
 
#### Komponenty jako zawartość stanu
 
```js
// so bad!
showForm() {
    this.setState({
        formComponent: <Form />
    });
}
```

```js
// instead handle condition in render
showForm() {
    this.setState({
        formEnabled: true
    });
}

rednerForm() {
    if (this.state.formEnabled) {
        return <Form />;
    }
}

render() {
    let form = this.rednerForm();
    return (
        <div>
            {form}
        </div>
    );
}
```

--

### Cykl życia komponentu

1. Montowanie

2. Modyfikacja

3. Odmontowanie

[Component Specs and Lifecycle reference](https://facebook.github.io/react/docs/component-specs.html)
 
--

### ```render```

 - Jedyna wymagana metoda
 
 - Może zwracać ```null```, ```false``` lub ```undefined``` jeżeli nie chcemy nic nie renderować
 
 - Konstruuje poddrzewo komponentów, dlatego musi zawierać tylko jeden węzeł korzenia
 
--

### ```getInitialState```

```js
object getInitialState()
```

 - Wywoływana raz, przed zamontowaniem komponentu.
 
 - Zwraca początkową wartość dostępną w ```this.state``` podczas pierwszego renderowania
 
--

### ```getDefaultProps```

```js
object getDefaultProps()
```

 - Metoda statyczna dla klasy, wywoływana podczas konstruowania klasy
 
 - Zwraca domyślne wartości dostępne w ```this.props``` jeżeli nie zostaną podane podczas tworzenia instancji komponentu
 
--

### ```componentWillMount```

```js
void componentWillMount()
```

 - Wywoływana raz, tuż przed pierwszym renderowaniem komponentu
 
--

### ```componentDidMount```

```js
void componentDidMount()
```

 - Wywoływana raz, tuż po pierwszym renderowaniu komponentu
 
 - Miejsce na integrację z innymi bibliotekami np. jQuery, D3 oraz wysyłanie requestów
 
--

### ```componentWillReceiveProps```

```js
void componentWillReceiveProps(
  object nextProps
)
```

 - Wywoływana za każdym razem, tuż przed otrzymaniem nowych danych weściowych przed komponent
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### ```shouldComponentUpdate```

```js
boolean shouldComponentUpdate(
  object nextProps, object nextState
)
```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan
 
 - Nie wywoływana podczas pierwszego renderowania
 
 - Jeżeli zwróci ```false``` to nie zostaną wywołane metody ```render```, ```componentWillUpdate``` oraz ```componentDidUpdate```
 
--

### ```componentWillUpdate```

```js
void componentWillUpdate(
  object nextProps, object nextState
)
```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan, tuż przed renderowaniem
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### ```componentDidUpdate```

```js
void componentDidUpdate(
  object prevProps, object prevState
)
```

 - Wywoływana tuż po zaaplikowaniu zmian w DOM
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### ```componentWillUnmount```

```js
void componentWillUnmount()
```

 - Wywoływana tuż przed odmontowaniem komponentu
 
 - Miejsce na cleanup
 
--

### Dynamiczne komponenty

 - Każdy dynamiczny komponent powinien zawierać unikalny klucz ```key```
 
 - Jeżeli klucz komponentu w danym miejscu w drzewie ulegnie zmianie to komponent zostaje odmontowany

```js
render() {
    let results = this.props.results.map(result => {
        return <li key={result.id}>{result.text}</li>;
    });
    return (
        <ul>
            {results}
        </ul>
    );
}
```

--

### Mixins
  
```js
const AuthMixin = {
    componentDidMount() {
        if (!authorized()) {
            // redirect to login page...
        }
    },
    authorized() {
        // do some OAuth...
    }
};

const Page = React.createClass({
    mixins: [ AuthMixin ],
    render() {
        return (
            <div>You are {authorized() ? 'authorized' : 'not authorized'}</div>
        );
    }
});
```

--

### Props types

 - Walidacja danych wejściowych komponentu
 
 - Bardzo przydatne przy dużych aplikacjach

```js
React.createClass({
    propTypes: {
        name: React.PropTypes.string,
        onClick: React.PropTypes.func.isRequired
    },
    getDefaultProps() {
        return {
            onClick: () => {}
        };
    },
    render() {
        return (
            <div>
                <h1>Hello, {this.props.name}!</h1>
                <button onClick={this.props.onClick}>Click me</button>
            </div>
        );
    }
});
```

[Prop Types reference](https://facebook.github.io/react/docs/reusable-components.html)
 
--

### Referencje

- Za pomocą atrybutu ```ref``` można przechować referencję do instancji komponentu potomnego

- Referencje są dostępne w ```this.refs```

```js
getValue() {
    return ReactDOM.findDOMNode(this.refs.input).value;
},
render() {
    return (
        <input type="text" ref="input" />
    );
}
```

--

### Uncontrolled Component

- Komponent ma swój stan

- Stan komponentu może być dostępny na zewnątrz

### Controlled Component

- Komponent nie ma stanu

- Udostępnia dane za pomocą *event handlers* np. ```onChange```

- Dane muszą zostać obsłużone przez komponent nadrzędny

--

### Uncontrolled Component

<p data-height="500" data-theme-id="0" data-slug-hash="YwRXJd" data-default-tab="js" data-user="pawelwieladek" class='codepen'>See the Pen <a href='http://codepen.io/pawelwieladek/pen/YwRXJd/'>React Uncontrolled Component</a> by Pawel Wieladek (<a href='http://codepen.io/pawelwieladek'>@pawelwieladek</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

--

### Controlled Component

<p data-height="500" data-theme-id="0" data-slug-hash="bEQVww" data-default-tab="js" data-user="pawelwieladek" class='codepen'>See the Pen <a href='http://codepen.io/pawelwieladek/pen/bEQVww/'>React Controlled Component</a> by Pawel Wieladek (<a href='http://codepen.io/pawelwieladek'>@pawelwieladek</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

--

# Case study

--

![Sample app](./images/wireframe.png "Sample app")

--

![Top level components](./images/top-components.png "Top level components")

--

![Lower level components](./images/bottom-components.png "Lower level components")

--

# Flux

--

![Flux](./images/flux-diagram.png "Flux")



