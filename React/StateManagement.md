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
    state=reducer(state,action)
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

Now state is being modified outside of React and inside of the store. our views are unaware of when it changes. If we are going to keep our views up to date with most current state in the store, the our views should receive a notification.

The store uses the observer patter to allow the views to immediately update when the state changes. The views will register a clalback function that they would like to be invoked when the state changes. The store will keep a list of all these callback functions. When the state changes, the store will invoke each function notifying the listeners of the change.

Inside createStore() , we will

1. Define an array called listeners
2. Add a subscriber() method which adds a new listener to listeners
3. Call each listener function when the state is changed.

```javascript
function createStore(reducer, initalState) {
  let state = initialState;
  const listeners = [];
}
```

```javascript
const subscribe = listener => {
  listeners.push(listener);
};
```

The listener argument of subscribe() is a function. the function that the view would like invoked whenever the state changes. To make subscribe accessible, we need to expose subscribe by adding it to the store object returned by createStore();

### createStore() in full.

```javascript

function createStore(initialState, reducer){
    let state=intialState;
    const listeners=[];

    const subscribe=(listener)=>{
        listeners.push(listener)
    }

    const getState=()=>(state);

    const dispatch=(action)=>{
        state=reducer(state,action);
        listeners.forEach(listener=>return{
            l();
        })
    }

    return {
        subscribe,
        getState,
        dispatch
    }
}
```

### Connection Redux to React

### Using sore.dispatch().

```javascript
class Message extends React.Component {
  handleDeleteClick = () => {
    store.dispatch({
      type: "DELETE_MESSAGE",
      index: this.props.index
    });
  };
}
```

The dispatch() call will modify the state.dispatch() which will then invoke the listener which we registered with subscribe().This forces the App component to re-render. When render() is invoked, the App component reads from this store again with getState() and then passes the latest version of the state down to it's child components.

### Breaking up the reducer function

With reducer composition, we can break up the state management logic of our app into smaller functions.

We will still pass in a single reducer function to createStore(). But this top level function will call one or more functions.

### A new reducer()

We'll still call our top-level function reducer(). We'll have two other reducer function each managing their part of the state's top level properties.

When reducer() receives a state of undefined , state is set to {}.

### Using Presentational and Container Components with Redux.

Container Components have similar behavior:

1. They subscribe to the store in componentDidMount
2. They might have some Logic to massage Data from state into a format fit as a prop for the presentational component
3. The map actions on the presentational component such as functions that dispatch to the store.

### connect()

The connect() function in react-redux generates container components. For each presentational component, we can write function that specify how state should map to props and how events should map to dispatches.

### The Provider Component

Before we can use connect() to generate container components, we need to make a small addition to our app.

In order for connect() to be able to generate container components, it needs some canonical mechanism for containers to access the redux store. The `react-redux` library supplies a special Provider component. You can wrap your top level component in Provider. Provuder will then make the store available to all components via React's context feature.

When we use connect() to generate container components, those container components will assume that the store is available to them via context.

```javascript
const WrappedApp=()=>{
return (<Provider store={store}><App/><Provider>);
}
```
