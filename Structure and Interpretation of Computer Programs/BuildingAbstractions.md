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

