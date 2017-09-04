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

<img src="images/react-logo.png" width="200" />

--

### Agenda

1. ES6
2. React

[pawelwieladek.github.io/react-workshop/](http://pawelwieladek.github.io/react-workshop/)

--

# ES6

<img src="images/js-logo.png" width="200" />

--

### Zmienne

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

--

### String

```js
'This ia a plain string.'
`This is a plain string as well.`
```

```js
// ES5
'Multiline strings' +
'in ES5 are' +
'not legal.'

// ES6
`Multiline strings
 in ES6 are
 legal from now.`
```

#### Interpolacja

```js
var name = 'world';

// ES5
'Hello ' + name + '!';

// ES6
`Hello ${name}!`;
```

Więcej: [Template literals @ MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

--

### Obiekty

#### Pola

```js
let name = 'Pawel';

// ES5
var you = {
    name: name
};

// ES6
let me = {
    name
};

console.log(me.name);
// Pawel
```

#### Metody

```js
// ES5
var you = {
    sayHello: function(name) {
        return 'Hi, ' + name;
    }
};

// ES6
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

console.log(me.name);
// Pawel Wieladek
```

#### Setter

```js
let me = {
    counter: 0,
    firstName: 'Pawel',
    lastName: 'Wieladek',
    get greeting() {
        return `Hello, I'm ${this.firstName} ${this.lastName}`;
    },
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

#### *Computed property names*

```js
// ES5
function createIconClassNames(name, size) {
    var classNames = {};
    classNames['icon-' + name] = !!name;
    classNames['icon-' + size] = !!size;
    return classNames;
}

// ES6
function createIconClassNames(name, size) {
    return {
        [`icon-${name}`]: !!name,
        [`icon-${size}`]: !!size
    };
}

let iconClassNames = createIconClassNames('square', 'large');

console.log(iconClassNames);
// { 'icon-square': true, 'icon-large': true }
```

--

### *Destructuring*

#### *List matching*
```js
var [a, b, c] = [1, 2, 3, 4];

console.log(a);
// 1;
console.log(b);
// 2;
console.log(c);
// 3;
```

```js
let [a, , c] = [1, 2, 3, 4];

console.log(a);
// 1;
console.log(c);
// 3;
```

#### *Object matching*

```js
function createPerson() {
    return {
        firstName: 'Pawel',
        lastName: 'Wieladek'
    };
}

// later on...

let { firstName, lastName } = createPerson();

console.log(firstName);
// Pawel
console.log(lastName);
// Wieladek
```

```js
let { firstName: f, lastName: l } = createPerson();

console.log(f);
// Pawel
console.log(l);
// Wieladek
```

```js
let response = {
    status: 400,
    errors: [
        {
            text: 'Bad Request',
            stack: '...'
        }
    ]
};

let { status, errors: [ { text: message } ] } = response;

console.log(status);
// 400
console.log({ message });
// { message: 'Bad request' }
```

#### *Destructuring* jako argument funkcji

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

### Argumenty domyślne i zmienna liczba argumentów

#### Argument domyślny

```js
function multiply(a, b = 1) {
    return a * b;
}

console.log(multiply(5));
// 5
console.log(multiply(2, 3));
// 6
```

#### Zmienna liczba argumentów

```js
function append(a, ...b) {
    // b is an Array
    return a + ', ' + b.join(', ');
}

console.log(append('apple', 'orange', 'plum', 'kiwi'));
// apple, orange, plum, kiwi
```

#### Operator *spread*

```js
function add(x, y, z) {
  return x + y + z;
}

let a = [1, 2, 3];
console.log(add(...a));
// 6

let b = [
    ...a,
    4,
    5
];
console.log(b);
// 1, 2, 3, 4, 5
```

--

### Wyrażenia lambda

#### Jeden argument

```js
[1, 2, 3, 4].map(x => x * 2);    

// [2, 4, 6, 8]
```

#### Wiele argumentów

```js
['a', 'b', 'c', 'd'].map((value, index) => {
    index = index + 1;
    return [index, value];
});

// [[1, 'a'], [2, 'b'], [3, 'c'], [4, 'd']]
```

#### Wiele argumentów + *destructuring*

```js
[[1, 'a'], [2, 'b'], [3, 'c'], [4, 'd']].map(([index, value]) => {
    return {
        [value]: index
    };
});

// [ { a: 1 }, { b: 2 }, { c: 3 }, { d: 4 } ]
```

#### ```this``` *autobinding*

```js
// ES5
function AsyncCounter(initialValue) {
    this.value = initialValue;
}

AsyncCounter.prototype.increment = function() {
    setTimeout(function() {
        this.value++;
    }.bind(this), 100);
};

// ES6
class AsyncCounter {
    constructor(initialValue) {
        this.value = initialValue;
    }

    increment() {
        setTimeout(() => {
            // `this` is bound to the value of current context
            this.value++;
        }, 100);
    }
}
```

--

### Klasy

- dziedziczenie oparte na prototypach
- konstruktor
- metody klasy bazowej
- metody statyczne i metody instancji

### Klasa bazowa

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString() {
        return `x = ${this.x}, y = ${this.y}`;
    }
}

let p = new Point(3, 4);

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

ES6 **nie udostępnia** poniższych funkcjonalności:

- interfejsy
- wielodziedziczenie
- przeciążanie konstruktora

--

### Moduły

#### Eksport domyślny

```js
// square.js

export default function square(x) {
    return x * x;
}
```

```js
// main.js

import sq from './square';

console.log(sq(8));
// 64
```

#### Eksport nazwany

```js
// lib.js

export const PI = 3.14;

export function square(x) {
    return x * x;
}

export class Circle {
    constructor(x, y, r) {
        this.x = x;
        this.y = y;
        this.r = r;
    }
}
```

```js
// main.js

import { square, PI, Circle } from './lib';

console.log(PI);
// 3.14
console.log(square(8));
// 64
console.log(new Circle(3, 4, 5));
// { x: 3, y: 4, r: 5 }
```

```js
// main.js

import * as lib from './lib';

console.log(lib.PI);
// 3.14
console.log(lib.square(8));
// 64
```

#### Dobra praktyka
**Eksport nazwany** wymusza używanie dokładnej nazwy klasy, co pomaga w utrzymaniu spójności kodu.

[ECMAScript 6 modules: the final syntax @ 2ality](http://www.2ality.com/2014/09/es6-modules-final.html)

--

### Promise

Obsługa wywołań asynchronicznych.

```js
function fetch() {
    return new Promise((resolve, reject) => {
        request
            .get('https://api.github.com/zen')
            .end((error, response) => {
                if (error) {
                    reject();
                    return;
                }
                resolve(response);
            });
    });
}

fetch()
    .then(response => console.log(response))
    .catch(error => console.log(error));
```

--

# React

<img src="images/react-logo.png" width="200" />

--

### Komponenty

Każdy komponent, z którego składa się layout aplikacji to niezależny klocek.
Komponenty składają się z innych komponentów tworząc **drzewo komponentów**

Kiedy problem staje się zbyt złożony komponent powinien być podzielony na mniejsze komponenty.
Mały komponent jest łatwy do **zrozumienia**, **utrzymania** i **testowania**.

<img src="images/components-layout.svg" width="800" />

--

### Virtual DOM

**Operacje na DOM są kosztowne**.

Zamiast operować na poszczególnych elementach DOM, React tworzy wirtualne drzewo komponentów (**Virtual DOM**), wykrywa zmiany i podmienia drzewo.

<img src="images/virtual-dom.svg" width="600" />

Podczas renderowania React porównuje stare i nowe drzewo i **modyfikuje tylko niezbędne elementy w DOM**.

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

Tag ```<div>``` tworzy instancję klasy ```div```.

```ReactDOM``` implementuje wszystkie tagi HTML wraz z odpowiednimi atrybutami jako pochodne klasy ```React.Component```.

#### Przykład

##### Input

```xml
<Nav>
    <Header className="primary">Hello world</Header>
    <Link active>Home</Link>
    <Logo />
</Nav>
```

##### Output

```js
React.createElement(Nav, null,
    React.createElement(Header, { className: "primary" }, "Hello world"),
    React.createElement(Link, { active: true }, "Home"),
    React.createElement(Logo, null),
);
```

#### Uwaga

Dowolny kod JavaScript może być umieszczony wewnątrz nawiasów ```{ }```.

#### Wartość atrybutu typu *string*

```xml
<div className="container"><div>
```

#### Wartość atrybutu typu *boolean*

```xml
<button disabled></button>
```

#### Dowolna wartość atrybutu

```xml
<textarea cols={6} onChange={this.handleChange}></textarea>
```

#### *Self-closing tag*

```xml
<input type="text" value="XYZ" />
```

#### Spread

```js
const props = {
    title,
    onClick
};
```

wtedy

```xml
<Component {...props} />
```

jest równoznaczne z

```xml
<Component title={title} onClick={onClick} />
```

#### Ważne
JSX nie jest częścią języka JavaScript w żadnej wersji (w tym ES6), dlatego do poprawnego działania w przeglądarce potrzebny jest kod po transpilacji.

--

### Props

*Props* to **dane wejściowe** komponentu.

```js
class Hello extends React.Component {
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

Każdy komponent może mieć swój **stan**.

Stan powinien być modyfikowany tylko za pomocą metody ```setState```.

Wywoływanie ```setState``` wyzwala przerenderowanie komponentu i wszystkich jego dzieci.

```js
// ES6
class Counter extends React.Component {
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

### Przepływ danych

Komponent to **funkcja dwóch zmiennych zmiennych**: *props* i *state* zwracająca wynik renderowania - drzewo węzłów DOM.

Przepływ danych jest **jednokierunkowy** - od góry do dołu drzewa komponentów.

#### Dobre praktyki

Jak najwięcej komponentów powinno być **bezstanowych**.

Korzeń drzewa powinien zawierać stan i implementować logikę biznesową, a jego dzieci do wyrenderowania powinny używać wyłącznie danych wejściowych.

<img src="images/top-down.svg" alt="Jednokierunkowy przepływ danych" width="800" />

--

### Zdarzenia

```js
class Counter extends React.Component {
    state = {
        count: 5
    }
    
    increment() {
        this.setState({
            count: this.state.count + 1
        });
    }
    
    render() {
        return (
            <button onClick={this.increment}>{this.state.count}</button>
        );
    }
}
```

#### Dobra praktyka

Stan powinien zawierać tylko te dane, które mogą być dynamicznie modyfikowane przez użytkownika.

#### Autobinding

Metody komponentu utworzonego poprzez ```createClass``` mają **automatycznie dowiązany kontekst ```this```**.

Tworząc komponent za pomocą dziedzieczenia po klasie ```Component``` należy pamiętać o **ręcznym dowiązaniu kontekstu do funkcji**, które nie należą do specyfikacji komponentu.

```js
constructor() {
    this.handleClick = this.handleClick.bind(this);
}
```

```js
handleClick = (event) => {
    console.log(event);
};
```

```js
<button onClick={() => this.handleClick()} />
```

Można też wykorzystać [eksperymentalny syntax ES7](http://babeljs.io/blog/2015/05/14/function-bind):

```js
<button onClick={::this.handleClick} />
```

--

### Złe praktyki

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

#### Przetrzymywanie komponentów w stanie

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

<img src="images/component-lifecycle.svg" alt="Component Lifecycle" width="600" />

[Component Specs and Lifecycle reference](https://facebook.github.io/react/docs/component-specs.html)

### ```render```

 - **Wymagana metoda** (jedyna).

 - Może zwracać ```null``` lub ```false``` jeżeli komponent ma nic nie renderować.

 - Musi zawierać dokładnie jeden węzeł korzenia zwracanego drzewa.

### Mount

### ```getInitialState```

```js
object getInitialState()
```

 - Wywoływana raz, przed zamontowaniem komponentu.

 - Zwraca początkową wartość dostępną w ```this.state``` podczas pierwszego renderowania.

### ```getDefaultProps```

```js
object getDefaultProps()
```

 - Metoda statyczna dla klasy, wywoływana podczas konstruowania klasy.

 - Zwraca domyślne wartości dostępne w ```this.props``` jeżeli nie zostaną podane podczas tworzenia instancji komponentu.

### ```componentWillMount```

```js
void componentWillMount()
```

 - Wywoływana raz, tuż przed pierwszym renderowaniem komponentu.

### ```componentDidMount```

```js
void componentDidMount()
```

 - Wywoływana raz, tuż po pierwszym renderowaniu komponentu.

#### Dobra praktyka

Miejsce na integrację z innymi bibliotekami np. jQuery, D3.

### Update

### ```componentWillReceiveProps```

```js
void componentWillReceiveProps(
  object nextProps
)
```

 - Wywoływana za każdym razem, tuż przed otrzymaniem nowych danych weściowych.

 - Nie wywoływana podczas pierwszego renderowania.

### ```componentWillUpdate```

```js
void componentWillUpdate(
  object nextProps, object nextState
)
```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan, tuż przed renderowaniem.

 - Nie wywoływana podczas pierwszego renderowania.

### ```componentDidUpdate```

```js
void componentDidUpdate(
  object prevProps, object prevState
)
```

 - Wywoływana tuż po zaaplikowaniu zmian w DOM.

 - Nie wywoływana podczas pierwszego renderowania.

### ```shouldComponentUpdate```

```js
boolean shouldComponentUpdate(
  object nextProps, object nextState
)
```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan.

 - Nie wywoływana podczas pierwszego renderowania.

 - Jeżeli zwróci ```false``` to nie zostaną wywołane metody ```render```, ```componentWillUpdate``` oraz ```componentDidUpdate```.

### Unmount

### ```componentWillUnmount```

```js
void componentWillUnmount()
```

 - Wywoływana tuż przed odmontowaniem komponentu.

#### Dobra praktyka

Miejsce na cleanup.

--

### Dynamiczne komponenty

Każdy dynamicznie tworzony komponent powinien zawierać unikalny klucz ```key```, aby React mógł w optymalny sposób porównać drzewa komponentów.

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

#### Dobra praktyka

Dobór klucza dla dynamicznych komponentów nie jest trywialny. Klucz powinien być unikalny, dlatego renderowany model powinien dostarczać swój unikalny id.

--

### *PropTypes*

Walidacja danych wejściowych komponentu.

```js
import React from 'react';
import PropTypes from 'prop-types';

class extends React.Component {
    static propTypes = {
        name: PropTypes.string.isRequired,
        onClick: PropTypes.func
    }
    
    static defaultProps = {
        return {
            onClick: () => {}
        };
    }
    
    render() {
        return (
            <div>
                <h1>Hello, {this.props.name}!</h1>
                <button onClick={this.props.onClick}>Click me</button>
            </div>
        );
    }
}
```

#### Dobra praktyka

Każdy element ```this.props``` powinien zostać zwalidowany w ```propsTypes```.

Walidacja danych wejściowych powinna być **wyłączona** na produkcji, ponieważ istotnie spowalnia renderowanie strony.

[Prop Types reference @ React Docs](https://facebook.github.io/react/docs/reusable-components.html)

[Performance Engineering with React](http://benchling.engineering/performance-engineering-with-react/)

--

### Referencje

Za pomocą atrybutu ```ref``` można przechować referencję do komponentów potomnych.

```js
getValue() {
    return ReactDOM.findDOMNode(this.refs.input).value;
},
render() {
    return (
        <form>
            <input type="text" ref="input" />
        </form>
    );
}
```

Referencje wykorzystuje się do integracji komponentu z bibliotekami, które wymagają elementów DOM np. jQuery lub d3.

```js
const list = ReactDOM.findDOMNode(this.refs.list);
d3.select(list);
```

--

### Style i klasy CSS

Komponenty DOM udostępniają dwa atrybuty do obsługi styli: ```classNames``` oraz ```style```.

Atrybut ```classNames``` przyjmuje stringa z nazwami klas, podobnie jak w HTML.

Pomocną biblioteką do obsługi klas CSS jest pakiet [classnames](https://github.com/JedWatson/classnames).

Atrybut ```style``` przyjmuje obiekt z polami odpowiednimi dla właściwości CSS.

```js
<div
    style={{
        color: 'red',
        borderWidth: 10,
        textDecoration: 'underline'
    }}>
</div>
```

--

### *Uncontrolled Component*

- Komponent stanowy

- Nie ma możliwości zmiany prezentowanej wartości z zewnątrz

### *Controlled Component*

- Komponent bezstanowy

- Wartość musi zostać obsłużona w stanie jednego z komponentów nadrzędnych

- Pełna kontrola nad prezentowaną wartością

--

### *Uncontrolled Component*

<p data-height="500" data-theme-id="0" data-slug-hash="YwRXJd" data-default-tab="js,result" data-user="pawelwieladek" data-embed-version="2" class="codepen">See the Pen <a href='http://codepen.io/pawelwieladek/pen/YwRXJd/'>React Uncontrolled Component</a> by Pawel Wieladek (<a href='http://codepen.io/pawelwieladek'>@pawelwieladek</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

--

### *Controlled Component*

<p data-height="500" data-theme-id="0" data-slug-hash="bEQVww" data-default-tab="js,result" data-user="pawelwieladek" data-embed-version="2" class="codepen">See the Pen <a href='http://codepen.io/pawelwieladek/pen/bEQVww/'>React Controlled Component</a> by Pawel Wieladek (<a href='http://codepen.io/pawelwieladek'>@pawelwieladek</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

--

### Komponent jako funkcja

Jeżeli komponent nie ma swojego wewnętrznego stanu, ani nie nadpisuje metod z cyklu życia komponentu może być zapisany jako czysta funkcja.

```js
import React, { PropTypes } from 'react';

const Todo = ({ onClick, completed, text }) => (
    <li
        onClick={onClick}
        style={{
            textDecoration: completed ? 'line-through' : 'none'
        }}>
        {text}
    </li>
);

Todo.propTypes = {
    onClick: PropTypes.func.isRequired,
    completed: PropTypes.bool.isRequired,
    text: PropTypes.string.isRequired
};

export default Todo;
```

--

### Testy jednostkowe

```js
class Counter extends Component {
    static propTypes = {
        initialValue: PropTypes.number
    };

    static defaultProps = {
        initialValue: 0
    };
    
    state = {
        value: this.props.initialValue
    };

    increment = () => {
        this.setState({
            value: this.state.value + 1
        });
    };

    reset = () => {
        this.setState({
            value: this.props.initialValue
        });
    };

    render() {
        return (
            <div>
                <h1 ref="value">{this.state.value}</h1>
                <button ref="increment" onClick={this.increment}>Increment</button>
                <button ref="reset" onClick={this.reset}>Reset</button>
            </div>
        );
    }
}
```

```js
describe('<Counter /> should', () => {
    it('render header and two buttons', () => {
        const counter = mount(<Counter />);
        expect(counter.find('h1')).to.have.length(1);
        expect(counter.find('button')).to.have.length(2);
    });

    it('render default value', () => {
        const counter = mount(<Counter />);
        expect(counter.ref('value').text()).equal('0');
    });

    it('render initial value', () => {
        const counter = mount(<Counter initialValue={3} />);
        expect(counter.find('h1').text()).equal('3');
    });

    it('increment value', () => {
        const counter = mount(<Counter />);
        counter.find('button').first().simulate('click');
        expect(counter.state().value).equal(1);
    });

    it('increment value multiple times', () => {
        const counter = mount(<Counter />);
        counter.ref('increment').simulate('click');
        counter.ref('increment').simulate('click');
        counter.ref('increment').simulate('click');
        expect(counter.ref('value').text()).equal('3');
    });

    it('reset value to default value', () => {
        const counter = mount(<Counter />);
        counter.ref('increment').simulate('click');
        counter.ref('reset').simulate('click');
        expect(counter.state().value).equal(0);
    });

    it('reset value to initial value', () => {
        const counter = mount(<Counter initialValue={3} />);
        counter.find('button').at(0).simulate('click');
        counter.find('button').at(1).simulate('click');
        expect(counter.ref('value').text()).equal('3');
    });
});
```

- [**Mocha**](https://mochajs.org/) - test runner
- [**Chai**](http://chaijs.com/) - TDD/BDD
- [**Sinon**](http://sinonjs.org/) - spy, stub, mock
- [**Enzyme**](http://airbnb.io/enzyme/) - React Component wrapper

--

### Dekoratory

```js
function log(DecoratedComponent) {
    const componentDidMount = DecoratedComponent.prototype.componentDidMount;

    DecoratedComponent.prototype.componentDidMount = function() {
        
        console.log('Decorator log');
        
        componentDidMount.call(DecoratedComponent);
    };
}

@log
class Page extends Component {
    componentDidMount() {
        console.log('Class log');
    }
}

// 'Decorator log'
// 'Class log'
```

##### Uwaga

Dekoratory nie są oficjalną częścią języka ES6, ani ES7. Prawdopodobnie wejdą w jednej z kolejnych iteracji.
Mimo to, są dostępne dzięki odpowiedniej konfiguracji Babela.

--

### Podsumowanie

#### Tylko UI

Jedyne zadanie biblioteki - wyrenderowanie danych.

#### Łatwy do zrozumienia przepływ danych

Wynik renderowania jest funkcją stanu i danych wejściowych.

#### Wysoka wydajność - Virtual DOM

Dodatkowa warstwa abstrakcji nad DOM upraszcza komunikację z przeglądarką.

--

# Warsztat

--

<img src="images/workshop.svg" width="800" />

--

### Project

Clone repository from [GitHub](https://github.com/pawelwieladek/react-starter-kit)

```
git clone https://github.com/pawelwieladek/react-starter-kit.git
```