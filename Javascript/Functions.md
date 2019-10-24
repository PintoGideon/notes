### Function declaration

```javascript

functio square(x){
	return x*x
}
```

This is a function declaration.
The statement defines the binding square and points it at the given function.

**_There is one subtelty with this form of function definition_**

```javascript
console.log('The future says', future());

function future() {
	return "You'll never have flying cars";
}
```

The above code works even though the function is defined below the code that uses it.

Function declarations are not part of the regular top to bottom flow of control. They are conceptually moved to the top of the scope and can be used by all the code in that scope.

This is sometimes useful because it offers the freedom to order code in a way that seems meaningful

Because a function has to jump back to the place that called it when it returns, the computer must remember the context from which the call happened.

The place where the computer stores the context is the call stack.

### Closure

```javascript
function multiplier() {
	return number => number * factor;
}

let twice = multiplier(2);
console.log(twice(5));
//10
```

Thinking about program like this takes some practice. A good mental model is to think of function values as containing both the code in their body and the environment in which they are created.

The following code summarizes the concepts convered above.

Question:
by starting from the number 1 and repeatedly either adding 5 or multiplying by 3, an infinite set of numbers can be produced. How would you write a function that, given a number, tries to find a sequence of such additions and multiplications that produces that number? For example, the number 13 could be reached by first multiplying by 3 and then adding 5 twice, whereas the number 15 cannot be reached at all

```javascript
function findSolution(target) {
	function find(current, history) {
		if (current == target) return history;

		else if(current<target>) return null
		else{
			return find(current+5,`${history}+5`) || find (current+3, `${history}+3`)
		}
	}
	find(1,"1")
}

findSolution(24)
```

### Data Structures: Objects and Arrays

Both string and array objects contain in addition to the length property, a number of properties that hold function values.

```javascript
let anObject = { left: 1, right: 2 };

delete anObject.left;
//undefined

console.log('left' in anObject);
//false
```

The binary in operator when applied to a string or an object tells you whether that object has a property with that name.

To fnd out that properties an object has, you can use the Object.keys.

There is an Object.assign function that copies all properties from one object into another.

```javascript
let objectA = { a: 1, b: 2 };
Object.assign(objectA, {
	b: 3,
	c: 4
});

// {a;1,b:3,c:4}
```

### A for-of-loop

```javascript
for (let i = 0; i < JOURNAL.length; i++) {
	let entry = Journal[i];
}

// for-of=loop

for (let entry of JOURNAL) {
	console.log(`${entry.events.length}events`);
}
```

### Important Array methods

```javascript
let todoList = [];

function remember(task) {
	todoList.push(task);
}

function getTask(task) {
	return todoList.shift();
	//get the first task
}

function rememberUrgently() {
	todoList.unshift(task);
}
```

To search for a specific value, provide an indexOf method. The method searches through the array from the start to the end and returns the index of which the requested value was found.

```javascript
console.log([1, 2, 3, 4].indexOf(2));
```

Another fundamental method is slice that the start and end indices and returns an array that has only the elements between them. The start index is inclusive, the end index is exclusive.

The concat method can be used to glue arrays together to create a new array which is similar to what the + operator does for strings.

```javascript
function remove(array, index) {
	return array.slice(0, index).concat(array.slice(index + 1));
}

console.log(remove(['a', 'b', 'c', 'd', 'e'], 2));
```

Values of types string, number and Boolean are not objects and the language doesn't complain if you try to set new properties on them. Such values are immutable and cannot be change.

```javascript
let kim = 'kim';

kim.age = 88;
console.log(kim.age);
//undefined
```

But these types have built in properties.

```javascript
console.log('coconuts', slice(4, 7));

console.log('coconut', indexof('u'));
//5
```

**_One difference between the string's indexof can search for a string containing more than one character, whereas the corresponding array method looks only for a single element._**



The padStart function takes the desired length and padds charcters as arguments

```javascript
console.log(String(6).padStart(3, '0'));
```
