```javascript
const ages = [21, 18, 42, 40, 64, 63, 34];

const maxAge = ages.reduce((max, age) => {
  if (age > max) {
    return age;
  } else {
    return max;
  }
}, 0);

const maxAge = ages.reduce((max, value) => (value > max ? value : max), 0);
```

```javascript
const colors = ["red", "blue", "green", "blue", "green"];

const distinctColors = colors.reduce(
  (distinct, color) =>
    distinct.indexOf(color) !== -1 ? distinct : [...distinct, color],
  []
);
```


### Higher order functions

Currying is a functional technique that involves the use of higher-order functions. Currying is the practice of holding on to some of the values needed to complete an operation until the rest can be supplied at a later point in time.  This is achieved through the use of a function that returns another function , the curried function.


