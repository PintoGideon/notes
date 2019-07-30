### Pure functions must not mutate external state

```js
const originalCart = {
	items: []
};

const addToCart = (cart, item, quantity) => {
	cart.items.push({
		item,
		quanitity
	});
	return cart;
};

const newCart = addToCart(
	originalCart,
	{
		name: 'Digital SLR Camera',
		price: '1495'
	},
	1
);
```

The problem with this is that we’ve just mutated some shared state. Other functions may be relying on that cart object state to be what it was before the function was called, and now that we’ve mutated that shared state, we have to worry about what impact it will have on the program logic if we change the order in which functions have been called.

### Alternate Approach

```js
const addToCart = () => {
	return {
		...cart,
		items: cart.items.concat([
			{
				item,
				quantity
			}
		])
	};
};
```

### Shared State

Shared state is any variable, object or memory space that exists in shared scope, or as the property of an object
being passed between scopes.

For example, a computer game might have a master game object, with characters and game items stored as properties
owned by the object. **_Functional programming avoids shared state-instead, relying on immutable data structures
and pure calculations to derive new data from existing data_**

### With Pure funcions, given the same input, you'll always get the same output.

### Functors, Containers, Lists and Streams

A functor is a data structure that can be mapped over.

```javascript
const double = number => number * 2;

const mapDouble = numbers => numbers.map(double);
mapDouble([2, 3, 4]);
```

### Declarative vs Imperative

1. Imperative(How to do things)
2. Declarative (What to do)

```javascript
const myReducer = (state = {}, action = {}) => {
	const { type, payload } = action;

	switch (type) {
		case 'FOO':
			return Object.assign({}, state, payload);
		default:
			return state;
	}
};
```

### Rest and Spread

A common feature of functions in JS is the ability to gather together a group of remaining arguments in the function signature using the rest operator.

```javascript
const aTail = (head, ...tail) => tail;
aTail(1, 2, 4);
```

### Currying

```javascript
function gte(cutoff) {
	return function(n) {
		return n >= cutoff;
	};

	const gte4 = gte(4);
	gte4(3); //false
	gte4(6); //true
}
```

### Higher order functions

Higher order function takes a function as an argument or returns a function. Higher order functions is in contrast to first order functions which don't take a function as an argument or return a function as output.

Higher order functions are also commonly use to abstract how to operate on different data types. For instance,
.filter() doesn't have to operate on an array of strings.

```javascript
const highpass = cutoff => n => n >= cutoff;

const gte = highpass(3);
[1, 2, 3, 4].filter(gte3);
```

### Abstraction

Abstraction is about removing things.

1. Generalization: Finding similarities in repeated patterns and hiding the similarities behind an abstraction.

2. Specialization: Supplying only what is different for
   each use case.

### Doing more with less code

```javascript
const add = (a, b) => a + b;

//Currying the add function

function add(a) {
	return function(b) {
		return a + b;
	};
}

const inc = add(1);
const a = inc(1); //2
const b = inc(a); //3
const c = inc(b); //4
```

### Constructing the map function

```javascript
function map(fn) {
	return function(arr) {
		arr.map(fn);
	};
}

const f = n => n * 2;

const doubleAll = map(f);
const doubled=doubleAll([1,2,4])
```

### Abstractions that are already available to you

Reduce let's you iterate over a list, applying a function
to an accumulated value and the next item in the list, until the iteration is complete and accumulated value
gets returned.





