### Basic Routing

To create a useful API, we should be able to respond
differently depending on the requested url path.

The `url` property of the req object will always contain the full path of the client request.

```
http://localhost:3000 -----> path is "/"
http://localhost:3000/dashboard----> path is "/dashboard"
```

### Express

A fast unopinionated, minimist web framework for Node.js used in production environments.

`express` is a drop in replacement for the core `http` module. The express module comes with a baked in router unlike the `http` module.

We can create a server instance with express()

```javascript
const app = express();

/// Taking advantage of the baked in routing

app.get("/", (req, res) => {
  res.end("Hello there");
});
```

`express` is built upon the core `http` module though.
`express` will also automatically parse search query parameters (e.g?input=hi) and make them available for us as an object (e.g {input:'hi'}).

### \_\_dirname

```javascript
const productsFile = path.join(__dirname, "../products.json");

try {
  const data = await fs.readFile(productsFile);
  res.json(Json.parse(data));
}
```

Here we use the core path module along with the globally available
`__dirname` to create a reference to where we are keeping the
products.json file.

`__dirname` is useful for creating absolute paths to files using the
current module as a starting point.

Unlike require, `fs` methods like `fs.readFile()` will resolve relative paths using the cwd (Current working directory). This means that our code will look in different places for a file depending on which directory we run the code from.

Suppose there are two paths:
`/Users/fullstack/project/example.js and /Users/fullstack/project/data.txt`

In `example.js`, we have `fs.readFile('data.txt',console.log)`. If we run `node example.js` from within `/Users/fullstack/project`, the `data.txt` would be loaded and logged correctly.
But if you run `node example.js` from `/Users/fullstack`, we would get an error.

**dirname is always set to the directory name of the current module.
If we created a file `/Users/fullstack/example.js` and `console.log(**dirname)`, when we run`node example.js`from`Users/fullstack`, we could see that`\_\_dirname`is`/Users/fullstack/`.

`res.json()` automaticallyy sets the appropriate content-type header and formats the response for us.
