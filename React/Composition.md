```javascript

function Animal(name, energy){
this.name=name
this.energy=energy
}


Animal.prototype.eat=function(amount){
console.log(`${this.name} is eating);
}

Animal.prototype.sleep=function(length){
console.log(`${this.name} is eating.`);
this.energy+=amount;
}

const leo=new Animal('Leo',7);

```

Object.create allows you to create an object which will delegate to another object on failed lookups.

Simply put ,every function in JS has a prototype property that references an object.

```javascript
function doThing() {}
console.log(dothing.prototype); //{}
```

```javascript
function Animal(name, energy) {
	this.name = name;
	this.energy = energy;
}

Animal.prototype.eat = function(amount) {
	console.log(`${this.name} is eating `);
	this.energy += amount;
};
```

### Inheritance

```javascript
function Dog(name, energy, breed) {
	Animal.call(this, name, energy);
	this.breed = breed;
}

const charlie = new Dog('Charlie', 10, 'Goldendoodle');

function Dog(name, energy, breed) {
	Animal.call(this, name, energy);
	this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
```

Any instances of Dog which log instance.constructor are going to get the Animal constructor rather than the Dog constructor.

The constructor property returns a reference to the Obect constructor function that created the instance object. Note that the value of this property is a reference to the function itself, not a string containing the function's name.

The most significant weakness with inheritance is that the we structure our classes around what they are, An User, An Animal, A Cat, A Dog- all these words encapsulate a meaning centered around what these things are.

But rather than thinking in terms of what things are, what if we think in terms of what things are.
Let's take a Dog, for example. A Dog is a sleeper, eater, player and barker. A Cat is a sleeper , eater, player and meower. A User is a sleeper, eater, player and adopter.

Let's transform these verbs into functions.

```javascript
const eater = () => ({});
const sleepter = () => ({});
```

Now whenever a Dog, Cat or User needs to add the ability to do any of the functions above, they merge the object they get from one of the functions onto their own object.

```javascript

const eater=(state)=>({
eat(amount){
state.energy+=amount
}
}}

function Dog(name, energy, breed){
let dog={
name,
energy,
breed
}

return Object.assign(dog, eater(dog))
}

const leo=Dog('Leo',10,'Goldendoogle');
leo.eat(10)  //Leo is eating
leo.bark()   // Woof Woof!
```
