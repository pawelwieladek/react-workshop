title: React ES6 Workshop
author:
  name: Pawel Wieladek
  twitter: pawelwieladek
  url: http://pawelwieladek.com
style: style.css
output: public/build/es6.html
controls: false
progress: true

--

![ES6](../images/js-logo.png "ES6")

# ES6

--

### Plac zabaw

[JS Bin](http://jsbin.com/?js,console) (Switch language to ES6 / Babel)

### Dokumentacje

[Learn Babel](http://babeljs.io/docs/learn-es2015/)

[MDN](https://developer.mozilla.org/en-US/)

[2ality](http://www.2ality.com/)

--

### Deklaracje zmiennych

- ```var``` - **hoisted**
- ```let``` - **block-scoped**
- ```const``` - **block-scoped**

--

### Deklaracje zmiennych

#### Var

```js
var a = 1;

if (a === 1) {
  // hoisted
  var b = 2;
}

console.log(a);   // 1
console.log(b);   // 2
```

--

### Deklaracje zmiennych

#### Let

```js
let c = 1;

if (c === 1) {
  let d = 2;
}

console.log(c);   // 1
console.log(d);   // ReferenceError: d is not defined
```

--

### Deklaracje zmiennych

#### Const

```js
const PI = 3.14;
PI = 3.141593;    // throws "PI" is read-only
```

--

### Nowe własności obiektów

```js
var name = "Pawel", age = 24;

var me = { name, age };

console.log(me.name);           // Pawel
console.log(me.age);            // 24
```

--

### Nowe własności obiektów

```js
var me = {
  name: 'Pawel',
  sayHi() {
    return `Hi, I'm ${this.name}!`;
  }
};

console.log(me.sayHi());        // Hi, I'm Pawel!
```

--

### Nowe własności obiektów

Getter

```js
var me = {
  firstName: 'Pawel',
  lastName: 'Wieladek',
  get name() {
    return `${this.firstName} ${this.lastName}`;
  }
};

console.log(me.name);     // Pawel Wieladek
```

--

### Nowe własności obiektów

Setter

```js
var me = {
  counter: 0,
  firstName: 'Pawel',
  get greeting() {
    return `Hi, I'm ${this.firstName}!`;
  },
  set name(value) {
    this.firstName = value;
    this.counter++;
  }
};

me.name = 'Pablo';

console.log(me.greeting);           // Hi, I'm Pablo!
console.log(me.counter);            // 1
```

--

### Nowe własności obiektów

```js
// Computed property names

function createIconClasses(name, size) {
  return {
    [`fa-${name}`]: !!name,
    [`fa-${size}`]: !!size
  });
}

let iconClasses = createIconClasses('square', 'lg');

console.log(iconClasses);   // { 'fa-square': true, 'fa-lg': true }
```

--

### Arrow functions

```js
[1, 2, 3, 4].map(x => Math.pow(x, 2));    // [1, 4, 9, 16]

['a', 'b', 'c', 'd'].map((value, index) => [index, value]);    
// [[0, 'a'], [1, 'b'], [2, 'c'], [3, 'd']]
```

--

### Arrow functions

```js

function Exp(base, exponents) {
    this.base = base;
    this.exponents = exponents;
}

Exp.prototype.calculate = function() {
    return this.exponents.map(n => {
        return Math.pow(this.base, n);
    });
};

var e = new Exp(2, [1, 2, 3, 4]);
console.log(e.calculate());        // [1, 4, 9, 16]
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
`Hello ${name}!`;     // Hello world!
```

--

### Destructuring

```js
// List matching

var [a, b, c] = [1, 2, 3, 4];
console.log(a);   // 1;
console.log(b);   // 2;
console.log(c);   // 3;
```

--

### Destructuring

```js
// List matching with empty element
var [a, , c] = [1, 2, 3, 4];
console.log(a);   // 1;
console.log(c);   // 3;
```

--

### Destructuring

```js
// Object matching

function createPerson() {
  return { firstName: 'Pawel', lastName: 'Wieladek' };
}

let { firstName, lastName } = createPerson();

console.log(firstName);   // Pawel
console.log(lastName);    // Wieladek
```

--

### Destructuring

```js
// Object matching with rename

function createPerson() {
  return { firstName: 'Pawel', lastName: 'Wieladek' };
}

let { firstName: f, lastName: l } = createPerson();

console.log(f);           // Pawel
console.log(l);           // Wieladek
```

--

### Destructuring

```js
// Object matching in function parameter

function sayHi({ firstName, lastName }) {
  return `Hi, I'm ${firstName} ${lastName}!`;
}

let greeting = sayHi({ firstName: 'Pawel', lastName: 'Wieladek' });

console.log(greeting);    // Hi, I'm Pawel Wieladek!
```

--

### Default arguments

```js
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5));                   // 5
console.log(multiply(2, 3));                // 6
```

--

### Rest arguments

```js
function append(a, ...b) {
    // b is an Array
    return a + ', ' + b.join(', ');
}

console.log(append('apple', 'orange', 'plum', 'kiwi));      // apple, orange, plum, kiwi
```

--

### Spread operator

```js
function add(x, y, z) {
  return x + y + z;
}

let args = [1, 2, 3];
console.log(add(...args));              // 6
```

--

### Klasy

- dziedziczenie oparte na prototypach
- konstruktory
- metody klasy bazowej
- metody statyczne i instancji

--

### Klasy

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  toString() {
    return 'x = ' + this.x + ', y = ' + this.y;
  }
}

let p = new Point(1, 2);
console.log(p.toString());      // x = 1, y = 2

// toString overridden
console.log('' + p);            // x = 1, y = 2
```

--

### Klasy

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
    return super.toString() + ', color = ' + this.color;
  }
}

let p = new ColorPoint(2, 3, 'red');
console.log(p.toString());      // x = 1, y = 2, color = red

let q = ColorPoint.createDefault();
console.log(q.toString());      // x = 0, y = 0, color = black
```

--

### Moduły

#### Named exports

math.js

```js
export const PI = 3.14;

export function square(x) {
    return x * x;
}
```
 
main.js

```js
import { square, PI } from './math';

console.log(PI);                            // 3.14
console.log(square(8));                     // 64
```

```js
import * as math from './math';

console.log(maths.PI);                      // 3.14
console.log(maths.square(8));               // 64
```

--

### Moduły

#### Default exports

square.js

```js
export default function square(x) {
    return x * x;
}
```
 
main.js

```js
import square from './square';

console.log(square(8));                     // 64
```

[ECMAScript 6 modules: the final syntax](http://www.2ality.com/2014/09/es6-modules-final.html)

--

### Promise

```js
function fetch() {
    return new Promise((resolve, reject) => {
        request.get('https://api.github.com/zen')
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