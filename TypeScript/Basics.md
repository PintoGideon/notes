> As application grow in size, it becomes harder and harder to reason about
  and any additional information we can codify in our syntax when we author our 
  program is hugely valuable. Any extra info in the syntax will be hugely useful
  
 # Abstract Syntax Tree
   
   The typescript compiler is made up of a couple of different parts or phases and of of
   one of these parts is the parser. Parser is responsible for looking at our code
   and transforming it into a data structure which other parts of the typescript compiler will
   use and understand.
   
   # Getting Started
   
   
   
   ```typescript
   var age =1
   ```
   The value of 1 is a number and age has a type associated with it
   age:number
   
   ```javascript
   
   npm init
   npm install -g typescript
   tsc --init
   
   ```
   ***When we use const instead of var, typescript will take the literal value as the
   literal type***
   
   ```javascript
   const symbol='#'
   ```
   
   # Control Flow Analysis
   
   Typescript has introduced the idea of non nullable types. 
   In the code below, the result of getElementById might not exist in the dom at the time
   we are querying it. Typescript performs ***control flow analysis*** where it will look
   at ```if statements, switch statements``` and know how it will affect the types in
   our program. At the time we run the if statement, app could be an HTML element
   or null. But within the body of our if statement, we know that app must be truthy which means app must be an HTML element.
   
   
   ```javascript
   function main() {
	var appcomponent = document.getElementById('app');
	setInterval(function() {
		if (appcomponent) {
			appcomponent.innerHTML = generateRandomId();
		}
	}, 1000);
}
```
   
  # Collections
  
  ```javascript
   let size:number[];
   size=[1,2,3]
  ```
  
  ### Generic Types
  
  ```javascript
  let toppings:Array<string>;
  toppings=['pepperoni','tomato','bacon']
  ```
  
  ### Tuple Type
 
  ```javascript
  let pizza:[string,number,boolean];
  pizza=['Pepperoni',20,true]
 ``` 
  The tuple type needs to conform to the exact argument list.
   
  ### Type alias
  
  ```javascript
  type Size: 'small' | 'medium' | 'large';
  let pizzaSize: Size='small'
  
  const selectSize=(size:Size)=>{
  pizzaSize=size;
  }
  selectSize('small')
  ```
  
  ### Type assertion
  
  ```javascript
  type Pizza={name:string, toppings:number}
  
  const pizza:Pizza={name:'Blazing Inferno',toppings:5}
  const serialized=JSON.stringify(pizza);
  
  function getNameFromJSON(obj:string){
  return(JSON.parse(obj) as Pizza).name
  }
  getNameFromJSON(serialized)
  
  ```
  
  # Classes 
  
  ```javascript
  function Pizza(name:string){
  this.name=name;
  this.toppings=[];
  }
  
  Pizza.prototype.addTopping=function addTopping(topping:string){
  this.toppings.push(topping);
 
  }
  
  const pizza=new Pizza('Pepperoni');
  pizza.addTopping('pepperoni');
  
  
  ```
  
  Rewriting the above code using Classes
  
  ```javascript
  
  class Pizza{
  name:string;
  toppings:string[]=[]
  
  constructor(name:string){
  this.name=name;
  }
  
  addTopping(topping:string){
  this.toppings.push(topping);
  }
   
  }
  
  const pizza=new Pizza('pepperoni')
  pizza.addTopping('pepperoni')
 ``` 
  
  ### Interface
  
  Interfaces just help us define the shape of a particular object.
  
  ```javascript
  
  interface Pizza{
  name: String
  size:string[]
  }
  
  
  let pizza=Pizza;
  
  function createPizza(name:string, sizes:string[])
  {
  //API to Create the pizza and return some value
     return{
     name,
     sizes
     }
    
  }
  
 pizza=createPizza('Pepperoni',['small','medium'])
  
  ```
  
  