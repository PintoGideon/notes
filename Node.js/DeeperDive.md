Each Node.js process has a set of built-in functionality, accessible through the global process module. The process module need not to be required - it is somewhat literally a wrapper around the currently executing process,  and many of the methods it exposes are actually wrappers around calls into some of Nodejs core C libraries.

```javascript
process.stdout.write("hello world")
```

The simplest way of retrieving arguments in Nodejs is via the ```process.argv``` array. This is a global object that you can use without 
importing any additional libraries to use it. You simply need to pass arguments to a Node.js application, just like we showed earlier, 
and these arguments can be accessed within the application via the process.argv array.

# Minimist
Another way to retrieve command line arguments in a Node.js application is using the minimist module. The minimist module will parse 
arguments from the process.argv array and transform it in to an easier-to-use associative array. 
In the associative array you can access the elements via index names in addition to the index numbers.

```javascript
var args = require('minimist')(process.argv.slice(2), {
	boolean: ['help', 'in'],
	string: ['file']
});
```


Using ```javascript __dirname``` in a Node script will return the path of the folder where the current JavaScript file resides.
Using ./ will give you the current working directory. It will return the same result as calling process.cwd().


# Streams

I/O in node is asynchronous, so interacting with the disk and network involves passing callbacks to functions. You might be tempted to write code that serves up a file from disk like this:

```javascript
var http = require('http');
var fs = require('fs');


var server = http.createServer(function (req, res) {
    fs.readFile(__dirname + '/data.txt', function (err, data) {
        res.end(data);
    });
});
server.listen(8000);
```

This code works but it's bulky and buffers up the entire data.txt file into memory for every request before writing the result back to clients. If data.txt is very large, your program could start eating a lot of memory as it serves lots of users concurrently, particularly for users on slow connections.

The user experience is poor too because users will need to wait for the whole file to be buffered into memory on your server before they can start receiving any contents.
Luckily both of the (req, res) arguments are streams, which means we can write this in a much better way using fs.createReadStream() instead of fs.readFile():

```javascript
var http = require('http');
var fs = require('fs');

var server = http.createServer(function (req, res) {
    var stream = fs.createReadStream(__dirname + '/data.txt');
    stream.pipe(res);
});
server.listen(8000);
```
This example is refereced from https://github.com/substack/stream-handbook

# Transform Streams

What are transform streams?
Node.js ```javascript transform streams``` are streams which read input, process the data manipulating it, and then outputing new data.

```javascript

function processFile(inStream) {
	var outStream = inStream;

	var upperStream = new Transform({
		transform(chunk, enc, cb) {
			this.push(chunk.toString().toUpperCase());
			cb();
		}
	});

	outStream = inStream.pipe(upperStream);

	var targetStream = process.stdout;
	 outStream.pipe(targetStream);
}

```
