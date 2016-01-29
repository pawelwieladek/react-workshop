title: React ES6 Workshop
author:
  name: Pawel Wieladek
  twitter: pawelwieladek
  url: http://pawelwieladek.com
style: style.css
output: public/index.html
controls: false
progress: true

--

# React & ES6
## Workshops

--

### Arrow functions

Anonymous function

```js
[1, 2, 3, 4].map(x => Math.pow(x, 2));    // [1, 4, 9 ,16]
```

Lexical this
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
