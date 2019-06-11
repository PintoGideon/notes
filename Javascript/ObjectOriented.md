### Object Oriented Javascript

```javascript
function userCreator(name, score) {
	let newUser = {};
	newUser.name = name;
	newUser.score = score;

	newUser.increment = function() {
		newUser.score++;
	};

	return newUser;
}

let user1 = userCreator('Will', 3);
let user2 = userCreator('Gidi', 4);
user1.increment();
```

The increment function has a common behavior for both objects. This code is not scalable to 10000
users as we are making copies of the function for each user.

------------------------------------------------------------

### __proto__

**_Mental Model_**
Store the increment function in just one object and have the interpreter,
if it doesn't find the function on user1, look up to that object if it's
there.

How to make this link?

```javascript
function userCreator(name, score) {
	let newUser = Object.create(userFunctionStore);
	newUser.name = name;
	newUser.score = score;
	return newUser;
}

let userFunctionStore = {
	increment: function() {
		this.score++;
	},
	login: function() {
		console.log('You are logged in');
	}
};

let user1 = userCreator('Will', 3);
let user3 = userCreator('Tim', 4);
user1.increment();
```

```
Object.create(userFunctionStore)

```

The above snippet essentially creates an empty object with
a bond to the userFunctionStore. This is how the object returned from the function looks.

```javascript
{
name:'Gideon',
score:4,
__proto__:userFunctionStore
}
```

The `__proto__` is a reference to the userFunctionStore in which the increment function is defined.

-----------------------------------------------------------------------------------------------------


### new operator

In JavaScript, a function is an object first.

```javascript
userCreator={
call:
}
```

userCreator() actually is userCreator.call under the hood.

**_When we call the constructor function with new in front, we automate 2 things_**

1. Create a new user object
2. return the new user object

```javascript
function User(name, score) {
	this.name = name;
	this.score = score;
}

User.prototype.increment = function() {
	console.log('login');
};

let user1 = new User('Eva', 3);
user1.increment();
```

User is an object and a function. It has a property on it called prototype to which we add an increment
function.
It's going to look something like this

```javascript

User={
prototype:{
increment:function(){},
login:function(){}
}
}

```

The function User returns out is an object which looks
something like this.

```javascript
this:{
name:'Eva',
score:3,
__proto__:User.prototype
}

```

### Arrow Functions

```javascript
function UserCreator(name, score) {
	this.name = name;
	this.score = score;
}

UserCreator.prototype.increment = function() {
	function add1() {
		this.score++;
	}
	add1();
};

var user1 = new UserCreator('Gidi', 10);
user1.increment();
```

The returned object from the UserCreator function looks like
this `{name:'Gidi', score:10, __proto__:User.prototype}`

When the increment function is called, the this inside the function is
set to user1. However, the this inside function add1 is set to the window
object. The arrow function has it's this assignment lexically bound.

Now we can do the following the above code

```javascript
UserCreator.prototype.increment = function() {
	const add = () => {
		this.score++;
	};
	add();
};
```
***Arrow functions bind 'this' lexically***

----------------------------------------------------------------------------------------------

### Class keyword

Javascript masks the prototypal nature of Javascript
with the class keyword.

```javascript
class UserCreator {
	constructor(name, score) {
		this.name = name;
		this.score = score;
	}

	increment() {
		this.score++;
	}

	login() {
		console.log('Login');
	}
}

const user1 = new UserCreator('Gidi', 10);
user1.increment();
```

Javascript uses the **_**proto**_** link to give objects, functions
and array a bunch of bonus functionality. All objects by
default have **_**proto**_**

- With Object.create, we overrule the default `__proto__`
  reference to Object.prototype and replace it with functionStore.
  = But functionStore is an object so it has a `__proto__`
  reference to Object.prototype. We just intercede in the chain.

```javascript
const obj = {
	num: 3
};

obj.num; // 3
obj.hasOwnProperty('num');
```

The object method looks like this:

```javascript
obj={
num:3
__proto__:Object.prototype
}
```

Object is a function as well as an object. Object has a prototype property on it
and looks like this.

```javascript
Object = {
	prototype: {
		hasOwnProperty: function() {
			//  inside function body
		}
	}
};
```

### Array.prototype and Function.prototype

```javascript
function multiplyBy2(num) {
	return num * 2;
}

multiplyBy2.toString();
multiplyBy2.hasOwnProperty('score');
```

The toString method does not exist in the function
definition of multiplyBy2. The object type of multiplyBy2
looks like this.

```javascript
{
	__proto__: Function.prototype;
}
```

The Function keyword is similary to Object. They are both created by the JavaScript
Engine.
This is how the object type of function looks like.

```javascript

Function={
prototype:{
toString: function(){},
call:function(){},
apply:function(){},
bind:function(){}
__proto__:Object.prototype
}
}

```

This is how the Object's object type looks like

```javascript
Object = {
	prototype: {
		hasOwnProperty: function() {}
	}
};
```

**_If we look for a method which is not present in both
the Function and the Object, the it throws an error. The **proto** on the Object's object points
to null_**

### Subclassing

**_Mental Model_**

Suppose we have a user with name and score properties. We want another user to have these properties with extended features.

```javascript
function userCreator(name,score)
{
const newUser=Object.create(userFunctionStore);
newUser.name=name;
newUser.score=score;

return newUser;
}

const userFunctionStore={
sayName: function(){console.log(this.name)),
increment:function(){this.score++}
}

const user1=userCreator('Gidi',10);
user1.sayName() //Gideon
```

The userCreator returns an object which looks like this:

```javascript
{
name:'Gideon',
score:10,
__proto__:userFunctionStore
}
```

**_Now I want to create an user with an extended feature_**

```javascript
function paidUserCreator(paidUserName,paidUserScore, monthlyFees)
{
const paidUser=userCreator(paidUserName,paidUserScore);
Object.setPrototypeOf(paidUser, paidUserFunction);
paidUser.monthlyFees=monthlyFees;
return paidUser;
}

const paidUserfunction={
displayFees: function(){console.log(this.monthlyFees));
}

Object.setPrototypeOf(paidUserFunction, userFunctionStore);

const subscribedUser=paidUserCreator('Gideon',10,500);
subscribedUser.displayFees() //500
subsribedUser.sayName()  // 'Gideon'

```

### Factory function approach

userCreator when called is going to return an object of the shape shown below.

```javascript
{
name:'Gideon',
score:8,
__proto__:userFunctionStore
}
```
We do not want the ```__proto__``` to refer to  userFunctionStore for a lookup if a property is not present in the paidUserCreator.
We want the paidUser function to be referenced instead.


```javascript
Object.setPrototypeOf(paidUser, paidUserFunctions);
```

The object returned by the paidUserCreator has the shape shown below.

```javascript
{
name:'Gideon',
score:8,
monthlyFees:500,
__proto__:paidUserFunctions
}

```

### Prototypal lookup

```javascript
paidUser.displayFees();

// paidUser does not find displayFees defined as a method.
// Look's in proto

// It finds a reference to paidUserFunction which has the displayFees method defined.
```

```javascript
paidUser.sayName();

//Method's not defined in the object body
//Look in __proto__
//Does not find it
//Look in the __proto__ of the referenced object
//Finds it
```

> The problem with object-oriented languages is they've got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle - John Armstrong (Creator of Erlang)

### Call and Apply approach

```javascript
const obj={
num:3,
increment:function(){this.name++);
}

const otherObj={
num:10
}

obj.increment();

obj.increment.call(otherObj);

```

### Subclassing alternate approach.

```javascript
function userCreator(name, score) {
	(this.name = name), (this.score = score);
}

userCreator.prototype.sayName = function() {
	console.log('I am ' + this.name);
};

userCreator.prototype.increment = function() {
	this.score++;
};

const user1 = new UserCreator('Phil', 3);
user1.sayName();
```

Here we use the new creator which creates an object implicitly with it's
`__proto__` reference to `userCreator.prototype`.

```javascript
function paidUserCreator(paidUserName, paidUserScore, accoundBalance) {
	userCreator.call(this, paidUserName, paidUserScore);
	this.accountBalance = accountBalance;
}

paidUserCreator.prototype = Object.create(userCreator.prototype);

paidUserCreator.prototype.increaseBalance = function() {
	this.accountBalance++;
};

const paidUser = new paidUserCreator('Alyssa', 8, 10);
paidUser.increaseBalance();
paidUser.sayName();
```

We want to call the userCreator function from paidUserCreator.
When the userCreator function runs, we want the **_this_** keyword
to reference the paidUserCreator instance.

The paidUserCreator function returns an object which looks like this

```javascript
{
name:'Alyssa',
score:8,
accountBalance:10
__proto__:paidUserCreator.prototype
}
```

```javascript
paidUserCreator.prototype = Object.create(userCreator.prototype);
```

The above line assigns the userCreator.prototype to the proto reference of the paidUserCreator.prototype object.

```javascript
class UserCreator {
	constructor(name, score) {
		(this.name = name), (this.score = score);
	}
	sayName() {
		console.log('I am' + this.name);
	}
	increment() {
		this.score++;
	}
}

const user1 = new UserCreator('Will', 9);
```

### Class syntax with the Super and Extends Keyword

The class keyword is essentially a object-function combo like
the example before.

The constructor function is function part of the combo which
takes in the parameters and assigns them to the object
created by the 'new' keyword.

```javascript
class paidUserCreator extends userCreator {
	constructor(paidUser, paidScore, accountbalance) {
		super(paidName, paidScore);
		this.accountBalance = accountBalance;
	}
	incrementBalance() {
		this.accountBalance++;
	}
}

const paidUser1 = new paidUserCreator('Alyssa', 8, 10);
paidUser1.increaseBalnce();
paidUser.sayName();
```

We know that the paidUserCreator is a function-object combo
which has on it's object has a prototype property with
the method increaseBalance.

The **_extends_** keyword sets the proto reference to
the UserCreator.prototype.

```javascript
{prototype: {
           increaseBalance:function(){},
          __proto__:UserCreator.prototype
           }
__proto__:userCreator
}

```
The ***extends*** keyword also sets the proto reference
to the userCreator object-function combo.

```javascript
paidUser1 = new paidUserCreator('Alyssa', 8, 25);
```

When we call the paidUserCreator,
we are going to assign the arguments to the parameters.
The this assignment is uninitialized. We call super with the paidName and the paidScore parameter
values.

The **_this_** in paidUserCreator is going
to be assigned to the return value of calling super.

```javascript
this=super('Alyssa',8)

//Behind the scenes
this=Reflect.construct(userCreator,[Alyssa,8],paidUserCreattor)

//Which is essentially doing this
this=new userCreator('Alyssa',8)
```

We are going to create the new object inside
of our class Usercreator unlike our above approach where
we created a new object in paidUserCreator and created
a side effect.

One gotcha to note here is whenever we create
an object using the **_new_** keyword infront of userCreator,
the returned object's proto reference is set
to userCreator.prototype.

However, the super keyword overrides the proto
reference in our new object to paidUserCreator.prototype
which we did manually before.

The new object returned is assigned to 
***this*** in paidUserCreator.


-----------------------------------------------------------------------------------------------------


### Summary

- The Object.create() creates a delegation pattern. If a property does not exist in the current object, the JS engine performs a lookup to the object to which the link is created to.

- The prototype is a property on a function that point to an object.

- There are two ways in which Object.create() can be used.

  - We can explicitly create an object with some function definitions and pass a reference of it to Object.create.
  - We can define the functions on the prototype property and have Object.prototype reference that.

- The 'new' keyword implicity creates an empty project and creates a link to the property which has the methods defined on it. It also implicity returns an object.

- Class is just syntactical sugar and abstracts the implementational details of javaScript's prototypal inheritance and helps you focus just on business logic.

- `javascript const friends=new Array()`

- The methods like forEach, filter, map live on the Array.prototype.

- Object.getPrototypeOf() gives you the prototype object a specific instance delegates to.

- ***You can use a 'new' keyword on an arrow function***

### Summary by example to get this etched in your brain

![Prototype](https://user-images.githubusercontent.com/15992276/59235390-ef848f80-8bbe-11e9-9119-6222e336fa6e.JPG)
![Prototype2](https://user-images.githubusercontent.com/15992276/59235391-ef848f80-8bbe-11e9-92f5-737592dbeded.JPG)
![Prototype3](https://user-images.githubusercontent.com/15992276/59235392-ef848f80-8bbe-11e9-941a-26e258a2dbf2.JPG)
![Prototype4](https://user-images.githubusercontent.com/15992276/59235393-ef848f80-8bbe-11e9-8d96-a907ec26384a.JPG)

![class vs prototype](https://user-images.githubusercontent.com/15992276/59235386-ef848f80-8bbe-11e9-835f-c2ac034210e6.JPG)


### Hold on, there is more, Composition vs Inheritance

>By favoring composition over inheritance and thinking in terms of what things do rather than what things are, you free yourself of fragile and tightly couple inhertiance structures-Abraham Lincoln

![composability](https://user-images.githubusercontent.com/15992276/59235387-ef848f80-8bbe-11e9-9741-bfe69228c72d.JPG)
![composability1](https://user-images.githubusercontent.com/15992276/59235388-ef848f80-8bbe-11e9-832c-f1aca72727e2.JPG)
![composability3](https://user-images.githubusercontent.com/15992276/59235389-ef848f80-8bbe-11e9-92e6-4022d744c810.JPG)








































































### Summarizing through an example so this is etched into your brain

![Prototype](https://user-images.githubusercontent.com/15992276/59235241-3faf2200-8bbe-11e9-9d05-1b9d2e73fd06.JPG)
![Prototype2](https://user-images.githubusercontent.com/15992276/59235242-3faf2200-8bbe-11e9-9ff6-fb91363ef579.JPG)
![Prototype3](https://user-images.githubusercontent.com/15992276/59235243-3faf2200-8bbe-11e9-8530-5a3ce751dc15.JPG)
![Prototype4](https://user-images.githubusercontent.com/15992276/59235244-3faf2200-8bbe-11e9-8d74-f08ebda84907.JPG)
![class vs prototype](https://user-images.githubusercontent.com/15992276/59235237-3f168b80-8bbe-11e9-9a1b-b34412460dcb.JPG)

### Composability vs inheritance

> By favoring composition over inheritance and thinking in terms of what things do rather than what things are, you free yourself of fragile and tightly couple inhertiance structures- Abraham Lincoln

![composability](https://user-images.githubusercontent.com/15992276/59235238-3f168b80-8bbe-11e9-9c41-62cf811cc31a.JPG)
![composability1](https://user-images.githubusercontent.com/15992276/59235239-3f168b80-8bbe-11e9-8824-971014072b48.JPG)
![composability3](https://user-images.githubusercontent.com/15992276/59235240-3faf2200-8bbe-11e9-9ca4-ba8de086d98d.JPG)
