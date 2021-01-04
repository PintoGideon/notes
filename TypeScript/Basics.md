### Typescript

My struggle with Typescript is real.... Let's do a deep dive.

### TypeScript vs JavaScript

Typescript is a typed superset of Js that compiles to
plain javascript

```ts
tsc --init

```

This command generates a tsconfig.json file.

```ts
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "outDir": "dist"
  }
}
```

Hovering over the properties for the `compilerOptions` object will give us the values we can provide for the keys.

The typescript compiler will compile the .ts file into a .js file and place it in the dist folder.

### Setting up Webpack for Typescript

```javascript
module.exports = {
  entry: "./src/app.ts",
  output: {
    filename: "app.js",
    path: __dirname + "./dist",
  },
  resolve: {
    extensions: [".ts", ".js"],
  },
  module: {
    rules: [{ test: /\.ts$/, use: "awesome-typescript-loader" }],
  },
  devServer: {
    port: 3000,
  },
};
```


Optional vs default vs undefined

1. Parameter is optional: x?:number
2. Parameter has a default value: x=456
3. Parameter has a union type:
x:undefined | number

### Typing Objects

1. Records
A fixed number of properties that are known at dev time.
Each prop has a different type.
```javascript
interface Point {
  x:number,
  y:number,
}

```

As of typescript's type system is concerned , method
definitions and properties whose values are functions 
are prevalent


```typescript

interface Test {
  simpleMethod(flag:boolean):void;
}

interface Test {
simpleMethod: (flag: boolean) => void;
}

interface Test {
simpleMethod(flag: boolean): void {},
};

```

Values exist at the dynamic level
Types exists at the static level

### Type Variables and Generic Types

Generic types exist at the static level , are factories
for types and have parameters representing types.

```javascript
type TypeFactory<X>=X; //definition
type MyType=TypeFactory<string>;

```

Example:

```typescript
interface ValueContainer<Value>{
  value:Value;
}


type StringContainer=ValueContainer<string>
```

Example of a Generic Class

```typescript

class SimpleStack<Elem>{
#data:Array<Elem>=[];
push(x:Elem):void{
  this.#data.push(x)
}

get length(){
  return this.#data.length
}
}


const stringStack=new SimpleStack<string>()
stringStack.push('first')
```

Type Variations for functions and methods

```typescript
function identity <Arg>(arg:Arg){
return arg;
}
const num1=identity<number>(123)
```


A more complex example:


```typescript
interface Array<T>{
  concat(...items:Array<T[] | T>):T[];
  reduce<U>(
    callback:(state:U,element:T,index:number,array:T[])=>U,
    firstState?:U
  ):U
}


```

Specifying types for plain Javascript via JSDoc comments


```typescript
/**
@param {number} x
@param {number} y
@returns {number} 
*/

function add(x,y){
  return x+y;
}
/** @typedef {{ prop1: string, prop2: string, prop3?: number }} SpecialType */
/** @typedef {(data: string, index?: number) => boolean} Predicate */

```


### Top Types



The top type is the universal type , sometimes called
the universal supertype as all other types in any given
type system are subtypes.

`any` and `unknown` are sets that contain all values.
Typescript has the `bottom` type never, which is the
empty set.

Every type is assignable to type any:

```typescript

let storageLocation:any;

storageLocation=null;
storageLocation=true;
storageLocation={}

```



Where 'any` allows us to do anything, `unknown` is much 
more restrictive.

```javascript

function test(value:unknown){
  value.length; //@ts-expect-error: Object is of type 'unknown'
  if(typeof value==='string'){
    value;
    value.length; //Ok
  }
}

```

### TypeScript enums

```typescript
enum NoYes{
  No=0,
  Yes=1
}

NoYes.No //0
NoYes.Yes // 1


```
The entries No and Yes are called members of the enum.
The enum member has a name and value. THe part of a member definition starts with an equals sign and specifies a values is called an initializer.

### Typing Objects

Mapped Types

```typescript

interface Point{
  x:number;
  y:number
}

type PointCopy={
  [Key in keyof Point]:Point[Key]; //A
}

```

Definining Key Value data structures

```typescript

interface PhoneNumber{
  [number:string]:undefined | {
    areaCode:number;
    num:number;
  }
}

const d:PhoneNumber={}

if(typof d.abc==='object'){
  d.abc //Works
}


const phoneDict:PhoneNumber={
  office:{areaCode:617,num:10202020202}
}

```

### Initializing Instance Properties in class


```typescript
class Point{
  x:number;
  y:number

  constructor(x:number, y:number){
    this.x=x;
    this.y=y;
  }
}


```

Classes work similarly to what we're used to seeing in TS.

```typescript
export class Contact implements HasEmail{
  email:string,
  password:string

  constructor(email:string,password:string){
    this.email=email;
    this.password=password;
  }
}


```

An equivalent setup to the above code is

```typescript
class ParamPropContact implements HasEmail{
  constructor(
    public name:string,
    public email:string='no email'
  ){
    // Nothing needed
  }
}

const x = new ParamPropContact('a','b')
x.email //a

```


### Inferring Types of Arrays is difficult

Due to the two roles of Arrays, it is difficult for Typescript to always guess the right type.



```javascript

type Fields = Array<[string, string, boolean]>;
//or
type Fields = [
Array<string|boolean>,
Array<string|boolean>,
Array<string|boolean>,
];

const fields:Fields=[
  ['first','string',true],
  ['last','string',true],
  ['age','number',false]
]

```


### Function Types

```typescript

interface ContactMessenger{
  (test:string):void
}

```

### Generics


Generics allows us to parametize types in the same way
that functions parameterize values


```typescript
interface WrappedValue<X>{
  value:X;
}


let value:WrappedValue<string>={value:''}

```

Type parameters can have default types just like function parameters can have default values.


```typescript

interface FilterFunction<T=any>{
  (value:T):boolean
}

const stringFilter:FilterFunction<string>=value=>typeof value==='string'

```

You don't have to use exactly your type parameter as
an arg - things are the based on your type parameter
are fine too.

```typescript
function resolveOrTimeout<T>(
  promise:Promise<T>,
  timeout:number
){

return new Promise<T>((resolve,reject))=>{
const task=setTimeout(()=>reject("Time up"),timeout)

promise.then(val=>{
  clearTimeout(task);
  resolve(val);
})

}}

resolveOrTimeout(fetch(""),1000)


```

Type parameters can also have constraints.


```typescript

function arrayToDict<T extends {id:string} (array:T[]){
const out :{
  [key:string]:T
}={}

array.forEach(val=> out[val.id]=val)

return out;
}

arrayToDict([
  {id:'1',firstname:"gidi",lastname:"huang"},
  {
    id:'2',firstname:'jon',lastname:'lawson'}
  }
])
```

Type Parameters are assosciated with scopes

```typescript

function startTupe<T>(a:T){
return function finishTupe<U>(b:U){
  return [a,b] as [T,U]
}
}

```

When to use Generics???

- Generics are necessary when we want to describe a relationship between
two or more types


