### State

It may not be immediately clear which component show own
what state.
State is created in the component and stays in the component. It can be passed to a children as it's props.

**_Model state_**: This is likely the data in your application. This could be the items in a given list
**_Ephemeral State_**: Stuff like the value of an input field that will be wiped away when you hit enter.

### this.setState() is asynchronous

Effective, you're queuing up state changes. React will
batch them, figure out the result and efficiently make that
change.

### There is actually a bit more to this.setState();

We could pass in a function as an argument

```javascript
increment(){
    this.setState((state)=>{return {count:state.count+1}});

}
```































```
