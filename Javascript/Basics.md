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

flatten([[1, 3, 4], [2, 3]]);
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

### Backtracking

The regular expression /\b([01]+b|[\da-f]+h)\d+)\b/ matches a binary number followed by a b , a hexadecimal number followed by a h, or a regular decimal number with no suffix character.

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
    return request(nest, neighbor, "ping").then(() => true, () => false);
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

Asynchronous Programs are executed piece by piece. Each pience may start some actions and schedule code to be  executed when the action finishes or fails. In between these pieces, the program sits idle waiting for the next action.

If I call setTimeout from within a function, that function will have returned by the time the callback function is called. And when the callback returns , control does not go back to the function that scheduled it.


As events come in, there are added to a queue and their code is executed one after the other. 


### Parsing

The most immediate visible part of a programming language is it's syntax or notation.

A parser is a program that reads a piece of text and produces a data structure that reflects the structure of the program contained at that text.

If the text does not form a valid program, the parser should point out the error. Our language will have a simple and uniform syntax.
