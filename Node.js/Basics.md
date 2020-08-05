# From Client Side Development to Full Stack

Our users open twitter.com, they need code and data to load twitter.com
on their computers.
- What code/data do they need to load?
- Where's the code/data coming from?

# Sending the data right back from the server requires using multiple features of the computer


- Network socket- Receive and send back messages over the internet
- Filesystem- that's where the html/css/javascript code is stored in files
- CPU- For cryptography and optimizing hashing passwords
- Kernel- I/O management

We are going to have C++ and JavaScript to work together to make the above possible.
C++ has many features that lets it directly interact with 
OS directly. 

Javascript has ton of built in labels that trigger Node Features that are built on C++ to use the computer internals.

# Using the http feature of Node to set up an open object


```javascript
const server=http.createServer();
server.listen(80)
```
The ```http.createServer() ``` set's up a network feature of Node specialzing in http protocols
The libuv library links C++ code and Node with the computer's internal structure which opens a socket and is ready to receive incoming messages.

Node auto-runs the code (function) for us when a request arrives from a user.
```javascript

function doOnIncoming(incomingData, functionToSetOutgoingData){
functionToSetOutgoingData.end('Welcome to Twitter');
}

const server = http.createServer(doOnIncoming)

```
**Two parts of calling a function**
- Parenthesis
- Insert Arguments

Let's look at the complete process:

- The client sends an HTTP request. This request is a encoded as a string of characters The **doInComing** function will be autorun by Node by adding a paranthesis at the end. 
- The function needs to now notify Node with two messages.
   - The function has finished executing
   - The data it needs to send as a response
   
- How does the **doInComing** function get access to these two messages? Node takes care of this.
- Node autocreates **two objects** as soon as the incoming message comes in.
- Node will parse the incoming request and save the url in one of the object for us
  http://twitter.com/tweet
    ```javascript
    {
    url:'/tweet'
    }
    ```
- The object created will be automatically inserted in the **doInComing** function as an argument.
   The 1st object with the url property is inserted as an argument.
   The 2nd object has functions including one called **end** which which can be called
   with some data from the execution context of the **doInComing** function.
   
   ![new doc 2019-05-10 23 32 30-1](https://user-images.githubusercontent.com/15992276/57564641-7961f280-739e-11e9-926e-2e240cbd68f4.jpg)
   
   
   # Messages are sent in HTTP format- The 'protocol' for browser-server interaction
   
   ```javascript
   const tweets=["Hi","Hello","Tweet"]
   
   function doOnIncoming(incomingData, functionsToSetOutgoingData){
    const tweetNeeded=incomingData.url.slice(8)-1
    functionsToSetOutgoingData.end(tweets[tweetNeeded]) 
   }
  const server=http.createServer(doOnIncoming)
  server.listen(80)
  ```
 # The Workflow
 
 ![new doc 2019-05-11 01 31 22-1](https://user-images.githubusercontent.com/15992276/57565502-7b33b200-73ae-11e9-9adc-94cfc155ef73.jpg)

- http feature which is really a network connection where it is going to set up an open channel to the internet.
- Store the doOnIncoming function to the autorun when we get an incoming message
- Store a object at the server which will have a listen function. 

The function needs two things that need to happen
- Have paranthesis for invocation
- Have arguments passed into the function

# Getting access to Node's built in features

We have to tell nodes we want to access to each of it's C++ features independently- we get a
built in function to do this.

```javascript
const http=require('http')
```

### Node will broadcast the event depending as soon it receives an incoming message from Port 80.

- The server has access to functions like ```listen``` and 
  ```on``` which will modify the http feature in Node 
  which controls the computer's internals.
  
  ```javascript
  function doOnIncoming(incomingData, functionsToSetOurOutgoingData){
  functionsToSetOurOutgoingData.end('Welcome to Twitter');
  }
  
  function doOnError(infoOnError){
  console.error(infoOnError)
  }
  
    server=http.createServer();
  server.listen(80);
  
  server.on('request', doOnIncoming)
  server.on('clientError',displayError)
  
  ```
  
  
  ![new doc 2019-05-11 22 11 00-1](https://user-images.githubusercontent.com/15992276/57577075-834b2a80-745e-11e9-9709-74b96430bad4.jpg)


- The C++ output of running createServer() allows you to setup a default port.
- The JS output of running createServer() is an object with two methods 'listen' and 'on'
- The 'on' and 'listen' methods can edit Node's underlying functionality. 
  ```javascript
  server.on('request',doOnIncoming)
  ```

When the event 'request' is broadcasted upon receiving an incoming message, Node autoruns the doOnIncoming function.
If the incoming message is corrupted, Node autoruns the ***doError*** function.

The 2nd argument is raw data that needs to be
send back which we can append status to that
and the user will receive a 404 error.
   
### A sample Snippet
```javascript
function cleanTweets(tweetsToClean){
//Code that removes bad tweets
}

function useImportedTweets(errorData, data){
const cleanedTweetsJson=cleanTweets(data);
const tweetsObj=JSON.parse(cleanedTweetsJson)
console.log(tweetsObj.tweet2)
}

fs.readFile('./tweets.json',useImportedTweets)
```
![new doc 2019-05-14 11 52 38-1](https://user-images.githubusercontent.com/15992276/57712711-9b8a9780-7660-11e9-8f95-866ca40cae04.jpg)

Javascript gets access to the computer's file system by accessing Node's C++ features via the libUV library. 
libuv provides a threadpool which can be used to run user code and get notified in the loop thread. This thread pool is internally used to run all file system operations, as well as getaddrinfo and getnameinfo requests.
The next few steps are identical to the one's earlier.


Most of the backends behind websites don’t need to do complicated computations. Our programs spend most of their time waiting for the disk to read & write , or waiting for the wire to transmit our message and send back the answer.

IO operations can be orders of magnitude slower than data processing.

***What if Node used the 'event' (message broadcasting) pattern to send out a message(event)
each time a sufficient batch of the json datahead is loaded in***

![new doc 2019-05-14 15 33 30-1](https://user-images.githubusercontent.com/15992276/57726840-8a04b800-767f-11e9-998f-ac2ea5cdbfab.jpg)

At each point, take that data and start cleaning it in batches.

```javascript

let cleanedTweets="";

function cleanTweets(tweetsToClean){
//
}

function doOnNewBatch(data){
cleanedTweets+=cleanTweets(data);
}

const accessTweetArchive=fs.createReadStream('./tweets.json');
accessTweetArchive.on('data', doOnNewBatch);
```
Even though V8 is single-threaded, the underlying C++ API of Node isn't. It means that whenever we call something that is a non-blocking operation, Node will call some code that will run concurrently with our javascript code under the hood. Once this hiding thread receives the value it awaits for or throws an error, the provided callback will be called with the necessary parameters.


```javascript

function useImportedTweets(errorData, data){
const tweets=JSON.parse(data)
console.log(tweets.tweet)
}

function immediately(){console.log("Run me last")

function printHello(){console.log("Hello")}

function blockFor500ms(){
//Block JS thread directly for 500ms
//
}

setTimeout(printHello,0);
fs.readFile('/tweets.json',useImportedTweets);
blockFor500ms();
console.log("Me First")
setImmediate(immediately);

```


![new doc 2019-05-15 15 50 25-1](https://user-images.githubusercontent.com/15992276/57804784-00222100-774b-11e9-82a0-c871c10c1067.jpg)

- Call Stack: JavaScript keeps track of what function is being run where it
was run from. Whenever a function is to be run, it is added to the call stack.

- Callback queue: Any function delayed from runnning are added to the callback queue when the 
background Node task has completed

- Event Loop: Determines what function/node to run next from the queue

***The Workflow of the above code snippet in Node***

- The ``` setTimeout ``` function creates a timer in Node and set's ```printHello```
as a function to be autorun by Node.
- Any function that is Autorun by Node get's bottom priority after all the
JavaScript code is run. It goes to our first queue named as the ***the timer queue***
- The fs.readFile sets up the instance of the fs feature of Node to access the file
system with ***libuv***. It sets up a background thread to handle all the incoming data.
- When the file is fetched from the path, the ***userImportedTweets*** function
is going to be triggered.
- When our call stack has no functions to run,the event loop check our queues. The first
queue we check is the Timer queue and puts the function ***printHello*** and console.log's hello

## Rules for the automatic execution of the JS code by Node

1. Hold each deferred function in one of the task queues when the Node
   Background API completes
2. Add the function to the call stack (i.e execute the function) only when the call   
   stack is totally empty 
3. Prioritize functions in Timer queue over I/O queue, over setImmediate('check')
   queue.




















