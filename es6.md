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

### Arrow functions

```js
[1, 2, 3, 4].map(x => Math.pow(x, 2));    // [1, 4, 9, 16]

['a', 'b', 'c', 'd'].map((value, index) => [index, value]);    
// [[0, 'a'], [1, 'b'], [2, 'c'], [3, 'd']]
```

--

### Arrow functions

Binding ```this```

```js
let exponentiation = {
  base: 2,
  exponent: [1, 2, 3, 4],
  calculate() {
    return this.exponent.map(n => {
      return Math.pow(this.base, n);
    });
  }
};
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

### Declaring variables

- ```var``` is **hoisted**
- ```let``` is **block-scoped**
- ```const``` is **block-scoped**

--

### Declaring variables

#### Var hoisting

```js
var a = 1;

if (a === 1) {
  var b = 2;
}

console.log(a);   // 1
console.log(b);   // 2
```

--

### Declaring variables

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

### Declaring variables

#### Const

```js
const PI = 3.14;
PI = 3.141593;    // throws "PI" is read-only
```

--

### Enhanced objects

```js
// given
var a = "foo", b = 42, c = {};

// then
var o = { a, b, c };

// equals to
var o = {
  a: a,
  b: b,
  c: c
};
```

--

### Enhanced objects

```js
var me = {
  _name: 'Pawel',
  sayHi() {
    return `Hi, I'm ${this._name}!`;
  }
};
// equals to
var me = {
  _name: 'Pawel',
  sayHi: function() {
    return `Hi, I'm ${this._name}!`;
  }
};
```

--

### Enhanced objects

Getter

```js
var me = {
  _name: 'Pawel',
  get name() {
    return this._name;
  }
};

console.log(me.name);     // Pawel
```

--

### Enhanced objects

Setter

```js
var me = {
  _name: 'Pawel',
  get name() {
    return this._name;
  }
  set name(value) {
    this._name = value;
  }
};

me.name = 'Pablo';

console.log(me.name);     // Pablo
```

--

### Enhanced objects

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

###### Tip: Use when function has many arguments

--

### Classes

- prototype-based inheritance
- constructors
- super calls
- instance and static methods

--

### Classes

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

### Classes

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

