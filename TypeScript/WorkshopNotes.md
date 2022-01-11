### Typescript Patterns

```typescript
{
    "compilerOptions":{
        "outputDir":"dist", //where to put TS files
        "target":"ES2017", // which level of JS support to target
        "module":"CommonJS"
    },
    "include":["src"] //which files to compile
}
```

`const` variable declarations cannot be reassigned. Typescript makes a assumption and infers the type of age as a literal type which is '6'.

```javascript
const age = 6;
```

### Implicit any

```javascript
let endTime; // endTime:any

setTimeout(() => {
  endTime = 0;
  endTime = new Date();
}, 100);
```

### Function Arguments and Return values

```javascript
const result = add(3, "4");

// Without type anotations, "anything goes" for the arguments passed into add.
```

```javascript
function add(a: number, b: number): number {
  return null;
}

// Great way for code authors to state their intentions up front.
```

`any` is a wildcard which can accept anything but it can also present itself as anything. That is a liability.

```javascript
const result = add(3, "4");
const p = new Promise(result); //const result:any
```

When any is passed into well typed code, that flexibility will compromise
well typed code.

### Objects

What do we when you need a property which is present sometimes.

```typescript
let car: {
  make: string;
  chargeVoltage?: number;
} = {
  make: "Toyota",
  chargeVoltage: 3,
};

let str:string='';

if(typeof of car.chargeVoltage!==undefined){
    str+=`${car.chargeVoltage}`
}
console.log("String",str);
```

### Dictionaries

```javascript
const phones: {
  [k: string]: {
    country: string,
    area: string,
    number: string,
  },
} = {
  fax: {
    country: "India",
    area: "Mumbai",
    number: "401803",
  },
};
```

### Array Types

```javascript
let myCar: [number, string, string] = [2002, "Toyota", "Corolla"];
```

### Structural vs Nominal Types

Type-checking can be thought of as a task that attempts to evaluate the question
of compatability

### Static vs Dynamic

Typescript's type system is static.

### Type Guards

```typescript
const [first, second] = outcome;

//const second: Error | {name:string, email:string}

if (second instanceof Error) {
} else {
}
```

### Interfaces

An interface is way of defining an object type.

```javascript
interface AnimalLike {
  eat(food): void;
}

class Dog implements AnimalLike {
  bark() {
    return "woof";
  }
  //Class 'Dog' incorrectly implements interface 'AnimalLike'
}
```

### `this` types

### Typescript Fundamentals

```typescript
class Car {
  make: string;
  model: string;
  year: number;
  constructor(make: string, model: string, year: number) {
    this.make = make;
    this.model = model;
    this.year = year;
  }
}

//Typescript provides a more concise syntax for this through the use
//of of param properties

class Car {
  constructor(public make: string, public model: string, public year: number) {}
}
```

### Nullish Values

`undefined` means the value isn't available yet.
`void` should exclusively be used to describe that a function's return value
should be ignored

```javascript
type GroceryCart = {
  fruits?: { name: string, qty: number }[],
  vegetables?: { name: string }[],
};

const cart:GroceryCart={};

cart.fruits!.push({name:"kumkat"});


```

### Generics

```javascript
function listToDict<T>(
  list: T[],
  idGen: (arg: T) => string
): {
  [k: string]: T,
} {
  const dict: { [k: string]: T } = {};
  return dict;
}
```

Typescript will infer what the type of T is on a per-usage basis, depending on what kind of array we pass in.

```javascript
function mapDict<T, U>(input: Dict<T>, mappingCb: (arg: T) => U) {}
```

```javascript
function filterDict<T>(input: Dict<T>, filterCb: (arg: T) => boolean): Dict<T> {
  const toReturn: Dict<T> = {};
  for (let key in input) {
    const thisVal = input[key];
    if (filterCb(thisVal)) {
      toReturn[key] = thisVal;
    }
  }
  return toReturn;
}
```

### Scopes and Constraints

Minimum Requirements on type parameters.

```javascript
function listToDict<T exends HasId>(list:T[]):Dict<T>{

}

// I can be given any T , but it has to fulfill a base requirement

```

### Intermediate Typescript

Modules & CJS interop.

### Importing non-TS things.

```javascript
import img from "./file.png";
```

`file.png` is obviously not a Typescript file.

Treat .png file extensions as a js module. Create a file global.d.ts,

```javascript
declare module '*.png' {
    const imgUrl:string
    export default imgUrl
}

import img from './file-png';
```

This is purely type info that will "compile away" as part of our build process.

### Type Queries

Type Queries allow us to obtain type information from values, which is important
particularly while working with libraries that may not expose type info in a way
that's most useful.

1. keyof

The keyof type query allows us to obtain type representing all property keys
on a given interface.

```javascript
type DatePropertyNames = keyof Date
```

2. typeof

```javascript
function main(){
    const apiResponse = await Promise.all([
        fetch('https://example.com'),
        Promise.resolve("Titanium white");
    ])
type ApiResponseType = typeof apiResponse //[Response, string]
}

```

### Conditional Types

```javascript

```

### Extract and Exclude

Extracting the subset of FavoriteColors that is assignable to string.
Exclude would be the subset of FavoriteColors not assignable to string.

```typescript
type FavoriteColors =
  | "dark blue"
  | "green"
  | "yellow"
  | [number, number]
  | { red: number; green: number; blue: number };

type StringColors = Extract<FavoriteColors, string>;
// type StringColors= 'dark blue' | 'green' | 'yellow'
```

### Extract and Exclude

```typescript
/**
 * Exclude from T those types that are assignable to U
*/
type Exclude<T, U> = T extends U ? never : T;
/**
 * Extract from T those types that are assignable to U
 */
type Extract<T,U> = T extends U > T : never
```

### infer keyword

```typescript
class WebpackCompiler {
  constructor(
    entry?: string,
    watch?: boolean,
    options: { amd?: false; bail?: boolean }
  ) {}
}

const config = {
  entry: "src/index.ts",
  wutch: true, //typo
};

try {
  const compiler = new WebpackCompiler(config);
} catch (error) {
  console.error(error);
}
```

We want typescript to help us in the above example , which is to catch the typo
in `watch`. IF we change our code to

```typescript
const config: WebpackComilerOptions;
```

we will have a more complete validation for the object.

```typescript
class FruitSalad {
  constructor(fruitNames: string[]) {}
}

type ConstructorArg<C> = C extends {
  new (arg: infer A, ...args: any[]): any;
}
  ? A
  : never;
```

### Index Types

```javascript
interface Car {
  color: {
    red: string,
  };
}

let carColor: Car["color"]["red"] = "red";
```

### The Basics

```javascript
type Fruit = {
  name: string,
  color: string,
  mass: number,
};

type Dict<T> = { [k: string]: T };

const fruitCatalog: Dict<Fruit> = {};
```

```javascript
type Fruit = {
  name: string,
  color: string,
  mass: number,
};
```

### Record

```javascript
type Fruit = {
  name: string,
  color: string,
  mass: number,
};

type MyRecord={[FruitKey in 'apple' | 'cherry']:Fruit}

```

If we make our type a bit more generalized with some type params instead of
hardcoding Fruit and "apple" | "cherry", we arrive at a built-in type.

```typescript
type Record<KeyType, ValueType> = { [key in KeyType]: ValueType };
```

```typescript
type PickWindowProperties<Keys extends keyof Window> = {
  [Key in Keys]: Window[Key];
};

type PartOfWindow = PickWindowProperties<
  "document" | "navigatior" | "setTimeout"
>;

type PartOfWindow = {
  document: Document;
  navigator: Navigator;
};
```

Records are more specific than dictionaries

```typescript
let d ={
  [k:string]:Date
}={}

let record: {[k in 'endOfweek'|'startOfweek']:Date}={
  endOfWeek:new Date(),
  startOfWeek:new Date()
}


```

### Utility type - Pick

```typescript
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

### Mapping Modifiers

Make all properties in T optional

```typescript
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

Make all properties in T required

```typescript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```


