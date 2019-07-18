### State

It may not be immediately clear which component show own
what state.
State is created in the component and stays in the component. It can be passed to a children as it's props.

- **_Model state_**: This is likely the data in your application. This could be the items in a given list.
- **_Ephemeral State_**: Stuff like the value of an input field that will be wiped away when you hit enter.

### this.setState() is asynchronous

 You're queuing up state changes. React will
batch them, figure out the result and efficiently make that
change.

### There is actually a bit more to this.setState();

We could pass in a function as an argument

```javascript
increment(){
    this.setState((state)=>{return {count:state.count+1}});

}
```

```javascript

increment(){
    this.setState(state=>{
        if(state.count>=5) return;
        return {count:state.count+1}
    })
}

```

### this.setState also takes a callback

### Patterns and Anti-Patterns

- Don't use this.state for derivations of props
- Don't use state for things you aren't going to render

### The Peris of Prop Drilling

### Higher order components

We can take a container and wrap a component in it and pass out a new component with the state.

### Render props

### A simple example

```javascript

export default class withCount extends Component{
state={count:0}

increment=()=>{
this.setState(state=>({count:state.count+1});
}

render(){
return (
<div className="withCount">
{
this.props.render(this.state.count, this.increment)
}
</div>
)
}
}

```

```javascript
<WithCount
	render={(count, increment) => (
		<Counter count={count} onIncrement={increment} />
	)}
/>
```

### State Architecture Patterns

1. Seperating out a presentation component from a state component

2. Identify every component that renders something based on the state

3. Find a common owner component ( a single component above all the components that need the state in the hierarchy)

### Context API

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

```

```
