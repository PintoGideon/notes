> As application grow in size, it becomes harder and harder to reason about
  and any additional information we can codify in our syntax when we author our 
  program is hugely valuable. Any extra info in the syntax will be hugely useful
  
 # Abstract Syntax Tree
   
   The typescript compiler is made up of a couple of different parts or phases and of of
   one of these parts is the parser. Parser is responsible for looking at our code
   and transforming it into a data structure which other parts of the typescript compiler will
   use and understand.
   
   # Getting Started
   
   ```  javascript
   
   let x="hello world";
   x="Hello mars"


   let zz:number
   zz=41;
   zz="abc"  //Error Type "abc" is not assignable to type "number"


   let aa:number[]=[];
   aa.push(33);
   aa.push("abc"); //Error: Argument of type "abc" is not assignable to parameter of type "numner"


   let cc: {houseNumber:number, address:string}
   cc={
     streetName:"Fake Street",
     houseNumber:123
   } 


   ```

### Interfaces

```javascript

interface Address{
   houseNumber:number,
   streetName?:string
}


let ee: Address={
   houseNumber:33
}


export interface HasPhoneNumber{
   name:string,
   phone:number
}

export interface HasEmail{
   name:string,
   email:string
}



let contactinfo:hasEmail | HasPhoneNumber=
Math.random>0.5 ? {
   name:"Mike",
   address: Park Drive
}: {
   name:"Mike",
   address:"Washington Square"
}


contactInfo.name // We can only access the .name property which is common to both.




```

   
```javascript

let words=['red', green, 'blue']
let foundWord:boolean;

for (let i=0;i<words.length;i++){
    if(words[i]==='green'){
        foundWord=true;
    }
}

```

### Type Inference


Type annotations for functions are the code that we tell Typescript what Type
of arguments a function will receive and what type of values it will return

Type inference for functions tries to figure out what type of value a function
will return

```javascript

const add=(a:number,b:number):number=>{
return a+b;
}


function divide(a:number, b:number):number{
return a/b;
}

const multiply=function(a:number, b:number):number{
    return a+b;
}

//void

const logger=(message:string):void=>{
    console.log(message);
}

const throwError=(message:string):void=>{
    if(!message){
        throw new Error(message)
    }
}

const forecast={
    date:new Date();
    weather:'sunny'
}


// Destructuring in Typescript

const todaysWeather={
    date:new Date(),
    weather:'sunny';
}


const logWeather=({date,weather}:{date:Date, weather:string}):void{
    console.log(forecast.date)
    console.log(forecase.weather)
}

logWeather(todaysWeather)


const profile={
    name:'Alex',
    age:20,
    coords:{
        lat:0,
        lng:15
    },
    setAge(age:number):void{
          this.age=age;
    }   
}

const age=profile.age
const age:number=profile.age

//Destructured syntax (Natural intuition)
const {age}:number=profile;

//Destructured syntax (THe correct syntax)
const {age}:{age:number}=profile;


//Similarly

const coords:{lat:number, lng:number}=profile.coords.
const lat=coors.lat;
const lng=coors.lng;


const {coords:{lat,lng}}: {coords:{
    lat:number,
    lng:number
}}=profile;

```

### Arrays in Typescript

Typed Arrays are arrays where each element is some consistent type of value

```javascript
const carMakers:string[]=['ford','toyota','chevy'];
const dates=[new Date(), new Date()];

const carsByMake:string[][]=[
    ['f150'],
    ['corolla'],
    ['camaro']
];

```

```javascript

const car=carMakers[0];  //string
const myCar=carMakers.pop(); //string

catMakers.map((car:string):string=>{
    return car.toUpperCase();
})

```

### Flexbile Typescript

```javascript

const importantDate:(Date|string)[]=[new Date()];
importantDate.push('2030-10-10');
importantDate.push(new Date());

```
### Tuples in Arrays
Tuples are array like structures where each element represents
some property of a record. If we are working with a CSV file,
we might use a tuple.

```javascript

const beverage={
    color:brown,
    carbonated:true,
    sugar:'40gm'
}

//tuples.ts

const pepsi:[string,boolean,number]=['brown', true, '40gm'];

or

type Drink=[
    string, boolean, number
]

const sprite:Drink=['brown',true,'50gm']

```

### Interfaces

Interfaces + Classes= How we get really strong code reusability.

Interfaces creates a new type, describing the property names
and value types of an object.

```javascript
const oldCivic={
    name:'Civic',
    year:2000,
    broken:true
    summary():string{
        return `Name:${this.name}`
    }
}

const printVehicle=(vehicle:{name:string; year:number, broken:boolean}):void=>{
console.log(`Name:${vehicle.name}`);
console.log(`Year:${vehicle.broken}`);
}

printVehicle(oldCivic);

//Rewriting the above code with the help of an Interfaces

interface Vehicle{
    name:string,
    year:number,
    broken:boolean
    summary():string;
}


const printVehicle=(vehicle:Vehicle):void=>{
    console.log(`Name:${vehicle.name}`);
    console.log(`Year:${vehicle.year}`);
    console.log(`Broken:${vehicle.broken}`);
    console.log(`Summary:${vehicle.summary}`);
}

printVehicle(oldCivic);

```

### Code Reusability with interface

```javascript

// Reusing the SummaryReport interface

interface SummaryReport{
    summary():string;
}

const drink={
    color:'brown',
    carbonated:true,
    sugar:40,
    summary():string{
        return `My drink has ${this.sugar} grams of sugar`
    }
}

const printSummary=(item:SummaryReport):void{
    console.log(item.summary);
}

printSummary(oldCivic);
printSummary(drink)

```

### General Plan for Interfaces

1. Create functions that accepts arguments that are typed with interfaces.
2. Objects/Classes can decided to implement a given interface to work with a function


```javascript
 class Vehicle{
     color:string;

     //This code can also be written also
     /*
         constructor(public color:string){}
     */

     constructor(color:string){
         this.color=color;
     }

     protected honk():void{
         console.log('beep')
     }
 }

 const vehicle=new Vehicle('orange');
 ```






































































