> We should design our software seperated into different pieces where each piece has a specific purpose and clear boundaries for how it interacts with other pieces. In software, these pieces are called modules-Tyler Mcginnis

# Modules

Modules comprises of three parts

- imports (dependencies)
- code
- exports

### IIFE

You can find detailed notes [here](https://github.com/PintoGideon/JS-for-reference/tree/master/Creating%20a%20Library)

There are a few downsides to an IIFE.
We still have a single property on the global namespace.
The order of the script tags matter.

### Our Module Standard

- File Based
- Explicit imports
- Explicit exports

> The CommonJS group defined a module format to solve JS scope issues by making sure each module is executed in it's own namespace. This is achieved by forcing modules to explicitly export those variables it wants to expose to the 'universe', and also by defining those other modules required to properly work- Webpack Docs

### Export

```javascript
var users = ['Tyler', 'Sarah', 'Dan'];

function getUsers() {
	return users;
}

module.exports.getUsers = getUsers;
```

### Import

```javascript
var users = require('./users');

users.getUsers();
```

### Cons of CommonJS

- The modules are loaded synchronously.
- Browsers don't support CommonJS

### Module Bundler

Module bundler looks at all of your require statements and all of the exports and it intelligently bundles all your modules into a single file that your browser can understand.

### Setting up a Basic Webpack config

```javascript
npm install webpack webpack-cli

//Webpack.config.js

var path=require('path');

module.exports={
    entry:'./app.js',
    output:
    {
    path:path.resolve(__dirname),
    filename:'bundle.js'
    },
    mode:'development'
}

// Now you can do this in your files
module.exports{
    getUsers:getUsers
}

// In an other file
var users=require('./getUsers').getUsers;

// Tell webpack to bundle your modules
//In package.json

"scripts":{
    "start":"webpack"
}

```

Webpack will create a bundle.js file.

### ES6 Modules

- Support Async
- import
- export

### Named Exports

```javascript
//Util.js

export function first(arr) {
	return arr[0];
}

export function last(arr) {
	return arr[arr.length - 1];
}
```

```javascript
import * as utils from './utls';

utils.first([1, 2, 3]); //1
utils.last([1, 2, 3]); //3
```

### Default Exports

```javascript
export default App;
```

```javascript
import leftPad from './utils';
```

### Default and Named Exports

```javascript

export function first(arr){
    return arr[0]
}

export function last(arr){
    return arr[arr.length-1]
}

export default function leftPad(str,len,ch){
    var pad='';

    while(true){
        if(len & 1) pad+=ch
        len>>1
        else break;
    }
    return pad+=ch;
}

```

```javascript
import leftPad, { first, last } from './utils';
```
