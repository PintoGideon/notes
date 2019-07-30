### Book Summary

The ability to visualize the consequences of the actions under consideration is cruicial to becoming an expert programmer.

### Recursive Process

```javascript
function factorial(n) {
	if (n == 1) return 1;
	else n * factorial(n - 1);
}

factorial(6);
```

### Taking a diffrent perspective on calculating factorial (Iterative Process).

To calculate the n!, we observe that
we multiply 1 by 2, then the result by 3, then by 4 till we reach n!.

We maintain a running product with a counter that counts from 1 upto n.

```javascript
product < -counter * product;
counter < -counter + 1;
```

```javascript
function factorial(n) {
	var product,
		counter = 1;
	if (counter < n) {
		product *= counter;
		counter + 1;
	}
	return product;
}
```

### IIFE

### Modularity

```javascript

var api=(function(){

var local=0;

var publicInterface={
counter:function(){
return ++local;
}

}

return publicInterface;
}
```

### Promises

```javascript
const promise = new Promise(function(fulfill, reject) {
	if (xhr.status >= 200 && xhr.status < 300) {
		fulfill(xhr.response);
	} else {
		reject(new Error(xhr.responseText));
	}
});
```

### Events from scratch

```javascript
function emitter(thing) {
	var events = {};

	if (!thing) {
		thing = {};
	}

	thing.on = function on(type, listener) {
		if (!events[type]) {
			events[type] = [listener];
		} else {
			events[type].push(listener);
		}
	};

	return thing;
}

```


### Testing JS Apps Crash Course


