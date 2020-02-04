### Creating Mental Models

### Primitive Values

Primitive values are numbers and strings, among other things.

```javascript
console.log(2);
console.log("Hello");
console.log(undefined);
```

Objects and Functions are also values, but they are not primitive.

```javascript
console.log({});
console.log([]);
console.log(x => x * 2);
```

Expressions are questions that JS can answer. JS answers questions in the only way it knows how- with values.

### Types of values

1. Undefined (undefined): used for unintentionally missing values.
2. Null (null) using for intentionally missing values
3. Booleans used for logical operations
4. Numbers used for math calculations
5. Strings
6. Symbols (uncommon) used to hide implementation details
7. BigInts (uncommon and new) used for math on big numbers.

Objects and Functions

8. Objects
9. Functions

Strings and arrays have some superficial similarities. An array is a sequence of items,
and a string is a sequence of characters.

```javascript
let arr = [212, 8, 506];
let str = "hello";

arr[0] = 212;
str[0] = "h";
```

You can change the array's first item.

arr[0]=240
However, You can't perform the above with a string.
str[0]='j'

All primitive values are immutable.

```javascript
let fifty = 50;
fifty.shades = "gray"; // Noooooo
```

In the JS universe, all primitive values exist in the outer circle further away from my code. This reminds me that even though I refer to them from my code, I can't change them.

### Contradictions??

```javascript
let pet = "Narwhal";
pet = "The Kraken";
```

We know that string values can't change because they are primitive. But the pet variable does change to 'The Kraken'.
Variables are not values. Variables point to values.
