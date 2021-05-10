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

### React Performance


React in itself is pretty fast. The abstraction we build is unlikely to be as fast as the abstraction built into React.

### Code Splitting

Network ---> Command + Shift + P ----> Show Coverage


How to monitor performance ?

Code Splitting acts on the principle that loading less code will speed up your app. 

```javascript

import('/some-module.js').then(module=>{
  // Do stuff with module exports
},
error => {
  // Module could not be loaded, React out the developer please
}
)
```

React has built in support for loading modules as React components.
The module must have a React component as the default export, and 
you have to use the `<React.Suspense>` component to render a fallback
value while the user waits for the module to be loaded.

```javascript

import * as React from 'react';

function SmileyFace(){
  <div></div>
}
export default SmileyFace;



function App(){
  return(
    <div>
       <React.Suspense fallback={<div>Loading....</div>}>
       <SmileyFace/>
       <React.Suspense>
    </div>
  )
}

```

1. Use Incognito mode so the browser plugins don't mess with the typical user experience
2. Run `npm run build` and then `npm run serve`
3. Play with the coverage feature of the dev tools


We can go to the react dev tools, click on the suspense component and suspend it with the stop watch to see the fallback ui.



### Eager Loading

So it's great that the users can get the app loaded faster, but it's annoying
when 99% of the time the reason the users are using the app is so they can
interact with our textarea. We don't want to have to make them wait first to
load the app and then again to load the globe. Wouldn't it be cool if we could
have globe start loading as soon as the user hovers over the checkbox? So if
they `mouseOver` or `focus` the `<label>` for the checkbox, we should kick off a
dynamic import for the globe module.


### Webpack magic comments

If you're using webpack to bundle your application, then you can use webpack magic comments, to have webpack instruct the browser to prefetch dynamic imports.

With this, the browser will automatically load this JS file into the browser cache so it's ready ahead of time.

Select any component in the dev tools, click the stopwatch to see the how the Suspense boundary is going to handle your component and how the fallback UI is going to look like.

We can also load npm dependencies lazily.

```javascript

const Globe=React.lazy(()=>import(/* webpackPrefetch:true */) 
'../globe'
)

function App(){
  const [showGlobe, setShowGlobe]= React.useState(false)
  

  return(
    <div>
       <div style={{width:400, height:400}}>
        <React.Suspense fallback={<div>Loading Globe </div>}>
        {showGlobe ? <Globe/>:null}
        </React.Suspense>
       </div>
    </div>
  )

}

```


### Performance Profiling

Click on the performance tab, record the activity.
You will see a flame graph. Click shift and scroll to navigate horizontally or vertically.


### useMemoize

Calculations performed within render will be performed every single render, regardless of whether the inputs for the calculations change.

```jsx

function Distance(x,y){
  const distance= calculateDistance(x,y)
  return(
    <div>
       The distance between {x} and {y} is {distance}
    </div>
  )
}

```

If that component's parent re-renders, or if we add some unrelated state
to the component and trigger a re-render, we will be calling 
`calculateDistance` every render which could lead to a performance bottleneck.

```jsx
function Distance({x, y}) {
  const distance = React.useMemo(() => calculateDistance(x, y), [x, y])
  return (
    <div>
      The distance between {x} and {y} is {distance}.
    </div>
  )
}
```




### Use web workers

Working with web-workers is asynchronous. 
Javascript is single threaded. Because of that if the rendering takes longer than 16s , the browser just locks up and results into a jenky experience
We can use a web worker provided by web worker called workerize. The communication between the web worker and the main thread is going to be asynchronous.


### Typical life cycle of a react app
render -> reconciliation -> commit -> state change

A React component can re-render for any of the following
reasons:

1. Props change
2. Internal State Change
3. Consuming context value have changed
4. Parent re-renders.

Fix a slow render than fixing an unnecessary render. In the profiler's flame graph, the different colors means that the component was updated.


```jsx
function CountButton({count, onClick}) {
  return <button onClick={onClick}>{count}</button>
}

function NameInput({name, onNameChange}) {
  return (
    <label>
      Name: <input value={name} onChange={e => onNameChange(e.target.value)} />
    </label>
  )
}

function Example() {
  const [name, setName] = React.useState('')
  const [count, setCount] = React.useState(0)
  const increment = () => setCount(c => c + 1)
  return (
    <div>
      <div>
        <CountButton count={count} onClick={increment} />
      </div>
      <div>
        <NameInput name={name} onNameChange={setName} />
      </div>
      {name ? <div>{`${name}'s favorite number is ${count}`}</div> : null}
    </div>
  )
}
```

Based on how this is implemented, when you click on the counter button, the <CountButton/> re-renders (so we can update the count value).

But the <NameInput/> is also re-rendered. React does this because it has no way of knowing whether the NameInput will need to return different React Components based on the state change of the parent.

`React.PureComponent` is for class components and `React.memo` is for function components.

We won't get re-rendering because we return true

```javascript

ListItem = React.Memo(ListItem, (prevProps, nextProps)=>{
  return true;
})

```
If you have a particular component with many changes to the prop, lift those calculations to the parent and pass the values to the 
child component, giving you an opportunity to optimize the values.


### react-virtual

Window Large Lists with react-virtual.

We often don't need to actually display tends of thousands of list items, table cells or data points to users. So if that content isn't displayed, you can kinda cheat by doing some 'lazy' just in-time rendering.

Let's stay you have a grid of data that rendered 100 columns and had 5000 rows. Do you really need to render 
all 500000 cells for the user all at once. 

We can only display a window of 10 columns by 20 rows and the rest you can delay rendering until the user starts scrolling around the grid.

```jsx

import {useVirtual} from 'react-virtual';

function MyListOfData({items}){
  const listRef=React.useRef();
  const rowVirtualizer=useVirtual({
    size:items.length,
    parentRef:listRef,
    estimateSize:React.useCallback(()=>20,[]),
    overscan:10
  })

  return(
    <ul ref={listRef} style={{display: 'relative', height: 300}}>
      <li style={{height: rowVirtualizer.totalSize}} />
      {rowVirtualizer.virtualItems.map(({index, size, start}) => {
        const item = items[index]
        return (
          <li
            key={item.id}
            style={{
              position: 'absolute',
              top: 0,
              left: 0,
              width: '100%',
              height: size,
              transform: `translateY(${start}px)`,
            }}
          >
            {item.name}
          </li>
        )
      })}
    </ul>

  )
}


```

We can simply tell `useVirtual` how many rows are there in our list, give it a callback that it can use to determine size, it will give us back `virtualItems`
and a `totalSize` which we can then use to only render the items the user should be able to see within the window.


### All the optimizations that can be done on large lists.

The way that context works is that whenever the provided value changes from one render to another, it triggers a re-render of all the consuming components
(which will re-render whether or not they're memoized)

```jsx

const CountContext=React.createContext()

function CountProvider(props){
  const [count,setCount]=React.useState(0)
  const value=[count,setCount]
  return <CountContext.Provider value={value} {...props}/>
}


```

Every time the `<CountProvider/>` is re-rendered , the 
value is brand new so even though the `count` value
itself may stay the same, all component consumers will
be re-rendered.




```javascript

function AppProvider({children}){
  const [state, dispatch] = React.useReducer(appReducer, {
    dogname:'',
    grid:initialGrid
  })

  const value = React.useMemo(()=>[state,dispatch],[state,dispatch]);
  return (
    <AppContext.Provider value={value}>
      {children}
    </AppContext.Provider>
  )
}

```

OR

```javascript


function AppProvider({children}) {
  const [state, dispatch] = React.useReducer(appReducer, {
    dogName: '',
    grid: initialGrid,
  })
  return (
    <AppStateContext.Provider value={state}>
      <AppDispatchContext.Provider value={dispatch}>
        {children}
      </AppDispatchContext.Provider>
    </AppStateContext.Provider>
  )
}

function useAppState() {
  const context = React.useContext(AppStateContext)
  if (!context) {
    throw new Error('useAppState must be used within the AppProvider')
  }
  return context
}

function useAppDispatch() {
  const context = React.useContext(AppDispatchContext)
  if (!context) {
    throw new Error('useAppDispatch must be used within the AppProvider')
  }
  return context
}

// Grid only renders when dispatch changes, as opposed to before with a single provider, where it would re-render on a state change.

function Grid() {
  const dispatch = useAppDispatch()
  const [rows, setRows] = useDebouncedState(50)
  const [columns, setColumns] = useDebouncedState(50)
  const updateGridData = () => dispatch({type: 'UPDATE_GRID'})
  return (
    <AppGrid
      onUpdateGrid={updateGridData}
      rows={rows}
      handleRowsChange={setRows}
      columns={columns}
      handleColumnsChange={setColumns}
      Cell={Cell}
    />
  )
}
Grid = React.memo(Grid)

```



###

Staying interactive === not blocking the main thread.

Typical Runtime Performance Issues

1. Computationally expensive tasks
2. Animations and graphical updates
3. Reflow/ Repaint/ Layout
4. Parsing
5. Memory Leaks


How do you know you have performance issues?

1. Best Case: Dropped Frames and input delay
2. Worst case: Dropped frames

How do we fix it?

Performance isn't really prescriptive. You'll have to get clever. A lot of the time it's a problem you have created.
Semi Prescriptive things we can do to improve runtime performance.

1. Use Regular for loops
2. Use lookup tables
3. Avoid creating new objects
4. Turn off animation
5. Avoid reflow/layouts
6. Caching
7. Sneaky tricks (set object index to undefined instead of using the delete keyword)
8. Use immer

### Learn how to use Dev Tools


Using the user timings api.

```javascript




```



















