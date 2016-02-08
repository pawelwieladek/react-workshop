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

### Wstęp

- Przede wszystkim **koncepcja**, biblioteka jest tylko jej implementacją

- Odpowiedzialny tylko za **renderowanie**

- Wysoka **wydajność** głównym celem twórców

--

### Komponenty

- Layout podzielony na małe kawałki - **komponenty**

- Komponenty składają się z innych komponentów tworząc **drzewo komponentów**

- Kiedy problem staje się zbyt złożony komponent powinien być podzielony na mniejsze komponenty: **Single Responsibility Principle**

--

### Komponenty 

- Nie ma jednego szablonu całej strony

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

### Entry point renderowania

```html
<div id="main"></div>
```

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
var Counter = React.createClass({
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
 
 - Dobra praktyka: Korzeń drzewa powinien zawierać stan i implementować logikę biznesową a jego dzieci powinny używać tylko propsów do wyrenderowania.

--

### Props vs State

![Unidirectional flow](../images/unidirectional-flow.png "Unidirectional flow")

--

### State: interaktywny UI

```js
var Counter = React.createClass({
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

--

### State: dobra praktyka

Stan powinien zawierać tylko te dane, które mogą być dynamicznie modyfikowane przez użytkownika.

--

### State: złe praktyki
 
 - Zduplikowane dane z ```props```

 - Zmodyfikowane dane z istniejącego stanu
 
 - Komponenty jako zawartość stanu

--

### Cykl życia komponentu

1. Montowanie

2. Modyfikacja

3. Odmontowanie

--

### Cykl życia komponentu

#### ```render```

 - Jedyna wymagana metoda
 
 - Może zwracać ```null```, ```false``` lub ```undefined``` jeżeli nie chcemy nic nie renderować
 
 - Konstruuje poddrzewo komponentów, dlatego musi zawierać tylko jeden węzeł korzenia

--

### Cykl życia komponentu

#### ```getInitialState```

 - Wywoływana raz, przed zamontowaniem komponentu.
 
 - Zwraca początkową wartość dostępną w ```this.state``` podczas pierwszego renderowania

--

### Cykl życia komponentu

#### ```getDefaultProps```

 - Metoda statyczna dla klasy, wywoływana podczas konstruowania klasy
 
 - Zwraca domyslne wartości dostępne w ```this.props``` jeżeli nie zostaną podane podczas tworzenia instancji komponentu

--

### Cykl życia komponentu

#### ```componentWillMount```

 - Wywoływana raz, tuż przed pierwszym renderowaniem komponentu
 
--

### Cykl życia komponentu

#### ```componentDidMount```

 - Wywoływana raz, tuż po pierwszym renderowaniu komponentu
 
 - Miejsce na integrację z innymi bibliotekami np. jQuery, D3 oraz wysyłanie requestów
 
--

### Cykl życia komponentu

#### ```componentWillReceiveProps```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### Cykl życia komponentu

#### ```shouldComponentUpdate```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan
 
 - Nie wywoływana podczas pierwszego renderowania
 
 - Jeżeli zwróci ```false``` to nie zostaną wywołane metody ```render```, ```componentWillUpdate``` oraz ```componentDidUpdate```
 
--

### Cykl życia komponentu

#### ```componentWillUpdate```

 - Wywoływana za każdym razem, gdy komponent otrzymuje nowe dane wejściowe lub zmienia swój stan, tuż przed renderowaniem
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### Cykl życia komponentu

#### ```componentWillUpdate```

 - Wywoływana tuż po zaaplikowaniu zmian w DOM
 
 - Nie wywoływana podczas pierwszego renderowania
 
--

### Cykl życia komponentu

#### ```componentWillUnmount```

 - Wywoływana tuż po odmontowaniu komponentu
 
 - Miejsce na cleanup

--

### Case study

![Sample app](../images/wireframe.png "Sample app")

--

### Case study

![Top level components](../images/top-components.png "Top level components")

--

### Case study

![Lower level components](../images/bottom-components.png "Lower level components")

