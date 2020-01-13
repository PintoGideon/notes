### Logical Operators

The && operators represents logical and. It is a binary operatory and its result is true only if both the values given to it are true.

```javascript
console.log(true && false);
//false
console.log(true && true);
// true
```

The || operator denotes logical or. It produces true if either one one of the value given to it is true.

```javascript
console.log(false || true);
//true
console.log(false || false);
//false
```

Not is written as an exclamation mark. It is an unary operator that flips the value given to it.

### Empty values

There are two special values , written null and undefined that are used to denote the absence of a meaningful value. They are themselves values that carry no information.

```javascript
console.log("five" * 2);

//NaN
```

When something doesn't map to a number in an obvious way is converted to a umber, you get the value NaN. Further arithemetic operatons on NaN keep producing NaN.

When comparing values of the same type usig ==, the outcome is easy to predict.

You should get true when both values are the same, except in the case of NaN (non-sensical value).

When types differ, JS uses a complicated and confusing set of rules to determine what to do.
In most cases, it just tries to convert one of the values to the other values type.

However, when null or undefined occurs on either side of the operator, it produces true onlu of both sides are one of null or undefined.

```javascript
console.log(null == undefined);
// true
console.log(null == 0);
//false
```

**_When you do not want automatic type conversions to happen, there are additional operators === and !==.
The first tests if the value is precisely equal to the other and the second check if the value is not precisely equal to the other_**

### Short circuiting of Logical operators

The logical && and || operators handle values of different types in a peculiar way.

They will convert the value on their left side to Boolean type in order to decide what to do, but depending on the operator and the result of that conversion, they will return either the original left-hand value or the right-hand value.

```javascript
console.log(null || "user");
// "user'

console.log("Agnes" || "user");
//"Agnes"
```

We can use this functionality to fall back on a default value.

The && operator works similary but the other way around. When the value to it's left is something that converts to false, it returns that value otherwise it returns the value to its right.

A statement starting with the keyword while creates a loop.

The word while is followed by an expression in paranthesis and then a statement much like if.

**_All string except "" convert to true_**

```javascript
let yourname;

do {
  yourname = prompt("Who are you??");
} while (!yourname);
console.log(yourname);
```

Many loops follow the pattern shown in the while examples. First a "counter" binding is created to track the progress of a loop. The comes a while lopp usually with a test expression that checks whether the counter has reach its end value. At the end of the loop body, the counter is updated to track progress.

### Breaking out of a loop

Having the looping condition produce false is not the only way a loop cal finish.

There is a special statement that has the effect of immediately jumping out of the enclosing loop.

```javascript
for (let current = 20; ; current = current + 1) {
  if (current % 7 == 0) {
    console, log(current);
    break;
  }
}
```

When continue is encountered in a loop body, control jums out the body and continues with the loop's next iteration.

### JSON

Because properties only grasp their value, rather than contain it, objects and arrays are stored in the computer's memory as sequence of bits holding the addresses- the place in memory - of their contents.

If you want to save data in a file for later or send it to another computer over the network, you have to somehow covert these tangle of memory address to a description that can be stored or sent. You could send over your entire computer memory along with the address of the value you are interested in,

What we can do is serialize data. That means it is converted into a flat description.

A popular serialization format is called JSON.

```javascript

{
    "squirrel":False,
    "events":["Work","touched tree","pizza"]
}
```

JS gives us the function JSON.stringify and JSON.parse to convert data to and fro from this format.

### Higher Order Functions

A large program is a costly program and not just because of the time it takes to build. Size almost always involves complexity and complexity confuses programmers.

```javascript
let total = 0,
  count = 1;

while (count <= 10) {
  total += count;
  count += 1;
}
```

Functions that operate on other functions, either by taking them as arguments or by returning them are called higher order functions. Since we have already seen that functions are regular values, there is nothing particularly remarkable about the fact that such functions exist.

Filter takes in a function as a test and returns an array with the values that pass the test.

Map transforms the orginal array using a transformation function and gives us the new array of the same length.

```javascript
function reduce(array, combine, start) {
  let current = start;
  for (let element of array) {
    current = combine(current, element);
  }
  return current;
}
```

### Using a reduce function to flatten an array

```javascript
function flatten(array) {
  //Extract each index out
  //Check if it's an array
  //Push it to the new array

  return array.reduce(function(newArr, toFlatten) {
    return newArr.concat(
      Array.isArray(toFlatten) ? flatten(toFlatten) : toFlatten
    );
  }, []);
}

flatten([
  [1, 3, 4],
  [2, 3]
]);
```

### Encapsulation

The core idea in object oriented programming is to divide programs into smaller pieces.

### Methods

Methods are nothing more tha properties that hold function values.

```javascript
let rabbit = {};
rabbit.speak -
  function(line) {
    console.log(`The rabbit says ${line}`);
  };

rabbit.speak("I'm alive");
```

### Prototypes

```javascript
let empty = {};
console.log(empty.toString);
//Function

console.log(empty.toString());
//object
```

Most objects have a prototype.
A prototype is another object that is used as a fallback source of properties.

You can use Object.create to create an object with a specific prototype.

```javascript
let protoRabbit = {
  speak(line) {
    console.log(`The ${this.type} rabbit says ${line}`);
  }
};

let killerRabbit = Object.create(protoRabbit);
```

The proto rabbit acts as a container for the properties that are shared by all rabbits.

Constructors (all functions, in fact) automatically get a property named prototype which by default holds a plain empty object that derives from Object.prototype.

It is important to understand the distinction between the way a prototype is assosciated with a constructor and the way objects have a prototype

### Maps

```javascript
let ages = new Map();

ages.set("Boris", 39);
ages.set("Liang", 22);
ages.set("Julia", 61);
```

### Polymorphism

When we call the String function on an object, it will call the toString method on the object to created a meaningful string from it.

### Inheritance

Javascript's prototype makes it possible to create a new class much like the old class but with new definitions for some of it's properties. The prototype for the new class derives from the old prototype but adds a new definition.

```javascript
class App extends React.component {
  constructor() {
    super();
  }
}
```

The use of the word extends indicates that the class shouldn't be directly based on the default Object prototype but on some other class.

To initialize App instance, the constructor calls it's superclass's constructor through the super keyword. This is necessary because if this new object is going to behave roughly like a Component, it going to need the instance properties the react.component has.

### Tests

```javascript
function test(label, body) {
  if (!body) console.log(`Failed: ${label}`);
}

test("convert latin text to uppercase", () => {
  return "hello".toUpperCase() == "Hello";
});
```

One way to address this is to use fewer side effects. Again, a programming style that computes new values instead of changing existing data helps. If a piece of code stops running in the middle of creating a new value, no one ever sees the half-finished value, and there is no problem. But that isn’t always practical. So there is another feature that try statements

### Regular Expressions

Programming tools and techniques

A regular expression is a type of object. It can be either constructed with the RegExp constructor or written as a literal value by enclosing a pattern in forward slash(/) characters.

### Sets of Characters

In regular expressions, putting a set of characters between square brackets makes that part of the expression match any of the characters between the brackets.

To invert a set of characters that is to express that you want to match any character execept the ones in the set-- you can write a caret(^) character after the opening bracket.

```javascript
let notBinary = /[^01]/;
notBinary.test("11001000100110");
```

### Repeating Parts of a Pattern

We not know how to match a sinle digit. What if we want to match a whole number- a sequence of one or more digits.

When we put a + sign after something in a regular expression, it indicates that the element may be repeated more than once.

```javascript
console.log(/'\d+'/.test("'123'"));

//true
```

The star(\*) has a similar meaning but also allows the pattern to match zero times.

A question marks makes a part of a pattern optional meaning it may occur zero times or one time.
In the following example, the u character is allowed to occur, but the pattern also matches when it is missing.

```javascript
let neighbor = /neighbou?r/;

neighbor.test("neighbour");
//true
neighbor.test("neighbor");
//true
```

### Choice patterns

So we want to know whether a piece of text contains not only a number but a number followed by one of the words pig, cow, or chicken or any of their plural forms.

```javascript
let animalCount = /\b\d+ (pig|cow|chicken)s?\b/;

animalCount.test("15 pigs");
```

To indicate that a pattern should occur a precise number of times, use braces. Putting {4} after an element, for example, requires it to occur exactly four times. It is also possoble to specify a range this way: {2,4} means the element must occur at least twice and at most four times.

```javascript

let dateTime=/\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2/}
dateTime.test("1-30-2003 8:45")
//true
```

### Grouping Subexpressions

To use an operator like \* or + on more than one element at a time, we need to use paranthesis.

```javascript
let cartoonCrying = /boo+(hoo+)+/i;
```

The test method is the simplest way to match a regular expression. It tells you only whether it matched and nothing else.

Regular expressions have an exec method. An object returned from an exec method has index as a property which tells us where in the string the successful match begins.

It will return null if no match was found.

```javascript
let match = /\d+/.exec("one two 100");

console.log(match);
//100
```

The object returned looks like an array of strings, whose first element is the string that was unmatched. In the previous example, this is sentence of digits that we were looking for.

String values have a match metho that behaves similarly.

When the regular expression contains subexpressions grouped with paranthesis, the text that matches those groups will show up in the array. The whole match is always the first elemet.

The next element is the part matched by the first group.

The \_ (underscore) binding is ignored and used only to skip the full match element in the array returned by exec.

### Word and String Boundaries

Unfortunately, getDate will also happily extract the nonsensical date 00-1-3000 from the string "100-1-30000". A match may happen anywhere in the string, so in this case, it will start at the second character and end at the second-to-last character.

If we want to enforce that the match must span the whole string, we can the markers ^ and $. The caret matches the start of the input string and the $ matches the end.

### The Mechanics of matching

Conceptually, when you use exec or test, the regular expression engine looks for a match in your string by trying to match the expression first from the start of the string.

### THe replace methid

String values have a replace method that can be used to replace part of the string with another string.

```javascript
console.log("papa".replace("p", "m"));
```

The first argument can also be a regular expression, in which case the first match of the regular expression is replaced. When a g option (for global) is added to the regular expression, all matches in the string will be replaced, not just the first.

```javascript

console.log("Borobudur".replace(/[ou/,"a"]))
// Barobudur
```

The real power of regular expression with replace comes from the fact that we can refer to matched groups in the replacement string.

For example, say we have a big string containing the nsames of people, one name per line, in the format lastname, firstname.
If we want to swap those names and remove the comma to get FirstName LastName format, we can use the following code:

```javascript
console.log("Liskov,Barbara\nMcCarthy,John\nWadler,Philip".replace(/(/w+),(/w+)/g, "$2 $1"))
```

The $1 and $2 in the replacement string refer to the paranthesized groups in the pattern,

We can also use a function as a second argument

```javascript
let s = "the cia and fbi";
console.log(s.replace(/\b(fbi|cia)\b/g, str => str.upperCase()));
// the CIA and FBI
```

### Here is an interesting example

```javascript
let stock = "1 lemon , 2 cabbages, and 101 eggs";

function minusOne(match, amount, unit) {
  let amount = Number(amount) - 1;
  if (amount == 1) {
    // only one left, remove the s
    unit = unit.slice(0, unit, length - 1);
  } else if (amount == 0) {
    amount = "no";
  }
  return amount + "" + unit;
}

console.log(stock.replace(/(\d+)(\w+)/g, minusOne));

// no lemon , 1 cabbage, and 100 eggs
```

### Dynamically creating REGEXP objects

There are cases where you might not know the exact pattern you need to match against when you are writing your code.

Say you want to look for the user's name in a piece of text and enclose it in underscore characters to make it stand out.
We can build up a string use the RegExp constructor on that.

```javascript
let name = "harry";
let text = "Harry is a suspicious character.";
let regexp = new RegExp("\\b(" + name + ")\\b", "gi");
console.log(text.replace(regexp, "_$1_"));
```

When creating the \b boundary markers , we have to use two backslashes because we are writing them in a normal string,not a slash-enclosed regular expression. The second argument to the RegExp constructor contains the options for the regular expression-- in this case, "gi" for global and case insensitive.

But what if the name is "dea+h1[]rd" because our user is a nerdy teenager? That would result in a nonsensical regex that won't actually match the user's name.

To work around this, we can add backslashes before any character that has special meaning.

### The search method

The indexof method on string cannot be called with a regular expression. But there is another method, search that does expect a regular expression. Like indexOf, it returns the first index on which the expression was found, or -1 when it wasn't found.

```javascript
console.log(" word".search(/\S/));
// 2
```

### The LastIndex Property

Regular expression objects have properties. Once such property is source, which contains the string that eppression was created from. Another property is lastIndex, which controls, in some limited circumstance, where the next match will start.

### Looping over matches

```javascript
let input = "A string with 3 numbers in it....42 and 88.";
let number = /\b\d+\b/g;
let match;
while ((match = number.exec(input))) {
  console.log("Found ", match[0], "at", match.index);
}
// Found 3 at 14
```

### Backtracking

The regular expression /\b([01]+b|[\da-f]+h)\d+)\b/ matches a binary number followed by a b , a hexadecimal number followed by a h, or a regular decimal number with no suffix character.

### Modules

NPM is two things: an online service where we can download and upload packages and a program (bundled with Node.js) helps you install and manage them.

If a web page consists of 200 different files, and fetching a single over the network takes 50 milliseconds, loading the whole program takes 10 seonds or maybe half thatn you can load several files simultaneously. Web programmers have started using tools to roll their programs back into a single big file before the publish it to the web.

### Asynchronous Programming

The central part of a computer, the part that carries out the individual steps that make up our programs is called the processor.
The programs we have seen so far above are things that keep the processor busy until they have finished their work.

THe speed at which something like a loop manipulates numbers can be executed depends pretty much entirely on the speed of that processor.

### Promises

In the case of asynchronous actions, you could, instead of arranging for a function to be called at some point in the future, return an object that represents this future event.

A promise is an asynchronous action that my may complete at some point and produce a value. The easiest way to create a promise is by calling a Promise.resolve. This function ensures that the value you gave it is wrapped in a promise. If it is already in a promise, it is simply returned-otherwise, you get a new promise that immediately finishs with your value as a result.

```javascript
let fiftenn = Promise.resolve(15);
fiftenn.then(value => console.log(`Got ${value}`));
```

To get the results of the promise , you can use the then method. This registers a callback function to be alled when the promise resolve and produces a value.

But that's not all the then method does. It returns another promise, which resolves to the value that the handler function returns or, if that returns a promise, waits for that promise and then resolves to it's result. A normal value is simply there. A promised value is a value that might already be there or might appeat at some point in the future. Computaion defined in terms of promises acts on such wrapped value and are executed asynchronously as the values become available.

To create a Promise, you can use Promise as a constructor. The constructor expects function as an argument.

### Creating promise for the readStorage function

```javascript

functions storage(nest, name){
	return new Promise(resolve=>{
		nest.readStorage(name, result=>resolve(result))
	})
}

storage(bigOak,"enemies").then(value=>console.log("Got",value))
```

### Failure

Promises can be either resolved or rejected. Resolve handlers are called when the action is successful., and rejections are automatically propogated to the new promise that is returned by them.

Much like resolving a promise provides a value, rejecting one also provides one. There;s a Promise.reject function that creates a new immediately rejected promise.

To explicitly handle such rejections, promises have a catch method that registers a handler to be called when the promise is rejected, similar to how these handlers handle normal resolution.

### Networks are hard.

Since we have established that promises are a good thing, we'll also make our request function return a promise. Callbacks and promise are equivalent.

```javascript
new Promise((_, reject) => reject(new Error("Fail")))
  .then(value => console.log("Handler 1"))
  .catch(reason => {
    console.log("Caught Failure" + reason);
    return nothing;
  })
  .then(value => console.log("Handler 2", value));
```

```javascript
class Timeout extends Error {}

function request(nest, target, type, content) {
  return new Promise((resolve, reject) => {
    let done = false;

    function attempt(n) {
      nest.send(target, type, content, (failed, value) => {
        done = true;
        if (failed) reject(failed);
        else resolve(value);
      });

      setTimeout(() => {
        if (done) return;
        else if (n < 3) attempt(n + 1);
        else reject(new Timeput("Timed out"));
      }, 250);
    }
    attempt(1);
  });
}
```

### Collections of promises

When working with collection of promises running at the same time, the Promise.all function can be useful. It returns a promise that waits for all of the promises in the array to resolve and then resolves to an array of the values that these promises produces. If any promise is rejected, the result of Promise.all is rejected.

```javascript
requestType("ping", () => "pong");

function availableNeighbors(nest) {
  let requests = nest.neighbores.map(neighbor => {
    return request(nest, neighbor, "ping").then(
      () => true,
      () => false
    );
  });
  return Promise.all(requests).then(result => {
    return nest.neighbors.filter((_, i) => result[i]);
  });
}
```

### Generators

When you defined a function function\* , it becomes a generator. When you call a generator, it returns an iterator.

```javascript
function* powers(n) {
  for (let current = n; ; curent *= n) {
    yield current;
  }
}

for (let power of powers(3)) {
  if (power > 50) break;
  console.log(power);
}
```

Initally when you call powers, the function is frozen at the start. Every time, you call next on the iterator, the function runs until it hits a yield expression which pauses it and causes the yielded value to becomes the next value produced.

An async function is a special type of generator. It produces a promise when called, which is resolved when it returns (finishes) and rejected when it throws an exception. Whenever it yields(awaits) a promise, the result of that promise is the result of the await expression.

### The event loop

Asynchronous Programs are executed piece by piece. Each pience may start some actions and schedule code to be executed when the action finishes or fails. In between these pieces, the program sits idle waiting for the next action.

If I call setTimeout from within a function, that function will have returned by the time the callback function is called. And when the callback returns , control does not go back to the function that scheduled it.

As events come in, there are added to a queue and their code is executed one after the other.

### Parsing

The most immediate visible part of a programming language is it's syntax or notation.

A parser is a program that reads a piece of text and produces a data structure that reflects the structure of the program contained at that text.

If the text does not form a valid program, the parser should point out the error. Our language will have a simple and uniform syntax.

### Networks and the Internet

If you put cables between two computers and allow them to send data back and forth, you can do all kinds of wonderful things.

A computer can use a netwrok to shoot bits to another computer. For any effective communication to arise out of this bit-shooting, the computers on both ends must know what the bits are supposed to represent. The meaning of any given sequence of bits depends entirely on the kind of thing that it is tryong to express and on the encoding mechanism used.

A network protocol describes a style of communication over a network.
A tcp connection works as follows: one computer must be waiting or listening for other computers to start talking to it. To be able to listen for different kinds of communication at the same time on a single machine, each listener has a number (called a port) assosciated by it.

Another computer can then establish a connection by connecting to the target machine using the correct port number.

### THe WEB

The WEB is a set of protocols and formats that allow us to vist page in a browser. TO become a part of the web, all you need to do is connect a machine to the internet and have it listen on PORT 80 with the http protocol so that other computer can ask it for documents.

### Layout

You may be have notice that different types of elements are laid out differently. Some, such as paragraphs (p) or headings (h1)take up the whole width of the document and are rendered on seperate lines. Those are called block elements. Others such as links (a) or the (strong) element are rendered on the same line with their surrounding text. Such elements are called inline elements.

A pixel is the basic unit of measurement in the browser.

```javascript
<p style="border:3px solid red">I am boxed in</p>
```

The most effective way to find the precise position of element on the screen is the getBoundingClientRect method.
It returns an object with top, left, botton and right properties indicating the pixel position of the side of the element relative to the top left of the screen.

Laying out a document can be quite a lot of work. In the interest of speed, browser engines does not immediately re-layout a document every time you change it but wait as long as they can. When a JS program that changed the document finishes running, the browser will have to compute a new layout to draw the changed document to the screen.

A program that repeatedly alternates between reading DOM layout info and changing the DOM force a lot of layout computations to happen and will consequently run very slowly.

### Positioning and Animating

The position style property influences layout in a powerful way. By default it has a value of static, meaning the element sits in its normal place in the document.

When it is set to relative, the element still takes up space in the document, but now the top and left style properties can be used to move it relative to that of normal place. When position is set to absolute, the element is remove the normal document flow- that is, it no longer takes up space and may overlap with other elements.

### Handling Events

A system actively notify your code when an event occurs.
Browsers do this by allowing us to register functions as handlers for specific events.

### Events and DOM nodes

Each browser event handler is registered in a context. Event listeners are called only when the event happens in the context of the object they are registered on.

Event handler functions are passed on argument called the event object. If we want to know which mouse button was pressed, we can look at the event objects button property.

### Propogation

For most event types, handlers registered on nodes with children will so receive events that happen in the children. If a button inside the paragraph is clicked, event handlers on the paragraph will also see the click event.

But if both the paragraph and the button have a handler, the more specific handlers gets to go first.

If you have a node containing a long list of buttons, it may be more convenient to register a single click handler on the outer node and have it use the target property to figure out whether a button was clicked.

```javascript
<button>A</button>
<button>B</button>
<button>C</button>
<script>
document.body.addEventListener("click",event=>{
	if(event.target.nodeName=="BUTTON"){
		console.log("Clicked",event.target.textContent)
	}
})
</script>

```

**_ To get precise information on where a mouse event happend, you can look at it's clientX and clientY properties which contain the event's co-ordinates (in pixels) relatives to the top-left corner of the window or pageX or pageY which are relative to the top-left corner of the whole document_**

### Mouse motion.

Every time the mouse pointer moves, a "mousemove" event is fired. This event can be used to track the position of the mouse.

### Scroll Events

Whenever an element is scrolled, a 'scroll' event is fired on it. This has various uses, such as knowing that is user is currently looking at or show indication of progress.
The global innerHeight binding gives us the height of the window, which we have to substract from the total scrollable height.

### Load Event

When a page finishes loading, the load event fires on the window and the document body objects. This is often used to schedule initialization actions that require the whole document to have been built. Remember that the content of script tags is run immediately when the tag is encountered. This may be too soon.

Elements such as images and script tags that load an external files also have a "load" event that indicates the files they reference are loaded. Like the focus-related events , loading events do not propogate.

### Events and the Event Loop

Browser event handlers like other asynchronous notifications. They are schedules when the event occurs but must wait for the scripts that are running to finish before they get a change to run.

The fact that events can be processed only when nothing else is running means that, if the event loop is tied up with other work, any interaction with the page will be delayed. So if you schedule too much work, either with long running event handlers or will lots of short running ones, the page will become slow and cumbersome to use.

For cases , where you really want to do something time consuming without freezing the page, browsers provide something called web workers. A worker is a JS process that runs alongside the main script, on its own timeline.

Imagine that squaring a number is heavy, long running computation that we want to perform in a seperate thread, we could write a file called code/squareworker.js that responde to messgae by computing a square and sending a message back.

```javascript
addEventListener("message", event => {
  postMessage(event.data * event.data);
});
```

To avoid the problems of having multiple threads touching the same data, workers do not share their global scope or any other data withing the environment's script. You have to communicate with them by sending messages back and forth.

```javascript
let squareworker = new Worker("code/squareworker.js");
squareworker.addEventListener("message", event => {
  console.log("The worker responsed:", event.data);
});

squareworker.postMessage(10);
```

The postMessage function sends a message which will cause a "message" event to fire in the receiver. The script that created the worker sends and receives message through the Worker object, whereas the worker talks to the script that created it by sending and listening directly on the global scope.

### Timers

We saw the setTimeout function which schedules a function to run after a given number of milliseoncs.
Sometimes you need to cancel a function you have scheduled. This is done by storing the value returned by setTimeout and calling clearTimeout on it.

```javascript
let bombTimer = setTimeout(() => {
  console.log("BOOM");
}, 500);

if (Math.random() < 0.5) {
  console.log("diffused");
  clearTimeout(bombTimer);
}
```

A similar set of functions ,setInterval and clearInterval are used to set timers that should repeate every X milliseconds.

```javascript
let ticks = 0;
let clock = setInterval(() => {
  console.log("tick", ticks++);
  if (ticks == 10) {
    clearInterval(clock);
  }
}, 200);
```

### Debouncing

Some events have the potential to fire rapidly, many times in a row (the "mousemove" and "scroll" events for example). When handling such events , you must be careful not to do anything too time-consuming or your handler will take up so much time that interaction with the document starts to feel slow.

In the first example, we want to react when the user has typed something, but we don't want to do it immediately for every input event. When they are typing quickly, we just want to wait until a pause occurs. Instead of immediately performing an action in the event handler, we set a timeout. We also clear the previous timeout, so that when events occur close together, the timeout from the previous event will be cancelled.

```html
<textarea> Type something here .....</textarea>
<script>
  let textarea = document.querySelector("textarea");
  let timeout;
  textarea.addEventListener("input", () => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      console.log("typed");
    }, 500);
  });
</script>
```

### HTTP and Forms

"Communications must be stateless in nature such that each request from a client to server must contain all the information necessary to understand the request, and cannot take advantage of any stored context on the server"

- Roy Fielding

### The Prototcol

If you type an address into your browser's address bar, the browser first looks up the address of the server assosciated with it and tries to open a TCP connection to it on port 80, the default port for HTTP traffic.
If the server exists and accepts the connection, the browser might send something like this.

```
GET /18_http.html HTTP/1.1
Host:eloquentjavascript.net
User-agent:Your browser's name

```

Then the server response, through the same connection

```
HTTP/1.1 200 OK
Content-Length: 65585
Content-Type: text/html
Last-Modified: Mon, 08 Jan 2018 10:29:45 GMT
```

The browser takes the part of the response after the blank line, its body and displays it as an HTML document.

```html
<form method="GET" action="example/message.html">
  <p>Name: <input type="text" name="name" /></p>
  <p>Message: <br /><textarea name="message"></textarea></p>
  <p><button type="submit">Send</button></p>
</form>
```

When the <form> element's method attribute is GET (or is omitted) , the information in the form is added to the end of action URL as a query string.

```javascript
GET /example/message.html?name=Jean&message=Yes%3f HTTP/1.1
```

THe question mark indicates the end of the path part of the URL, and the start of the query. It is followed by pairs of names and values, corresponding to the name attribute on the form field elements and the content of those elements respectively.

If we change the method attribute of the HTML form to POST, the HTTP request made to submit the form will use the POST method and put the query string in the body of the request, rather than adding it to the URL.

```javascript

POST /example/message.html HTTP/1.1
Content-length:24
Content-type: application/x-www-form-urlencoded

name=Jean&message=Yes%3F

```

### Fetch

The interface through which browser Javascript can make HTTP requests is called fetch. Since it is relatively new, the conveniently uses promises.

```javascript
fetch("example/data.txt").then(response => {
  console.log(response.status);
});
```

Calling fetch returns a promise that resolves to a response Object holding information about the server's response such as status code and headers.

The first argument to fetch is the URL that should be requested. When the URL doesn't start with a protocol name it is treated relative , which means it is interpreted relative to the current document.
Servers can include a header like this in their response to explicitly indicate to the browser that it is okay for the request to come from another domain.

```html
Access-Control-Allow-Origin: *
```

### Appreciating HTTP

When building a system that requires communication between a JS program running in the browser and the program on a server, there are several different ways to model the communication.

A commonly used model is that of remote procedure calls. In this model, communication follows the patterns of normal function calls , except that the function is actually running on another machine.

Calling it involves making a request to the server that includes the functions name and arguments. The response to that request contains the returned value.

The secure HTTP protocol, used for URLs starting with https://, wraps HTTP traffic in a way that makes it harder to read and tamper with. Before exchanging data, the client verifies that the server is who it claims to be by asking it to prove that it has a cryptographic certificate issued by a certificate authority that the browser recognizes. Next, all data is going over the connection is encryped in away that should prevent eavesdropping and tampering.

### The form as a whole

When a field is contained in a <form> element, its DOM element will have a form property linking back to the form's DOM.
The <form> element in turn, has a property called elements that contains an array-like collections of the fields inside it.

The name attribute of a form field determines the way its value will be identified when the form is submiited. It can also be as a a property name when accessing the form's elements property which acts both as an array-like object and a map.

```javascript

<form action ="example/submit.html">
Name: <input type="text" name="name"><br>
Password: <input type="password" name="password"><br>
<button type="submit">Log in</button>
</form>

<script>
let form=document.querySelector("form")
console.log(form.elements[1].type);
console.log(form.elements.password.type);
</script>
```

### Offset and Pagination\

Typically in an application with a database, you might have more records than you can fit onto a page or in a single result set from a query.
When you and your users want to retrieve the next page of results, the two common options include:

1. Offset Pagination
2. Cursor Pagination

### Offset Pagination

When retrieving data with offset pagination, you would typically allow clients to supply two additional paramters in their query: an offset and a limit.
An offset is simply the number of records you wish to skip before selecting records. This gets slower as the number of records increases because the database still has to read up to the offset number to know where it should start selecting data.

If you want to get the first page of the newest posts from a database, the query might look like this.

```javascript
Post |> order_by(inserted_at::desc) |> limit(20);
```

Then when we want the second page of results , we can include an offset

```javascript
Post |> order_by((inserted_at: "desc")) |> limit(20) |> offset(20);
```

### A few examples on Promise to have a firm understanding

**_Promise.all_**

The all function retunrs a new promise which is fulfilled with an array of fulfillment values for the passed promises or rejects with the reason of the first promise that rejects.

```javascript
function readJsonFiles(filenames) {
  return Promise.all(filenames.map(readJSON));
}

readJsonFiles(["a.json", "b.json"]).done(
  function(results) {},
  function(err) {}
);
```

The Promise.all is a built in method so you don't need to worry about implementing it yourself.

```javascript
function all(promises) {
  var accumulator = [];
  var ready = Promise.resolve(null);
  promises.forEach(function(promise, ndx) {
    ready = ready
      .then(function() {
        return promise;
      })
      .then(function(value) {
        accumulator[ndx] = value;
      });
  });
  return ready.then(function() {
    return accumulator;
  });
}
```

### Node.js

The console.log method in Node does something similar to what it does in the browser. It prints out the piece of text.
But in Node, the text will go to the process's standard output stream, rather than to a browser's JS console.

### Synchronous Generatory

Synchronous generators are special versions of functions definitions and method definitions that always return synchronous iterables.

```javascript
function* genFunc1() {}

const obj = {
  *generatorMethod() {}
};
```

If you call a generator function, it returns an iterable (actually, in iterator that is also iterable). The generator fills that iterable via the yield operator.

```javascript
function* genFunc1() {
  yield "a";
  yield "b";
}

const iterable = genFunc1();
```

Yield pauses a generator function.

1. Function-calling it returns an iterator iter
2. Iterating over iter repeatedly invokes iter.next(). Each time, we jump into the body of the generator function until there is a yield that returns a value.

```javascript
let location = 0;
function* genFunc2() {
  location = 1;
  yield "a";
  location = 2;
  yield "b";
  location = 3;
}

const iter = genFunc2();

iter.next(); //location=0   {value:0, done:false}
iter.next(); //location=1 {value:1,done:false}
iter.next(); //location=2 {value:2,done:false}
```

### Why does yield pause execution?

What are the benefits of yield pausing execution? Due to pausing, generators provide many of the features of coroutines. For example, when you ask for the next value of an iterable, that value is computed lazily.

```javascript
function* mapIter(iterable, func) {
  let index = 0;
  for (const x of iterable) {
    yield func(x, index);
    index++;
  }
}

const iterable = mapIter(["a", "b"], x => x + x);
```

### Calling generators from generators

```javascript
function* bar() {
  yield "a";
  yield "b";
}

function* foo() {
  yield* bar();
}
```

```javascript
class BinaryTree {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right;
  }

  *[Symbol.iterator]() {
    yield this.value;
    if (this.left) {
      yield* this.left;
    }
    if (this.right) {
      yield* this.right;
    }
  }
}
```

### A look into Synchronicity

JavaScript executes tasks sequentially in a single process. The loop is also called the event loop because events such as clicking a mouse, add tasks to the queue.

Due to this style of cooperative multitasking , we don't want a task to block other tasks from being executed while, for example, it waits for results coming from a server.

What if divide() needs a server to compute it's result. The caller shouldn't have to wait until the result is ready; it should be notified when it is. One way of delivering the result asynchronously is by giving divide() a callback function that it uses to notify the caller.


### The event loop

By default, JS runs in a single process.

Task sources like DOM manipulation, User interaction, Networking, History traversal add code to run to the task queue, which is emptied by the event loop.

If a user clicks on the UI, then an invocation of that listener is added to the task queue.

The event loop runs continously inside the JS process. During each loop iteration, it takes one task out of the queue and executes it. The task is finished when the call stack is empty and then there is a return.
Control goes back to the event loop, which then retrieves the next task from the queue and executes it.

### Events: XMLHttpRequest





