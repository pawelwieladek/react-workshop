title: React ES6 Workshop
author:
  name: Pawel Wieladek
  twitter: pawelwieladek
  url: http://pawelwieladek.com
style: style.css
output: public/build/react.html
controls: false
progress: true

--

![React](../images/react-logo.png "React")

# React

--

### Plac zabaw

[Fork this pen](http://codepen.io/pawelwieladek/pen/BjGNxX/?editors=0010)

### Dokumentacja

[Facebook](https://facebook.github.io/react/docs/getting-started.html)

--

### Dlaczego React?

- jeden cel: tworzenie interfejsu użytkownika

- łatwy do zrozumienia przepływ danych w aplikacji

- wysoka wydajność

--

### Komponenty

- Layout podzielony na małe kawałki - **komponenty**

- Komponenty składają się z innych komponentów tworząc **drzewo komponentów**

- Kiedy problem staje się zbyt złożony komponent powinien być podzielony na mniejsze komponenty: **Single Responsibility Principle**

- Mały komponent jest łatwy do **zrozumienia**, **utrzymania** i **testowania**

--

### Virtual DOM

- Operacje na DOM są kosztowne

- Rozwiązaniem jest wirtualne drzewo komponentów **(Virtual DOM)**

- Podczas renderowania React porównuje stare i nowe drzewo i **modyfikuje tylko niezbędne elementy w DOM**

--

### Virtual DOM

- Znajdowanie minimalnej liczby modyfikacji pomiędzy dwoma dowolnymi drzewami ma złożoność O(n<sup>3</sup>)

- React używa kilku heurystyk optymalizując złożoność do O(n)

--

### Diff algorithm

#### Klasy komponentów

Dwa komponenty o różnych klasach na pewno są różne.

![Components](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/3.png "Components")

--

### Diff algorithm

#### Poziom po poziomie

Porównywane są komponenty tylko z tych samych poziomów.

![Level by Level](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/1.png "Level by Level")

--

### Diff algorithm

#### Listy

Komponenty są porównywane za pomocą podanych kluczy.

![List](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/2.png "List")

--

### Punkt wejścia aplikacji

index.html

```html
<div id="main"></div>
```

main.js

```js
import React from 'react';
import ReactDOM from 'react-dom';

import App from './app';

ReactDOM.render(<App />, document.querySelector('#main'));
```

--

### Przykładowy komponent

```js
const App = React.createClass({
    render() {
        return (
            <div className="app">
                App
            </div>
        )
    }
});
```

--

### JSX

###### Input

```xml
<Nav>
    <Header className="primary">Hello world</Header>
    <Link active>Home</Link>
    <Logo />
</Nav>
```

###### Output

```js
React.createElement(Nav, null,
    React.createElement(Header, { className: "primary" }, "Hello world"),
    React.createElement(Link, { active: true }, "Home"),
    React.createElement(Logo, null),
);
```

--

### Props

 - **Jednokierunkowy przepływ danych** z góry do dołu drzewa komponentów

 - Dane komponentu na wejściu
  
 - Dostępne za pomocą ```this.props```

--

### Props

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

 - Każdy komponent może mieć swój stan **(state)**
 
 - Stan powinien być modyfikowany tylko za pomocą metody ```setState```
 
 - Wywoływanie ```setState``` wyzwala przerenderowanie komponentu i wszystkich jego dzieci
 
 - Do ustawienia stanu początkowego służy metoda ```getInitialState```

--

### State

```js
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

--

### State

 - O komponentach można myśleć jak o **maszynach stanu**
 
 - Jak najwięcej komponentów powinno być **bezstanowych**
 
 - Dobra praktyka: Korzeń drzewa powinien zawierać stan i implementować logikę biznesową, a jego dzieci powinny używać tylko propsów do wyrenderowania.

--

### Props vs State

![Unidirectional flow](../images/unidirectional-flow.png "Unidirectional flow")

--

### State: interaktywny UI

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
 
Zduplikowane dane wejściowe ```props```
 
```js
// so bad!
getInitialState() {
    return {
        name: this.props.name  
    };
}
```

--

### State: złe praktyki

Nadmiarowe dane
 
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

--

### State: złe praktyki
 
Komponenty jako zawartość stanu
 
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

### Cykl życia komponentu

#### ```render```

 - Jedyna wymagana metoda
 
 - Może zwracać ```null```, ```false``` lub ```undefined``` jeżeli nie chcemy nic nie renderować
 
 - Konstruuje poddrzewo komponentów, dlatego musi zawierać tylko jeden węzeł korzenia

--

### Cykl życia komponentu

#### ```getInitialState```

```js
object getInitialState()
```

 - Wywoływana raz, przed zamontowaniem komponentu.
 
 - Zwraca początkową wartość dostępną w ```this.state``` podczas pierwszego renderowania

--

### Cykl życia komponentu

#### ```getDefaultProps```

```js
object getDefaultProps()
```

 - Metoda statyczna dla klasy, wywoływana podczas konstruowania klasy
 
 - Zwraca domyślne wartości dostępne w ```this.props``` jeżeli nie zostaną podane podczas tworzenia instancji komponentu

--

### Cykl życia komponentu

#### ```componentWillMount```

```js
void componentWillMount()
```

 - Wywoływana raz, tuż przed pierwszym renderowaniem komponentu
 
--

### Cykl życia komponentu

#### ```componentDidMount```

```js
void componentDidMount()
```

 - Wywoływana raz, tuż po pierwszym renderowaniu komponentu
 
 - Miejsce na integrację z innymi bibliotekami np. jQuery, D3 oraz wysyłanie requestów
 
--

### Cykl życia komponentu

#### ```componentWillReceiveProps```

```js
void componentWillReceiveProps(
  object nextProps
)
```

 - Wywoływana za każdym razem, tuż przed otrzymaniem nowych danych weściowych przed komponent
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### Cykl życia komponentu

#### ```shouldComponentUpdate```

```js
boolean shouldComponentUpdate(
  object nextProps, object nextState
)
```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan
 
 - Nie wywoływana podczas pierwszego renderowania
 
 - Jeżeli zwróci ```false``` to nie zostaną wywołane metody ```render```, ```componentWillUpdate``` oraz ```componentDidUpdate```
 
--

### Cykl życia komponentu

#### ```componentWillUpdate```

```js
void componentWillUpdate(
  object nextProps, object nextState
)
```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan, tuż przed renderowaniem
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### Cykl życia komponentu

#### ```componentDidUpdate```

```js
void componentDidUpdate(
  object prevProps, object prevState
)
```

 - Wywoływana tuż po zaaplikowaniu zmian w DOM
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### Cykl życia komponentu

#### ```componentWillUnmount```

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
        // do some oAuth...
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

### Case study

![Sample app](../images/wireframe.png "Sample app")

--

### Case study

![Top level components](../images/top-components.png "Top level components")

--

### Case study

![Lower level components](../images/bottom-components.png "Lower level components")

--

### Flux

![Flux](../images/flux-diagram.png "Flux")



