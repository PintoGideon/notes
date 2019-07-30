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

### Refresher on state architecture patterns

### Higher Order Components

Generate the ability to take a container and wrap other components in it creating a container factory.

```javascript

const withCount=function(WrappedComponent){
    return {
        class extends Component{
            state={count:9};
            increment=()=>{
                this.setState(state=>({count:state.count+1}))
            }
            render(){
                <WrappedComponent count={this.state.count} onIncrement={this.increment} {...this.props}/>
            }
        }
    }
}


const CounterWithCount=withCount(Counter)

```

### Render Properties

One issue with the higher order component is that we might pass data as props to components which might not need it. For example, a card
assignment card would not need a onCreateUser and onUpdateUser functionalities.

```javascript
<withUsers>{({ users, createUser }) => <User key={user.id} />}</withUsers>
```

### Flux Implementation

Flux is a design pattern,not a specific library of implementation.

### The Redux workflow

The views dispatch actions to the store. When the store receives an action from the views, the store uses a reducer function to process the action. The store provides the reducer function with the current state and the action.

Consider a store with a current state of 5. The store receives an increment action. The store uses its reducer to derive the next state.

### The counter's actions

```javascript
function reducer(state, action) {
  if (action.type === "INCREMENT") {
    return state + 1;
  }
}
```

### Building the store

We have been calling our reducer and manually supplying the last version of the state along with an action.

The redux library provides a function for creating stores, createStore(). The function returns a store object that keeps an internal variable state. In additiion, it provides a few methods for interacting with the store.

```javascript

const createStore=(reducer)=>{
const state=0;

const getState=()=>{
    return state;
}

const dispatch=(action)=>{
    reducer(state,action)
}

return {
    getstate,
    dispatch
}

}
}
```
### The core of Redux

As it stands out, our createStore() function closely resembles the createStore() function that ships with the Redux library.

- All the application's data is in a single data structure called the state which is held in the store

- Your app reads the state from this store

- the state is never mutated directly outside the sotre.

- The views emit actions that describe what happened

- A new state is created by combining the old state and the action by a function called the reducer.


### Subscribing to the store

Our store so far provides methods for the view to dispatch actions
and to read the current version of the state.

While the view can read the state at any time with getState(), the view needs to know when the state has cnahged.

Now state is being modified outside of React and inside of the store.  our views are unaware of when it changes. If we are going to keep our views up to date with most current state in the store, the our views should receive a notification.



