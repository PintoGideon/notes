Whenever you change the state of a React Comonent, it triggers the reactive re-rendering process. React will construct a new virtual DOM representing your application's State UI and perform a diff with the current virtual DOM to work on what DOM elements should be mutated, added or removed.

### Batching

In React, whenever you call setState on a component, instead of updating it immediately React will only mark it as dirty. React use an event loop to render changes in batch.

An event loop is a JS process that keeps on running indefinitely , constantly distributing data around , checking for all the event handlers and lifecycle methods that need to be invoked. By Batching the reconciliation process, the DOM is updated only once per event lopp which is key to building a performant application.

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


