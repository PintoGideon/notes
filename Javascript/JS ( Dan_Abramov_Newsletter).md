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
console.log((x) => x * 2);
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

### Undefined

Undefined in JS represents the concept of an unintentionally missing value. If you forgot to assign a value to a variable it will point to undefined.

```javascript
let bandersnatch;
console.log(bandersnatch); //undefined
```

If you read a variable that was actually not defined, you will get an error.

```javascript
console.log(jabberwocky); //Reference error
```

### Null

Null behaves very similarly.

Due to a bug in JS, null pretends to be an object.

In practice, null is used for intentionally missing values. Null is used for intentionally missing values.

### NaN

NaN is a numeric value, it is not null, undefined, a string, or some other type. But in the floating point math, the name for that term is "not a number".

```javascript
console.log(typeof NaN); //number
```

```javascript
let isSad = false;
let isHappy = !isSad; // isHappy=true
let isFeeling = isSad || isHappy; // isFeeling=true
let isConfusing = isSad && isHappy; // false
isSad = true; //isSad=true
```

### Objects

```javascript
let sherlock = {
  surname: "Holmes",
  address: { city: "London" },
};

let john = {
  surname: "Watson",
  address: sherlock.address,
};

john.surname = "Lennon";
john.addressCity = "Malibu";
```

Here john.address.city is "Malibu" and sherlock.address.city is also "Malibu".

Importantly, properties don't contain values--they point at them! It turns out the js universe is full of wires.
