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
console.log('five' * 2);

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
console.log(null || 'user');
// "user'

console.log('Agnes' || 'user');
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
	yourname = prompt('Who are you??');
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

ages.set('Boris', 39);
ages.set('Liang', 22);
ages.set('Julia', 61);
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

test('convert latin text to uppercase', () => {
	return 'hello'.toUpperCase() == 'Hello';
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
notBinary.test('11001000100110');
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

neighbor.test('neighbour');
//true
neighbor.test('neighbor');
//true
```

### Choice patterns

So we want to know whether a piece of text contains not only a number but a number followed by one of the words pig, cow, or chicken or any of their plural forms.

```javascript
let animalCount = /\b\d+ (pig|cow|chicken)s?\b/;

animalCount.test('15 pigs');
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
let match = /\d+/.exec('one two 100');

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

