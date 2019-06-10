### Higher order function

A function which returns an other function.
Why are they useful though?

### Abstraction

Abstractions hide details and give us the ability to talk about problem at a higher
level.

Higher order functions allow us to abstract
over actions, not just value. They come
in several forms. For example, we can have
functions that create new functions.

```javascript
function greaterThan(n) {
	return m => m > n;
}

let greaterThan10 = greaterThan(10);
console.log(greaterThan10(11));
//True
```

**_We can have functions that change other functions_**

```javascript
function noisy(f) {
	return (...args) => {
		console.log('calling with', args);
		let result = f(...args);
		console.log('called with', args, ', returned', result);
		return result;
	};
}
noisy(Math.min)(3, 2, 1);
// → calling with [3, 2, 1]
// → called with [3, 2, 1] , returned 1
```

### Transforming with Map

The map method transforms an array by applying a function to all the elements
and building a new array from the
returned values. The new array will have
the same length as the input array. but its
contents are mapped to a new form by the function.

```javascript

function map(array, transform){
let mapped=[];

for(element of array){
    mapped.push(transform(element))
}

return mapped;

}

map([1,2,3,4], s=>s.name));
```

### Reduce function

A common thing to do with array is to compute
a single value from all of them. One recurring example, summing a collection of numbers is an instance of this.

```javascript
function reduce(array, combine, start) {
	let current = start;
	for (let element of array) {
		current = combine(current, element);
	}
	return current;
}

console.log(reduce([1, 2, 3, 4], (a, b) => a + b, 0));
// → 10
```

### Composability

Higher order function start to shine when
you need to compose operations.

Suppose we have a data set and we want to
find the average year of origin for alive
and dead scripts in the data set.

```javascript
function average(array) {
	return array.reduce((a, b) => a + b);
}

Math.round(average(scripts.filter(s => s.living).map(s => s.year)));

//1188
```
