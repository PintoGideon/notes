### Hoisting

```javascript

console.log(teddy)
var teddy='bear;
sing();
function sing(){
console.log('Oh hey hi')
}

```
***Output*** 
undefined
Oh hey hi

The JS engine is a 2 pass system. In the first pass,
it is going to hoist or assign a memory location
to variables and functions and in the 2nd pass,
it's going to assign a value.

Variables are partially hoisted and functions are fully hoisted. In the above

```javascript
(function(){
console.log("Does this hoist")
})()
```

The above function does not hoist. const and let are not
hoisted. It's going to throw a reference error.

### What about function expressions?

```javascript
var app=function(){
console.log('How does this hoist?');
}
```
The variable app is going to be hoisted and assigned
undefined. If I run sing2() before it is defined, I get
an error saying that the function is not defined.

### Difference between a function expression and declaration

Function expressions are defined at runtime while
function declarations are defined at parse time.

![Variable environment](https://user-images.githubusercontent.com/15992276/59047082-ba043d00-8872-11e9-9684-272abbc5d1b1.JPG)

When a function is invoked, we get the this keyword and
arguments. The arguments keyword give us an object with
the argument passed as property values.

Now we want the arguments to be an array so that we can
perform some computations on them. Now the arguments are
an array.

```javascript

function marry2(...args){
console.log('Arguments',arguments);
}

marry('Tim','Tina')
```

### Variable Environment

In Javascript, our lexical scope determines our
available variable. Not where the function is called.


![The scope chain](https://user-images.githubusercontent.com/15992276/59047080-ba043d00-8872-11e9-865d-72df855ebebf.JPG)

```javascript

function sayName(){
var a='a';

return function findName(){
var b='b';
console.log(c)

return function printName(){
var c='c';
return 'Andrei Neagoie'
}
}
}

```

### IIFE

One major issue with global variables is that we can have collissions. To avoid
this JS developers used an IIFE.

An IIFE is an function expression which looks like this:

```javascript
(function(){

})();
```

Using thi design pattern, we can define all our variables in a local scope. We can aalso see
that the function expression is immediately invoked. We cannot do the same
with a function declaration. What's the benefit of this?

```javascript
(function(){
var a='Is this available outside'
})();

console.log(a) //reference error
```
This is really powerful though. I can encapsulate the behaviour of the
underlying function and only return the values neeed. We still used
a global namespace but we kept it from polluting.

```javascript
var z=1
var script1=(function(){
return 5;

})()
```

***JQuery made use of the IIFE pattern***

```javascript

// Importing jquery here
<script src="http://code.jquery.com/......"></script>

// Now jquery has been added to our window object
<script>

var script1=(function(what){
what('h1').click(function(){
what('h1').hide();
})

function a(){
return 5;
}

return {
a:a
}

})(jQuery)
</script>
```

We can also do something like this
```javascript
script.a()  //5
```

### this keyword

***this*** is the object that the function is a property of. 

```javascript
function a()
{
console.log(this)
}
a();  //Window

```




































