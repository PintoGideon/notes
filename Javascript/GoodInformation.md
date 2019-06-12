Side effects come in two flavours: reading and writing. When you're expecting an input from a console, you can't predict the exact value that will come – why would you need an input otherwise. And when you're writing to a console, a file, a database... you're change (mutate!) some external state. Mutation is a standard term, describing a value (or variable) change. It's important to get that a) mutation is a side effect b) any writing side effect can be seen as mutation.

```javascript
var array = [1, 2, 3];
```

Arrays in javascript are objects. If we want to know
if a variable is an array, we can use the isArray function.

```javascript
Array.isArray([]);
```

### Pass by Reference and Pass by Value

Primitive variables are immutable.`We can't modify
a variable declaration.

```javascript
var a = 10;
```

Variable a references the value 10.

```javascript
var a = 5;
var b = a;

b++; //6
a; //5
```

---

```javascript
let obj1 = {
	name: 'Gideon',
	password: 'secret123'
};

let obj2 = obj1;

obj2.password = 'yeezy123';

console.log(obj1); //{name:'Gideon', password:'yeezy123'}
console.log(obj2); //{name:'Gideon',password:'yeezy123'}
```

```javascript
var c = [1, 2, 3, 4, 5];

var d = c;
d.push(187628761);
console.log(d); //[1,2,3,4,5,187628761]
console.log(c); //[1,2,3,4,5,187628761]

var d = [].concat(c);
d.push(187628761);
console.log(d); //[1,2,3,4,5,187628761]
console.log(c); //[1,2,3,4,5]
```

```javascript
let obj={a:'a',b:'b',c:'c'}
let clone=Object.assign({},obj}

obj.c=5;
console.log(clone);
//Output
{a:'a',b:'b',c:'c'}

```

**_Important Concept_**

```javascript
let obj = {
	a: 'a',
	b: 'b',
	c: '5'
};

//Shallow clone

let clone = Object.assign({}, obj);
let clone2 = { ...obj };

obj.c = 5;
console.log(obj); // {a:'a',b:'b',c:5}
console.log(clone); // {a:'a',b:'b',c:'c'}
console.log(clone2); // {a:'a',b:'b',c:'c'}
```

### Shallow Clone vs Deep clone

```javascript
let obj={
    a:'a',
    b:'b',
    c:{
        deep:'Copy me'
    }
}

let clone=Object.assign({},obj);
let superclone=JSON.parse(JSON.stringify(obj));

obj.c.deep:'hahaha';
console.log(obj); //{a:'a',b:'b',c:{deep:'hahaha'}};
console.log(clone); //{a:'a',b:'b',c:{deep:'hahaha'}};
console.log(superclone);//{a:'a',b:'b',c:{deep:'Copy me'}};
```


### Recreating Arrays from Scratch




