```javascript

const itemsToRemove={id:'1',name:'PoshDog',price:400}

/* I want to remove the price property immutably */

const {price,...leftOverItems}=itemsToRemove;
console.log(price, itemsToRemove);
```

### Identity

```javascript
console.log(itemsToRemove===itemsToRemove); //true
console.log(itemsToRemove===leftOverItems); //false

```
Modern frameworks like React and Angular will use an identity comparison to make change detection faster.

### Imperative vs Declarative

```javascript

const items=Object.freeze([
    {id:1,name:'Super burger',price:500},
    {id:2,name:'Double Whopper',price:199}
])

const getTotalPure=v=>v.reduce((x,y)=x+y.price,0)
document.querySelector('#app').innerHTML=getTotalPure(items)

```

### Closures

```javascript

const getNameFromItems=(name)=>{
    return (items)=>{
       return items.find((item)=>item.name===name)
    };
}

const superBurger=getNameFromItems('Super burger');
console.log(superBurger(items)); // {id:1 , name:'Super burger',price:400}


```

### Higher Order Function

Higher order function returns a new function.
Takes other functions as arguments.


### Currying

```javascript

const curry=(fn)=>{
    return (...args)=>{
        //console.log('Args',...args)  //["Super burger"]
        return fn.bind(null, ...args)
    }
}
const getNameFromItems=curry(
    (name,items)=>items.find((item)=>item.name===name)
)
const getBurger=getNameFromItems('Super burger')(items)

```

```javascript

const slugify='Ultimate Courses'.split(' ').map((x)=>x.toLowerCase()).join('-')); //ultimate-courses

```
We are using function composition to build of a chain of operations

Creating our own utility function.


```javascript
const curry=(fn)=>(...args)=>{
    if(args.length >= fn.length){
        return fn.apply(null,args)
    }
    return fn.bind(null,...args)
}

const map=curry((fn,array)=>array.map(fn));
const join=curry((seperator,string))=>string.join(seperator))
const split=curry((seperator, string)=>string.split(seperator))

const slugify=(str)=>join('-')(map(toLowerCase)(split(' ')(str)))
console.log(slugify('Ultimate Courses'))
//ultimate-courses



```





