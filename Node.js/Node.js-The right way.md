### How Node.js Apps Work

Node.js couple JS with an event loop for quickly dispatching operations when events occur. Many JS environments use an event loop but it is a core feature of Node.js.

As long as there's something left to do, Node.js's event loop will keep spinning. Whever an event occurs, Node.js invokes any callbacks that are listening for that event.

In Node.js, a module is a self-contained bit of Javascript that provides functionality to be used elsewhere. The output of require() is usually a plain old JS object , but may also be a function. Modules can depend on other modules , much like libraries in other programming environments which import or #include other libraries.

Processes are important in Node, It's pretty common in Node.js development to spawn seperate processes as a way of breaking up work , rather than putting everything into a one big Node.js program.

### Spawning a Child Process

EventEmitter is a very important class in Node.js. It provides a channel for events to be dispatched and listeners to be notified. Many object you'll encounter in Node.js inherit from EventEmitter.

### Binding a Server to a TCP port

TCP socket connections consists of two enpoints. One enpoint binds to a numbered port while the other endpoint connects to a port.

In Node.js, the bind and connect operations are provided by the net module. Binding a TCP port to listen for connections looks like this.

```javascript
const net = require("net");
server = net.createServer(connection => {});

server.listen(60300);
```

The net.createServer method takes a callback function and returns a Server object. Node.js will invoke the callback function whenever another endpoint connects. The connection parameter is a Socket object that you can use to send or receive data.
