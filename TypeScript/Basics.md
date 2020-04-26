### Typescript

My struggle with Typescript is real.... Let's do a deep dive.

### TypeScript vs JavaScript

Typescript is a typed superset of Js that compiles to
plain javascript

```ts
tsc --init

```

This command generates a tsconfig.json file.

```ts
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "outDir": "dist"
  }
}
```

Hovering over the properties for the `compilerOptions` object will give us the values we can provide for the keys.

The typescript compiler will compile the .ts file into a .js file and place it in the dist folder.

### Setting up Webpack for Typescript

```javascript
module.exports = {
  entry: "./src/app.ts",
  output: {
    filename: "app.js",
    path: __dirname + "./dist",
  },
  resolve: {
    extensions: [".ts", ".js"],
  },
  module: {
    rules: [{ test: /\.ts$/, use: "awesome-typescript-loader" }],
  },
  devServer: {
    port: 3000,
  },
};
```
