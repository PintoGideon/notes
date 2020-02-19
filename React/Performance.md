Whenever you change the state of a React Comonent, it triggers the reactive re-rendering process. React will construct a new virtual DOM representing your application's State UI and perform a diff with the current virtual DOM to work on what DOM elements should be mutated, added or removed.

### Batching

In React, whenever you call setState on a component, instead of updating it immediately React will only mark it as dirty. React use an event loop to render changes in batch.

An event loop is a JS process that keeps on running indefinitely , constantly distributing data around , checking for all the event handlers and lifecycle methods that need to be invoked. By Batching the reconciliation process, the DOM is updated only once per event lopp which is key to building a performant application.

React's tree reconciliation process uses the order of the children in the diffing alogrithm. This unfortunately means that if a child is removed from the tree, all siblings after it will need to updated in the process. If children are driven by a dynamic array that can shifted and unshifted, it can cause many renders. To force React to respect the identity and state of each child in a list, we need to uniquely identify each child instance by assigning a key prop.

### Sub-tree rendering

When the event loop ends, React re-renders the dirty components as well as their children. All the nested components, even if they didn't change will have their render method called.

This may sound inefficient but in practice it is actually very fast because React is not touching the actual DOM. All this happens in the in-memory virtual DOM.

React provides a way to fine-tune this process and prevent sub trees from re-rendering with a lifecycle method called shouldComponentUpdate. Before re-rendering a child component, React will always invoke its shouldComponentUpdate method.

```
npm install --save react-addons-perf
```

```javascript
import Perf from 'react-addons-perf'

class App extends React.Component(){
    Perf.start(),
    render(<App/>,document.getElementById('root'))
    Perf.stop();
    Perf.pringInclusive();
    Perf.printWasted();
}

```

### Detour into Component Lifecycle

- Mounting

componentDidMount() is invoked once immediately after the intial rendering occurs. At this point in the lifecycle, the component has a DOM representation that can be accessed.

- Props Changes

shouldComponentUpdate() is a special function called before the render function and it gives the opportunity to define if a re-rendering is needed or can be skipped?

componentWillUpdate() Invoked immediately before rendering when new props or state are being received. Any state changes via this.setState are not allowed as this function should be strictly used to prepare for an upcoming update and not trigger an update itself.

componendDidUpdate() is invoked immediately after the comonent's updates are flushed to the DOM.

### Scheduling is the future of React

Speed alone doesn't define the user experience.

Web Apps are often bount by network speed rather than CPU speed.

### Collections

Collections do not get downloaded. You got to ask for the collection.
https://medium.com/@baphemot/understanding-react-suspense-1c73b4b0b1e6

```javascript
const cache = {
  [file.id]: file
};
const pendingCache = {};

// Read from the cache first

// Set the id anf the file if not read from a cache

let pending = pending[file.id];

// Check if you have a promise sitting in the pending cache

const promise = (pending || pending[file.id] = await filelist.getItems());

promise.then(files => {
  cache[file.id] = file;
});
```

### Prevent React setState on unmounted Component

**_Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op._**

This warnng usually shows up when this.setState() is called in a component even though the component got already unmounted. The unmounting can happen for different cases.

1. You don't render a component anymore due to React's conditional rendering
2. You navigate away from a component by using a library such as react router.

It can still happen that this.setState() is called if you have done async logic in your component and updated the local state of the component afterward.

3. You made an asynchronous request to an API, the request (e.g. Promise) isn't resolved yet, but you unmount the component. Then the request resolves, this.setState() is called to set the new state, but it hits an unmounted componen

4. You have a listener in your component, but didn't remove it on componentWillUnmount(). Then the listener may be triggered when the component unmounted

```javascript

class News extends Component {
  _isMounted = false;
  constructor(props) {
    super(props);
    this.state = {
      news: [],
    };
  }
  componentDidMount() {
    this._isMounted = true;
    axios
      .get('https://hn.algolia.com/api/v1/search?query=react')
      .then(result => {
        if (this._isMounted) {
          this.setState({
            news: result.data.hits,
          });
        }
      });
  }
  componentWillUnmount() {
    this._isMounted = false;
  }
  render() {
    ...
  }
}

```

### Race Conditions

1. Redux Middleware

```javascript
const apiMiddleware = ({ dispatch }) => next => action => {
  next(action);
};

or;

const test = function apiMiddleware({ dispatch }) {
  return function(next) {
    return function(action) {
      next(action);
    };
  };
};
```


### Using the React Profiler 


1. Reading Performance Data

React works in 2 phases:

a. The render phase determines what changes need to be made. During this phase , React calls render and then compares the result
to the previous render.

b. The commit phase is when React applies any changes. React also calls lifecycles like componentDidMount and componentDidUpdate during this phase.


The Dev tools profiles groups performance info by commit.

