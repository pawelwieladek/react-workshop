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

# React

--

### ReactDOM.render

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
