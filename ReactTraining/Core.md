### Render Props

The following code is just a rough template for render props.

```jsx
class App extends React.Component {
  state = {};

  render() {
    <Component>
      {(props) => {
        return <div>props.renderProp</div>;
      }}
    </Component>;
  }
}

class Component extends React.Component {
  state = {};

  render() {
    this.props.children(error, coordinates);
  }
}
```

### Render Optimizations

If you component is a pure function of the state or props,
we could use PureComponent.

1. shouldComponentUpdate lifecycle hook or PureComponent.

### useState hook demystified

```javascript
const states = [];
let callCount = -1;

function useState(initialValue) {
  const id = ++callCount; //0
  if(states[id]){
    return states[id] // after re-rendering, [1,fn]
  }
  const setValue = (newValue) => {
    // assign a new Value;
    states[id][0] = newValue; //states[0][0]=1
    renderPhonyHooks();
  };
  const tuple = [initialValue, setValue];   //[0, fn]
  states.push(tuple); //state=[[0,fn]]
  return tuple;
}

function Minutes(){
  const [test,setTest]=useState(0)

  render(){
    return(
      <button onClick={()=>setTest(1)}></button>
    )
  }
}

function renderPhonyHooks(){
  callCount=-1;
  ReactDOM.render(<Minutes/>,document.getElementById('root))
}
renderPhonyHooks();
```

### useRef()

This is just an example to demonstrate the syntax of refs.

```jsx
const [message, setMessage] = useState(null);

const messageLengthRef = useRef();
const span = messageLengthRef.current;
span.innerText = message.length;

<textarea value={message} onChange={this.handleMessageChange} />;

<div>
  Character Count: <span ref={messageLengthRef}>/{Max_Message_Length}</span>
</div>;
```

### useEffect

```javascript
useEffect(() => {
  // Whenever the message changes, I want the message count
  // to update
  const span = messageLengthRef.current;
  span.innerText = message.length;
}, [message]);
```

`useEffect` is called at the first render and whenever the message changes,

A really use case is also when the effect is supposed to happen but also when the effect is not supposed to happen.

```javascript
const firstTwenty = message.substr(0, 20);

useEffect(() => {
  document.title = "New Post" + (firstTwenty ? `:${firstTwenty}` : "");
}, [firstTwenty]);
```

### Fetching data using hooks

```javascript
useEffect(() => {
  let isCurrent = true;

  async () => {
    const user = await fetchUser(uid);
    if (isCurrent) setUser(user);
  };

  return () => {
    isCurrent = false;
  };
}, [uid]);
```

useEffect is designed to synchronize every committed render to your own effects.

With data loading, we can use [] for the first time, but if the uid changes, we're still rendered. We could be showing the wrong avatar.

We put the uid in there to make sure we always display the right thing. Make sure we 'synchronize' our rendered elements.

### Context in React

context can be thougt of as a communication channel.

```jsx
<Tabs>
  <Tab>
    <Signup />
  </Tab>
  <Tab>
    <Login />
  </Tab>
</Tabs>
```

```jsx
const TabsContext = createContext();
const TabListContext = createContext();

function TabList({ children }) {
  return (
    <div data-react-tab-list>
      {children.map((child, index) => (
        <TabListContext.Provider value={{ index }}>
          {child}
        </TabListContext.Provider>
      ))}
    </div>
  );
}

function Tabs2({ children }) {
  const [activeIndex, setActiveIndex] = useState(0);
  return (
    <TabsContext.Provider value={{ activeIndex, setActiveIndex }}>
      <div data-react-tabs>{children}</div>
    </TabsContext.Provider>
  );
}

function Tab({ children, disabled }) {
  const { index } = useContext(TabsListContext);
  const { activeIndex, setActiveIndex } = useContext(TabsContext);
  //....
}

function TabPanels({ children }) {
  const { activeIndex } = useContext(TabsContext);
}
```

### children module

### useReducer

```javascript
const array = [1, 2, 3, 4, 5];
const add = ((x, y) = x + y);

const sum = array.reduce(add, 0);
// You can start the reducer at any value you want apart from 0.
```

### useReducer and useContext for global state.

```jsx
// App state.js

const Context = createContext();

export function AppStateProvider({ reducer, initialState = {}, children }) {
  const value = useReducer(reducer, initialState);
  return <Context.Provider value={value} children={children} />;
}

export function useAppState() {
  return useContext(Context);
}
```

```javascript
// Reducer

const appStateReducer = (state, action) => {
  switch (action.tyoe) {
    case "AUTH_CHANGE": {
      return { ...state, auth: action.auth, authAttempted: true };
    }
    case "LOAD_USER": {
      return { ...state, user: action.user };
    }
    default:
      return state;
  }
};

export default appStateReducer;
```

```jsx
// useAuth
export default function useAuth() {
 const [{ authAttempted, auth }, dispatch] = useAppState()
useEffect(() => {
  if (!authAttempted) {
      return onAuthStateChanged(auth => {
        dispatch({type: "AUTH_CHANGE",
          auth: auth
        })
      })
    }
  }, [authAttempted, dispatch])

  return { auth, authAttempted }
}
}


```

```jsx
// In app.js

function App() {
  const { auth, authAttempted } = useAuth();

  if (!authAttemped) {
    return <div className="Layout">{auth ? <LoggedIn /> : <LoggedOut />}</div>;
  }
}

export default () => (
  <AppStateProvider reducer={appReducer} initialState={initialState}>
    <App />
  </AppStateProvider>
);
```

### Three types of element animations:

1. Interpolating values on an element:
   width, opacity, position etc.

2. Enter/Exit an element

3. Transitioning between elements

### useTransition from react-spring

### Performance optimizations

```jsx
const el = Dashboard();
renderReactElementToDOM(el, rootDOMElement);
```

### second render upon state change

```jsx
const newElement = Dashboard();
const oldel = el;
const diff = compare(newElement, oldel); // pretty much always fast

commit(diff);
```

When we talk about performance, we talk about one of these things being slow.

Calling a component could be slow if the algorithm in your code is slow.

One small hack would be to do

```javascript
console.time("Test the time of the below function");
const weeks = calculateWeeks(posts, startDate, numWeeks);
console.timeEnd("Test time");
```

`useMemo` is going to allow use to memoize the value.
Maybe we don't want to do the calculation upon every state change.

```javascript
const weeks = useMemo(() => {
  calculateWeeks(posts, startDate, numWeeks);
}, [posts, startDate, numWeeks]);
```

### Optimizing the diffing process

```javascript
const diff = compare(newElement, oldEl);
```

React also has a way to help us identify the components that needs to be optimized.

```javascript
import React, {memo} using 'react'
```

```javascript
// This is what memo does.
if (propsChange(newElement.props, oldEl.props)) {
  const diff = compare(newElement, oldEl);
  commit(diff);
}
```

We can use `memo` which is a higher order component.
Diffing the props is probably going to be faster than diffing the element tree.

If the props doesn't change, dont render the component.

```javascript
{}==={} //false
'string'==='string'
```

What does that mean for comparing props?

Everytime you compare props with `posts` for example from a fetch request, the component still re-renders as their object identity is different.

### Important patterns to remember

If you pass a function to useState, everytime there is an update within the component that calls the hook, that function will be invoked every single time.

```javascript
const useClapAnimation = () => {
  const [animationTimeline, setAnimationTimeline] = useState(
    new mojs.Timeline()
  );
};
```

In the above example, I do not want to initialize the mo-js timeline every single time. Essentially , favor passing a function reference as opposed to invoking a function within useState.

```javascript
const useClapAnimation = () => {
  const [animationTimeline, setAnimationTimeline] = useState(
    () => new mojs.Timeline()
  );
};
```

### Callback Refs

React also supports another way of set refs called 'callback refs', which gives more fine-grain control over when refs are set and unset.

Instead of passing a ref attribute created by createRef(), you pass a function. The function receives the React component instance or HTML DOM element as it's argument, which can be stored and accessed elsewhere.

```jsx
const MediumClap = () => {
  const [refs,setRefState]=useState({})
  const {clapRef,clapCountRef,clapTotalRef}=refs;

  const setRef = (node) => {
    //save the node
    setRefState(prevRefState=>{
      ...prevRefState,
      [node.dataset.refkey]:node
    })
  };

  const animationTimeline = useClapAnimation({
    duration: 300,
    bounceEl: clapCountRef,
    fadeEl: clapTotalRef,
    burstEl: clapRef,
  });

  return (
    <button
      ref={setRef}
      data-refKey="clapRef"
      className={styles.cap}
      onClick={handleClapClick}
    >
      <ClapIcon isClicked={isClicked} />
       <ClapCount count={count} setRef={setRef} />
      <CountTotal countTotal={countTotal} setRef={setRef} />
    </button>
  );
};
```

We are sending the `setRef` function as a prop to child components, we don't want the child components to be rendering because a reference to this changes.

```jsx
const setRef = useCallback((node) => {
  if (node !== null) {
    setRefState((prevRefState) => ({
      ...prevRefState,
      [node.dataset.refkey]: node,
    }));
  }
}, []);
```

### Getting Closures on React Hooks by Shawn Wang

```javascript
let foo = 1;
function add() {
  foo = foo + 1;
  return foo;
}

console.log(add()); // foo is in the global scope, you can move it inside the scope of the function.

// How to fix this

function getAdd() {
  let foo = 1;
  return function () {
    foo = foo + 1;
    return foo;
  };
}

const add = getAdd();
add(); // Now we cannot touch foo.
```

### Cloning hooks

```javascript
const React = function () {
  let hooks = [];
  let idx = 0;
  function useState(initVal) {
    const state = hooks[idx] || initVal;
    const _idx = idx;
    const setState = (newVal) => {
      hooks[_idx] = newVal;
    };
    idx++;
    return [state, setState];
  }
  function render(Component) {
    idx = 0;
    const C = Component();
    C.render();
    return C;
  }
  return { useState, render };
};

function Component() {
  const [count, setCount] = React.useState(1);
  const [text, setText] = React.useState("apple");
  return {
    render: () => console.log({ count, text }),
    click: () => setCount(count + 1),
    type: (word) => setText(word),
  };
}

var App = React.render(Component);
App.click(); //1
var App = React.render(Component);
App.type(); //2
```
