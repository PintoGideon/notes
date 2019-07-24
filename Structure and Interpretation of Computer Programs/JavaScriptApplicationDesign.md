Chapter 1

Core Principles in Build First

- Reducer Error Proclivity because there's no human interaction
- Enhanced productivity by automating repetitive tasks
- Modular, scalable application design

A build process is intended to automate repetitive tasks such as installing dependencies, compiling code, running unit tests and performing any other important functions.

### Introducing Grunt

Grunt is a task running that helps you execute commands, run JS code and configure different tasks with the config entirely written in JS.

Tasks can be imported from plugins which are Node modules containing one or more Grunt tasks. You only need to figure out what config to apply to them, and that's it. the task
itself is handled by the plugin.

Installing grunt using npm.

The following code shows the bare minimum Gruntfile.js module:

```javascript
module.exports = function(grunt) {
	grunt.registerTask('default', []); //register a default task alias
};
```

The first step in setting up a Grunt task is installing
a plugin.

Grunt plugins are usually distributed as npm modules, which are pieces of js code someone published so you can use them.

```javascript
grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.registerTask('default', ['jshint']);
```

### Chapter 5

### Working with code encapsulation

Encapsulation means keeping functionality self-contained and hiding implementation details from consumers of a given piece of code.

### Information hiding and interfaces

One way to drive down the complexity creep is to hide away
unnecessary information, keeping it inaccessible on the interface. This way only what matters gets exposed; the rest is considered to be irrelevant to the consumer, and it's
often referred to as implementation details.

You don’t want to expose elements such as
state variables you use while computing a result or the seed for a random number generator.

```javascript
function Average() {
	this.sum = 0;
	this.count = 0;
}
Average.prototype.add = function(value) {
	this.sum += value;
	this.count++;
};
Average.prototype.calc = function() {
	return this.sum / this.count;
};
```

The example below takes an array of values, and it returns the average, as its name indicates. In addition,it doesn’t keep any state variables the way your prototypical implementation did, effectively hiding
any information about its inner workings. This is what’s called a pure function: the result can only depend on the arguments passed to it, and it can’t depend on state
variables, services, or objects that aren’t part of the argument body.

Pure functions have another property: they don’t produce any side effects other than the result they provide. These two properties combined make pure functions good interfaces; they
are self-contained and easily testable.

```javascript
function average(values) {
	var sum = values.reduce(function(val, accum) {
		return val + accum;
	}, 0);
	return sum / values.length;
}
```

### Factory Functions

An alternative implementation might use a functional factory. That’s a function that,
when executed, returns a function that does what you want. As you’ll better understand in the next section, anything you declare in the factory function is private to the
factory, and the function that resides within.

```javascript
function averageFactory() {
	var sum = 0;
	var count = 0;
	return function(value) {
		sum += value;
		count++;
		return sum / count;
	};
}
```

![An Overview of the Build Process](https://user-images.githubusercontent.com/15992276/60635345-802a4600-9de0-11e9-9374-fc7e3d4b6ee2.JPG)
![Capture](https://user-images.githubusercontent.com/15992276/60635346-802a4600-9de0-11e9-83d9-1a5de7ab2592.JPG)
![Task-Target config](https://user-images.githubusercontent.com/15992276/60635347-802a4600-9de0-11e9-8100-d4c6ee0b89a8.JPG)


