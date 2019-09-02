# Problem

We have a 3 \* 3 grid with colors and our goal is to find the maximum
number of connected colors.

Some clarifying questions:
How many grids are there?

```javascript
class fibIterative {
	constructor() {
		this.stack = []; //[8]
		this.sum = 0;
	}

	fibIterative(n) {
		this.stack.push(n);
		while (this.stack.length > 0) {
			n = this.stack.pop(); //[]

			if (n == 0) this.sum += 0;
			else if (n == 1) this.sum += 1;
			else {
				this.stack.push(n - 1); //5
				this.stack.push(n - 2);
			}
		}
		return this.sum;
	}
}

s = new fibIterative();
s.fibIterative(8);
```


