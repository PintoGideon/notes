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











