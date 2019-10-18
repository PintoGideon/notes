### The React Ecosystem

As our app scales, we need a bundler to bundle these files together. 

### Babel

Before we give our React code with JSX to a browser, we first need to transform it to a syntax which the browser will understand.

You can use it to covert ES2015+ code into a JS backward compatible version that current or older browsers can understand.

### Webpack is a module bundler. 
Webpack intelligently bundles all the imports and exports from your codebase into a single file your browser can understand.

The output that webpack gives you does not contain export and import statements.

### Everything you should know about NPM

npm is a package manager for Node. It contains two parts.

- Registry for hosting the package
- CLI for accessing the packages

```javascript

npm init //initialize your project with npm

```

Whenever you install a package, the source code for the package will be put inside node_modules. The package.json contains all the meta info for your project. It also contains a list of packages and the versions they depend on. The devDependencies are the packages your application needs during development. The scripts in package.json automate our tasks.

### Installing packages

```javascript

npm install react
npm install webpack --save-dev
```

### Publishing packages

Create an account on npm. The package needs to have the name , version and main properties.

```javascript
npm login
npm publish
```

### Webpack

Why does it exist? Why do I need it?

At it's core, webpack is a module bundler. It examines all of the modules in the code and creates a dependency graph and intelligently puts them into one or more bundles that your index.html file can reference.

```javascript
npm install webpack webpack-cli

```

All your configurations will go into a webpack.config file.

1- The entry point of your application
2- Which transformations, if any, to make on your code
3- The location to put the newly formed bundles.

### The entry point

If we give webpack our entry file, it will create a dependency graph with all the files that are imported and exported.

```javascript
module.exports = {
	entry: './app/index.js'
};
```

### Transformations with Loaders.

When webpack is building your dependency graph by examining your import and require statements, it's only able to process javascript and json files. If you import a css or a svg file, it will fail.

This is where the loaders can help us out.

```javascript

npm install svg-inline-loader --save-dev
npm install css-loader --save-dev
npm install style-loader --save-dev
npm install babel-loader --save-dev

```

```javascript

module.exports={
entry:'./app/index.js',
module:{
rules:[
{test:/\.svg$/, use:'svg-inline-loader'}
{test:/\.css$/, use:['style-loader','css-loader']},
}
}

```

Now we want to tell webpack where to create the byndle for us.

```javascript

output:{
path:path.resolve(__dirname,'dist'),
filename:'index_bundle.js'
}
```

- Webpack grabs the entry point located at `./app/index.js`.
- It examines all of our 'import' and 'require' statements and creates a dependency graph
- webpack starts creating a bundle, whenever it comes across a path we have a loader for, it transforms the code according to that loader then adds it to the bundle.
- Takes the final bundle and outputs it at 'dist/index_bundle.js'.

### Plugins

The HTMLWebpackPlugin generates an index.html page, put it in our dist/folder with a <script> tag that references the newly created bundle.

```javascript
npm install html-webpack-plugin --save-dev

const HTMLWebpackPlugin=require('html-webpack-plugin')l

plugins:[
new HTMLWebpackPlugin(),
new webpack.EnvironmentPlugin({
'NODE_ENV':'production'
})
]

```

### Environment Plugin

We would want to set our process.env variable to production before we deploy our code. Webpack provides a EnvironmentPlugin and it comes with the webpack namespace so we don't have to download it. We also want to minify our code and reduce our bundle size by setting our 'mode' property to 'production'.

```javascript
npm run build // build our app for production
npm run start  // start our development server
```

```javascript
"scripts":{
"build":"SET NODE_ENV='production' && webpack'
}

```

The webpack DevServer is a development server for webpack. It doesn't generate /dist. It stores files in cache and also enables live reloading.

```javascript
npm install webpack-dev-server --save-dev
"scripts":{
"build":"NODE_ENV='production' webpack",
"start":"webpack-dev-server"
}
```
