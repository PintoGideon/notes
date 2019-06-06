### How React really works?

```javascript

import React from 'react';
import ReactDOM from 'react-dom'

function HelloWorld(){
return(
<div>Hello World!</div>
);

}

ReactDOM.render(
<HelloWorld/>,
document.querySelector('#root'));

```


React uses the concept of a virtual DOM. It creates a representation of your component hierarchy and then renders those components by creating real DOM elements and inserting them where you tell it. In the above case, that's inside the
element with an id of root.

JSX is compiled down to JS by Babel.

### Two approaches to Building a react app

1. Top-down: Start at the top. Build the Tweet Component first and then
it's children

2. Bottom-up: Start at the bottom(the leaves of the tree). Build Avatar, then name with handle, then the rest of the child components. Verify that they work in isolation. Once they are all done, assemble them into the Tweet.

### JSX

In JSX, single braces must surround JS expressions. The code
in the brace is real Javascript, and it follows the same scoping rules as normal Javascript.

It is important to understand that the JS inside the braces
must be an expression and not a statement. It would be helpful to ask this question when deciding what to put in a
JSX expression. "Could I pass this as a function argument?"


## Communication with Parent components

If you can't change props but you need
to communicate something up to a parent
component, how does that worK?

If a child needs to send data to it's parent, the parent
can send down a function as a prop like this:

```javascript

function handleAction(event){
console.log('Child did:',event)
}

function Parent(){
return(
<Child onAction={handleAction}/>
)
}

```

You can validate that an object has a certain shape, meaning that it has particular properties. The object passed to this 
prop is allowed to have extra properties too, but it must at least have the ones in the shape description.

```javascript
PropTypes.shape({
name:PropTypes.string,
age:PropTypes.number
})
```

This will match an object of the exact shape like this:

```javascript
person={
name:'Joe',
age:27
}
```

### Children

We can image using it like this.

![icon1](https://user-images.githubusercontent.com/15992276/58818308-40bfdc80-861d-11e9-86db-375bbe51b82c.JPG)


<IconButton>Do the thing </IconButton>

What happens to the innerText, "Do the thing"?. Well, that where the children prop comes in. When React renders IconButton, it will pass all of it's sub-elements into
IconButton as a prop called Children.

The children are automatically passed in, but they're not
automatically displayed anywhere. You have to explicitly render the children somewhere in your component. If you don't, they are ignored.

```javascript
function IconButton({children}){
return (
<button>
<i class="target-icon"/>
{children}
</button>
);
}

```

### Different types of Children

The children prop is always pluralized as children no matter whether there's a single child or multiple children. When there are multiple children, children is an ***array of Reactelements***. When there is only on child, it is a ***ReactElement object***.


```javascript
<Nav>
    <NavItem url="/">Home</NavItem>
    <NavItem url="/about">About</NavItem>
    <NavItem url="/contact">Contact</NavItem>
</Nav>

function Nav({children}){
    let items=React.children.toArray(children);
    for(let i=items.length-1;i>=1;i--){
        items.splice(i,0,<span key={i} className="seperator"
        >|</span>
        )
    }
}
```

### The 'key' Prop

The key prop on the <tr> is a special one and it's required
any time you render an array of elements.

React uses it to tell components apart when reconciliing
differences during a re-render.


### State

- an intial State
- A call to this.setState
- display the current count



When the component is first instantiated, it's initial
state is setup via the state property intializer.

Most of the times, an event triggered in the child component
calls a function in the parent component. The function in the parent component call this.setState with an object that
describes the new state.

The setState function will update the state and then re-render the component and all of it's children. The parent's
render function is called which displays the newly updated
state.

***Every instance of a component has it's own state.***

### setState is Asynchronous

The setState function is actually asynchronous. If you setState and act on it immediately, it is likely to print
the old state instead of the one I just set. We can use a callback to perform the above action.

```javascript

this.setState({name:'Gideon'},function(){
});

```

### Functional setState

Another way to make it so that sequential state updates
run in sequence is to use the functional form of setState
like this:

```javascript
this.setState((state,props)=>{
return{
value:state.value+1
}
})
```

### Shallow vs Deep Merge

When you call setState, whetehr we call it with an object
or in the functional form, the result is that it will
shallow merge the properties in your object with the current
state.

```javascript
{
score:42,
 user:{
 name:'somebody',
 age:26
 }
}
```

After we run this this.setState({score:56}), the new state
will be

```javascript
{
score:56,
user:{
name:"somebody",
age:26
},
prodcuts:[]
}

```

That is it merges the object you pass to setSate with the
existing state. It doesn't erase the existing state and
it doesn't replace the whole top-level state with your
object.


### Handling events

The event object passed to a handler function is only valid right at the moment. The SyntheticEvent object is pooled for
performance. Instead of creating a new one for every event,
React replaces the content of the one single instance.

### What to put in state

Data stored in state should be reference inside the render
somewhere. Component state is for storing UI state- things 
that affect the visual rendering of the page. Anytime
the state is updated, the component will re-render.

### Kinds of Components

Presentational components are stateless. They simply accept
props and render some elements based on those props.

Container a.k.a Smart components are stateful. The maintain
state for themselves and any child components, and pass it down to them via props. 


### Controlled Inputs

The reason they are called 'controlled'
is because you are responsible for controlling
their state.
Remember how a state works: a value and a callback
function are passed in as props, the child
notifies the parent of events (via the callback prop)
and then the parent component is reponsible for reacting
to changes and passing an updated value to the child.

### Refs
A ref gives you access to the input's underlying DOM node,
so you can pull out it's value directly. Here's an example
using a ref on an input to extract its value when a button
is clicked:

```javascript
class RefInput extends React.Component{
showValue=()=>{
alert(`Input containers: ${this.input.value}`);
}


render(){
return(
<div>
<input type="text" ref={input=>this.input=input}/>
<button onClick={this.showValue}>
Alert the value!
</button>
</div>
);
}
}
```

The ref prop is a special once. Pass it as a function and
React will call that function with the component's DOM
element once the component is finished mounting. We are saving the DOM element on the component instance so that we
can access it from the showValue function later.



### The Component Lifecycle

Every React Component goes through a lifecycle as it's rendered on screen. There are a sequence of methods called
in a specific order. Some of them run once and some run
multiple times.

Within the lifecycle of a component, there are a few phases
that occur.

![compon](https://user-images.githubusercontent.com/15992276/58834636-652eaf80-8643-11e9-9363-1b489f1f9679.JPG)


1. ***Mount*** occurs when the component is first added to the DOM. Initialization and setup are done here.
2. ***Render*** occurs when the component renders for the first time, and whenever it re-renders due to change in props
or state
3. ***Commit*** takes the output from render and updates the DOM to match
4. ***Unmount*** happens when the component is being removed from the DOM.



### Mounting

***Constructor***: First method called, when the component is created. If state is initalized with a property intializer then it will already be set by the time the constructor execute.

***componentDidMount***: Called immediately after the first render. The component's children are already rendered at the point too. This is a good place to make AJAX requests to fetch any data you need.

***static getDerviedStateFromProps(nextProps,prevState)***: This is an opportunity to change the state based on the value of props which can be used for intialization. This method must not have side effects so don't call setState here.

***shouldComponentUpdate(nextProps,nextState)***: This is an opportunity to prevent rendering if you know that props and state have not changed. The default implementations always return true. If you return false, the render will not occur.

***getSnapshotBeforeUpdate(nextProps,prevState)***: Called after
Render but before changes are committed to the DOM.
If you need to do any calculations based on the old DOM, this is the time to do it. Return anything you want from this function and the value will get passed as the third argument to componentDidUpdate

***componentDidUpdate(prevProps,prevState,snapshot)***: Render is done. DOM changes have been committed. You can use this opportunity to operator on the DOM if you need to. Prior to this, DOM nodes would still be in Flux.


### Unmounting:

***componentWillUnmount***:The component is about to be unmounted. Maybe it's item was removed from a list, maybe the user navigated to another tab.











