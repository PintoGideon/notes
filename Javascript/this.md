## Implicit Binding

```javascript
var sayNameMixin = function(obj) {
	obj.sayName = function() {
		console.log(this.name);
	};
};

var me = {
	name: 'Tyler',
	age: 25
};

var you = {
	name: 'Joey',
	age: 21
};

sayNameMixin(me);
sayNameMixin(you);

me.sayName(); //Tyler
you.sayName(); //Joey
```

## Explicit Binding

### Call, Apply and Bind

1. Call

```javascript
var sayName = function() {
	console.log('My name is' + this.name);
};

var stacey = {
	name: 'Stacey',
	age: 34
};

sayName.call(stacey);
sayName.apply(stacey);
```

**_Bind returns a new function instead of invoking the original function_**

```javascript
var newFn = sayName.bind(stacey);
newFn();
```




