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


