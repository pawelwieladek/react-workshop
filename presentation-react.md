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

### Introduction

- React is firstly a **concept** and then a library 

- Responsible only for **rendering**

- **Performance** is the primary goal

--

### Components

- You should break down the layout into smaller pieces - **components**

- Components are built on top of other components - **components tree**

- When problem becomes more challenging you should split a component into smaller components

--

### Components 

- In the component-driven development, you won't see the whole site in one template.

- It makes things easier to **understand**, to **maintain** and to cover with **tests**.

- Each component should respect **Single Responsibility Principle**.

--

### Virtual DOM

- Operating on DOM is heavy so React introduces **Virtual DOM** and **diff algorithm**.

- DOM elements are untouched as much as possible.

- React isolates the changes between the old and new virtual DOM and then only **updates the real DOM with the necessary changes**.

--

### Virtual DOM

- Finding the minimal number of modifications between two arbitrary trees is a O(n<sup>3</sup>) problem.

- React uses simple and yet powerful heuristics to find a very good approximation in O(n).

--

### Diff algorithm

#### Components

React will match only components with the same class.

![Components](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/3.png "Components")

--

### Diff algorithm

#### Level by Level

React only tries to reconcile trees level by level.

![Level by Level](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/1.png "Level by Level")

--

### Diff algorithm

#### List

Components are matched by provided keys. 

![List](http://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/2.png "List")

--

### Rendering entry point

Mounting DOM element

```html
<div id="main"></div>
```

Render entry point

```js
import React from 'react';
import ReactDOM from 'react-dom';

import App from './app';

ReactDOM.render(<App />, document.querySelector('#main'));
```

--

### React Component

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

**Important:** ```render``` method requires single root element.

--

### JSX

JavaScript syntax extension

###### With JSX

```xml
<Nav>
    <Header className="primary">Hello world</Header>
    <Link active>Home</Link>
    <Logo />
</Nav>
```

###### Without JSX

```js
React.createElement(Nav, null,
    React.createElement(Header, { className: "primary" }, "Hello world"),
    React.createElement(Link, { active: true }, "Home"),
    React.createElement(Logo, null),
);
```

--

### Props

 - **Unidirectional data flow** from the top to the bottom of the components tree.

 - Component's input is called **props**
  
 - Props are available as ```this.props```

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

 - Every component can have its own **state**.
 
 - State can be set only with the ```setState``` method.
 
 - Calling ```setState``` triggers re-rendering the component and all of its children.
 
 - To set an initial state before any interaction occurs use the ```getInitialState``` method.

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

 - Components are just **state machines**
 
 - Try to keep as many of your components as possible **stateless**
 
 - Common pattern: several stateless components that just render data, and a stateful component above them all

--

### Props vs State

![Unidirectional flow](../images/unidirectional-flow.png "Unidirectional flow")

--

### State: interactivity

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

**Autobinding:** event handlers have automatically bound ```this```

--

### State: best practices

State should contain data that a component's event handlers may change to trigger a UI update.

--

### State: anti-patterns

 - **Computed data:** Instead, recalculate data in ```render``` method.
 
 - **React components:** Build them in ```render``` method.
 
 - **Duplicated data from props**: Props in ```getInitialState``` is an anti-pattern. **Use props as the source of truth where possible.** 

--

### Component Lifecycle

#### ```render```

 - The only **required** method

--

### Case study

![Sample app](../images/wireframe.png "Sample app")

--

### Case study: Top level components

![Top level components](../images/top-components.png "Top level components")

--

### Case study: Lower level components

![Lower level components](../images/bottom-components.png "Lower level components")

