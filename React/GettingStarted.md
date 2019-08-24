### How React really works?

There are two ways to declare React Components.

- As ES6 classes
- Importing and using createReactClass() method

```javascript
class HelloWorld extends React.Component{
  render(){
    return <p>Hello World</p>
  }
}

```

The render() method of a component needs to describe how the view should be represented
as HTML. React builds our apps with a fake representation of the DOM model.

### ReactDOM.render()

```javascript
class ProductList extends React.Component {
  render() {
    return (
      <div className="ui unstackable items">
        Hello Friend! I am basic React Comp
      </div>
    );
  }
}

ReactDom.render(<ProductList />, document.getElementById("content"));
```
 We pass in two arguments to the ReactDOM.render() method. The first argument is what we would like to render and the second where to render it.

 For what, we are passing in a reference to our ProductList component, from where you might recall index.html
 contains a div tag with an id of content

 ```
<div id="content"></div>
 ```
We pass in a reference to the DOM node as the second argument to see ReactDOM.render().



### this keyword

this is a special keyword in JS. The details of 'this' are a bit nuanced. In React, 'this'
will be bound to the React Component Class. When we write this.props, we are accessing
the props property on the component. 


### Building Custom Component methods

In react, inside render(), this is bound to the component. Understanding the binding of this
is one of the trickiest parts of learning Javascript programming.For a render() function, React binds this to the component for us. React specifies a default set of special API methods. render() is one such method. constructor() is a special function in a JS class. JS invokes constructor() whenever an object is created via a class. If you've never worked 
with an Object Oriented language before, it's sufficient to know that React invokes constructor() first thing when initializing our component. React invokes constructor() with the component's props.

The constructor() function defined by React.component will bind this inside our constructor() to the component. Because of this, it's a good practice to always call super() first whenever you declare a constructor() for your component. After calling super(),we call bind() on our custom component method.

### Component
A component should ideally only be responsible for one piece of functionality.



```javascript
import React from "react";
import ReactDOM from "react-dom";

function HelloWorld() {
  return <div>Hello World!</div>;
}

ReactDOM.render(<HelloWorld />, document.querySelector("#root"));
```

React works differently than many earlier front-end JS frameworks in that instead of working with the browser's DOM, it builds virtual representation
of the DOM. By virtual, we mean a tree of JS objects that represent the "actual DOM".

We do not directly manipulate the actual DOM. Instead, we must manipulate the virtual representation and let React take care of changing the browser's DOM.
When using the virtual DOM, we code as it we are recreating the entire DOM on every update.

Instead of the developer keeping track of all DOM state changes, the developer simply returns the DOM they wish to see. React takes care of the transformation
behind the scenes. React's virtual DOM is a tree of ReactElements. A ReactElement is a representation of the DOM element in the virtual DOM.

JSX is a syntactic sugar to call React.CreateElement. JSX is going to parse the tags we write and then create JS objects. JSX is a convenience syntax to help build the component tree. ReactElement is stateless and immutable. If we want to add interactivity , we need another piece of the puzzle: ReactComponent.

JSX is compiled down to JS by Babel.

### Two approaches to Building a react app

1. Top-down: Start at the top. Build the Tweet Component first and then
   it's children

2. Bottom-up: Start at the bottom(the leaves of the tree). Build Avatar, then name with handle, then the rest of the child components. Verify that they work in isolation. Once they are all done, assemble them into the Tweet.

### Deep Dive

A ReactCompponent is a JS object that at minimum has a render() function which is expected to return a ReactElement.

render() returns a ReactElement tree. After the component is mounted and initalized, render() willl be called. The render() function's job is to provide React
a virtual representation of the native DOM component.

## Communication with Parent components

If you can't change props but you need
to communicate something up to a parent
component, how does that worK?

If a child needs to send data to it's parent, the parent
can send down a function as a prop like this:

```javascript
function handleAction(event) {
  console.log("Child did:", event);
}

function Parent() {
  return <Child onAction={handleAction} />;
}
```

You can validate that an object has a certain shape, meaning that it has particular properties. The object passed to this
prop is allowed to have extra properties too, but it must at least have the ones in the shape description.

```javascript
PropTypes.shape({
  name: PropTypes.string,
  age: PropTypes.number
});
```

This will match an object of the exact shape like this:

```javascript
person = {
  name: "Joe",
  age: 27
};
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
function IconButton({ children }) {
  return (
    <button>
      <i class="target-icon" />
      {children}
    </button>
  );
}
```

### Different types of Children

The children prop is always pluralized as children no matter whether there's a single child or multiple children. When there are multiple children, children is an **_array of Reactelements_**. When there is only on child, it is a **_ReactElement object_**.

```javascript
<Nav>
  <NavItem url="/">Home</NavItem>
  <NavItem url="/about">About</NavItem>
  <NavItem url="/contact">Contact</NavItem>
</Nav>;

function Nav({ children }) {
  let items = React.children.toArray(children);
  for (let i = items.length - 1; i >= 1; i--) {
    items.splice(
      i,
      0,
      <span key={i} className="seperator">
        |
      </span>
    );
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

**_Every instance of a component has it's own state._**

### setState is Asynchronous

The setState function is actually asynchronous. If you setState and act on it immediately, it is likely to print
the old state instead of the one I just set. We can use a callback to perform the above action.

```javascript
this.setState({ name: "Gideon" }, function() {});
```

### Functional setState

Another way to make it so that sequential state updates
run in sequence is to use the functional form of setState
like this:

```javascript
this.setState((state, props) => {
  return {
    value: state.value + 1
  };
});
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
class RefInput extends React.Component {
  showValue = () => {
    alert(`Input containers: ${this.input.value}`);
  };

  render() {
    return (
      <div>
        <input type="text" ref={input => (this.input = input)} />
        <button onClick={this.showValue}>Alert the value!</button>
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

1. **_Mount_** occurs when the component is first added to the DOM. Initialization and setup are done here.
2. **_Render_** occurs when the component renders for the first time, and whenever it re-renders due to change in props
   or state
3. **_Commit_** takes the output from render and updates the DOM to match
4. **_Unmount_** happens when the component is being removed from the DOM.

### Mounting

**_Constructor_**: First method called, when the component is created. If state is initalized with a property intializer then it will already be set by the time the constructor execute.

**_componentDidMount_**: Called immediately after the first render. The component's children are already rendered at the point too. This is a good place to make AJAX requests to fetch any data you need.

**_static getDerviedStateFromProps(nextProps,prevState)_**: This is an opportunity to change the state based on the value of props which can be used for intialization. This method must not have side effects so don't call setState here.

**_shouldComponentUpdate(nextProps,nextState)_**: This is an opportunity to prevent rendering if you know that props and state have not changed. The default implementations always return true. If you return false, the render will not occur.

**_getSnapshotBeforeUpdate(nextProps,prevState)_**: Called after
Render but before changes are committed to the DOM.
If you need to do any calculations based on the old DOM, this is the time to do it. Return anything you want from this function and the value will get passed as the third argument to componentDidUpdate

**_componentDidUpdate(prevProps,prevState,snapshot)_**: Render is done. DOM changes have been committed. You can use this opportunity to operator on the DOM if you need to. Prior to this, DOM nodes would still be in Flux.

### Unmounting:

**_componentWillUnmount_**:The component is about to be unmounted. Maybe it's item was removed from a list, maybe the user navigated to another tab.

### state

Defining state on our component requires us to set an instance variable called this.state in the object.prototype class. In order to do this, it requires
us to set the state in one of two places, either as a property of the class or in the constructor.

Spreading state througout our app can make it difficult to reason about. When building stateful components, we should be mindful about what we put in state and
why we are using state.

### Talking to Children Components with props.children

React provides some special props for us. In our components, we can refer to child components in the tree using props.children.

```javascript
const NewsPaper = props => {
  return (
    <Container>
      <Article headline="An interesting read">Content Here </Article>
    </Container>
  );
};
```

The container above contains a single child, the Article component and the article component contains a single child the text Content here.
In the Container component, say that we want to add markup around whatever the Article Component renders. To do this, we write our JSX in the Container Component and then place this,props,children.

```typescript
class Container extends React.Component{
    render(){
        return <div className="container">{this.props.children}</div>s
    }
}


```

Let's rewrite the previous Container to allow a configurable wrapper component for each child.

1. A prop component which is going to wrap each child
2. A prop children which is list of children we are going to wrap.

### Routing in React

A URL is a reference to a web resource. A typical URL looks something like this.

```javascript

http://www.example.com/index.html

```

1. Browser makes a request to the server of this page
2. The server uses the identifiers in the URL to retrieve data about the artist and the album from the database.
3. The server populates a template with this data
4. The server returns this poplulated HTML document along with any other assets like CSS and images.
5. The browser renders these assets

With React,

1. Browser makes a request to the server
2. The server doesn't care about the pathname. Instead it returns a standard index.html that includes the React app and any static assets
3. The React app mounts
4. The React app extracts the identifiers from the URL and uses the identifiers to make an api call to fetch the data for the artist and the album. It might
   takes this call to the same server. The React app renders the page using data it received from the API call.

With React Routing this would look like this,

1. User visits https://example.com/artists/87589/albums/1758221
2. The server delivers the standard index.html that includes the React app and assets
3. The React app mounts and populates itself by making an API call to the server
4. User clicks on the Account button. The React app re-renders, checks the url. It sees the user is viewing /AccountView and it swaps in the Account View Component.
5. The React app makes an API call to populate the AccountView Component.

### Building Router

Our Basic Version of Router should do two things:

1. Supply its children with context for both location and history
2. Re-renders the app whenever the history changes

Regarding the first requirement, at the moment our Route and Link Components are using
two APIs directly. Route uses window.location to read the location and the Link uses history to modify the location. Redirect will need to access the same APIs. The Router
supplied by react-router makes these APIs available .

We just saw that Route invokes a function passed as a render prop
with the argument match. Route also sets this prop on components rendered via the component prop. Regardless of how Route renders its component, it will always set three props:

1. match
2. location
3. history

The match object contains the following properties.

1. params- (object) key/value pairs from the URL corresponding to the dynamic segments of the path
2. isExact- true of the entire URL was matched
3. path- (string) The path pattern used to match.
4. url- (string) The matched portion of the URL. Useful for building nested <Link>s.

### Redirect state

If a user visits a page on the site they can't access because they are not logged in, we redirect them to /login. When they log in, we should send them back to whichever page they came from. In order to do this, Login needs some way to know where the user came from.

### What is Dynamic Routing?

Routing that takes place as your app is rendering,

### Ref's in React

When we define a ref attribute on the input elemenet and use a function , for its value, Reac will execute that function when the input element gets mounted with the Component. React will also pass a reference to the DOM input element as an argument to the ref function. Inside the ref function we can access the Component instance via the this keyword so when we can store in input reference as an instance variable.


Every React Component has a story.

Here's how its story starts


```javascript

class Quote extends React.Component{
render(){
return(
<div className="quote-container">
 <div className="quote-body">{this.props.body}</div>
 <div className="quote-author-name">{this.props.authorName}</div>
</div>
)
}
}

```

Our quote container story continues, the next major event in it's history is when we instanitate it. This is when we tell the component class to generate a copy from the template to represent the actual quote data object.


```html
<Quote body="..." authorName="..."/>

```

The instantiated <Quote/> element is now full-term and ready to be born. We can render it somewhere in the browser.





