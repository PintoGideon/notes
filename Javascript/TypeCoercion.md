### Global Execution Context:

Default:

--------------

Phase: Creation
- window : global object
- this : window


```javascript
var name='Tyler'
var handle='@tylermcginnis'

function getUser(){
return{
name : name,
handle : handle
}

}

```

### Global Execution Context

1st Step:

-------------

Phase : Creation
- window : global object
- this : window
- name : undefined
- handle : undefined
- getUser : fn()


2nd Step:

----------------
Phase: Execution
- window : global object
- this : window
- name : 'gideon'
- handle : 'gpinto'
- getUser : fn()

![functions](https://user-images.githubusercontent.com/15992276/58756602-46b5a080-84eb-11e9-92cb-eda25c29ccfd.JPG)



# Hoisting

Hoisting is a language convention to discuss the idea of lexical
scope. 

```javascript
function teacher(){
return "Gideon"
}

var otherTeacher;
teacher(); //"Gideon"

otherTeacher(); //Type Error

otherTeacher=function(){
return "Mahesh"
}

```
***Let doesn't hoist?***

There is a difference in how let and const hoist.  let is hoisted in it's block scope but don't get initialized to undefined.


# Type Coercion

Whenever there's a divergence between what your brain thinks 
is happening and what the computer does, that's where bugs enter
the code.

Fundamental Pillars of JavaScript
- Types
- Scope
- Objects 

### Coercion

***Primitive Datatypes***

- undefined
- string
- number
- boolean
- object
- symbol

### typeof operator

```javascript
var v;

typeof v; //undefined

v="1"
typeof v; //string

v=2
typeof v //number

v=true;
typeof v  //boolean

v={}
typeof v   //object

v=Symbol();
typeof v;  //symbol

typeof doesntExist; //undefined

var v=null;
typeof v;  // object  OOPS!!

v=function(){
}
typeof v   // function

```

### undefined vs undeclared

***undefined*** means that there is a variable declared and at the moment it has no value

***undeclared*** means it's never been created in any scope we have
access to.

### NaN 

Essentially NaN means a special sentinel value that indicates
a nonnumeric value.

```javascript
var myAge=Number('0o46') //38
var myNextAge=Number('39) //39
var myCatsAge=Number("n/a")  //NaN
myAge="my son's age";  //NaN

```
### isNaN

```javascript
isNaN(myAge);  //false
isNaN(myCatsAge) //true
isNaN('my sons age') //true


```

The third example in the above code return true for a string input.
The isNaN function is going to coerce the input to a Number
and return true if it's a NaN value.

The ES6 specification gives us the correct result for
the input of the type NaN.

NaN: Invalid Number

```javascript
typeof NaN
number
```

```javascript

Number.isNaN(myCatsAge)    //true
Number.isNaN("My son's age")  //fase

```

### Negative Zero
The negative representation of zero. 

```javascript

var trendRate=-0;
trendRate===-0 //true

trendRate.toString();  // "0" OOPS!
trendRate===0;   //"True"  OOPS!
trendRate<0; //false
trendRate>0; //false

Object.is(trendRate,-0); //true
Object.is(trendRate,0);  //false;

```
***Application***
```javascript
function formatTrend(trendRate){
var direction=(trendRate<0 || Object.is(trendRate,-0)) ? 
"down":"up";
return `$(direction) $(Math.abs(trendRate)"`;

}

formatTrend(-3)   //"down 3"
format(-0) // "down 0"
```

### Fundamental Objects

- Built-in Objects
- Native Functions

**Use new:**
- Object()
- Array()
- Function()
- Date()
- RegExp()
- Error()

**Don's use new:**
- String()
- Number()
- Boolean()


```javascript
var yesterday=new Date("March 6, 2019");
yesterday.toUTCString();


var myGPA=String(transcript.gpa);
```

# Abstract Operations

- ToPrimitive(hint)
- ToString- It takes any value and gives us a string representation

```javascript
[]===>""
[1,2,3]===>"1,2,3"
[null, undefined]=","
[[],[],[]]="..."

```

### ToPrimitive(hint)


hint "number"                
number.valueOf()             
number.toString()

-------------------

hint:"string" 
string.toString()
string.valueof()

-------------------

### ToString
The ToString abstract takes any value and gives us a string representation
of the value.

If we call ToString on an object, it's going to invoke the ToPrimitive with string
as a hint.

Example:
```
 [] gives you "",
 [1,2,3] gives "1,2,3"
 
```
### ToNumber

If we don't have a number but need to perform a numeric operation, we can use ToNumber as the abstract
operation.

Examples:

"" when coerced into a number gives me 0.

 
### ToBoolean

Falsy:

-0,-0,
null,
NaN,
false,
undefined

------------

Truthy:

"foo",
23,
{a:1},
[1,3],
true,
function(){...}


--------------

### Case of Coercion

```javascript
var numStudents=16;
console.log(`There are ${numStudents} of students`);
```

If either of them is a string, then the '+' operator prefers
a string concatenation

```javascript
function addAStudent(numStudents){
return numStudents+1
}

addAStudent(
+studentsInputElem.value
);

```

The '+' operator invokes the ToNumber abstract operation. 

***Falsy && Truthy***

```javascript
if(studentsInputElem.value)
{
numStudents=Number(studentsInputElem.value);
}

while(newStudents.length){
enrollStudent(newStudents.pop());
}

```


# Boxing

It is a form of implicit coercion. Javascript coerces the primitive string value into an object upon which the .length method can be called on.

```javascript
studentNameElem.value.length

```

### Corner Case for Type Conversion

The Root of all (Coercion) Evil
```javascript
studentInput.value="";
Number(studentInput.value) //0
```

```javascript
studentInput.value=" \t\n"
Number(studentInput.value)  //0
```

### Double Equals Algorithm

This algorithm prefers to do numeric coercion. It prefers
to reduce everything down to number for comparison.

```javascript
var workshopEnrollment=16;
var workshopEnrollment2=worshopElem.value

if(Number(workshopEnrollment)==Number(workshopEnrollment2)
{
//
}
```
***Summary of the algorithm***

```javascript
If the Types are the same: ===
If null or undefined: equal
If non-primitives:ToPrimitive
Prefer:ToNumber
```




























