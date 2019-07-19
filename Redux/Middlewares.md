### Basic Idea

A web server can be seen as a function that takes in a request and outputs a response. Middlewares are functions executed in the middle after the incoming
request then produces an output which could be the final output passed or could be used by the next middleware until the cycle is completed.
















Attempt #1: Logging Manually

```javascript

store.dispatch(addTodo('Use Redux'))

const action=addTodo('Use Redux');

console.log('dispatching',action)
store.dispatch(action);
console.log('next state',store.getState())

```



Attempt #2: Wrapping Dispatch

You can extract logging into a function

```javascript

function dispatchAndLog(store,action){
    console.log('dispatching',action)
    store.dispatch(action)

    console.log('next state',store.getState())
}

dispatchAndLog(store,addTodo('Use Redux'))

```

Attempt #3 MonkeyPatching Dispatch







