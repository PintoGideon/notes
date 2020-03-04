Why webpack?

### ESM

The Ecmascript module spec.

```javascript
import { uniq, forOf, bar } from "lodash-es";
import * as utils from "utils";
```

These modules are reusable. encapsulated, organized, convenient. But there are some issues.

ESM for Node? How do they work in the browser? They are incredibly slow.

Webpack is a module bundler. It lets you write any module format, compiles them for the browser. On top of that , it supports static async bundling.

The most performant way to ship JS today.

### How to use it?

webpack.config.js is just a commonjs module.

the node_modules has a bin folder with the all the excutables for the modules needed by your app. npm allows you to scroipts that hoists the binary package in your scope.
