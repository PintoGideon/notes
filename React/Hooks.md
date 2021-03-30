React.useEffect is a built-in hook that allows you to run some custom code after React renders (and re-renders) your component to the DOM. It accepts a callback function which React will call after the DOM has been updated.

Why is lazy state initialization necessary?

React's useState hook allows you to pass a function instead of the actual value and then it will only call that function to get the state value when the component is rendered the first time.

So we can go from this:

```js
React.useState(someExpensiveComputation());
```

to this:

```javascript
React.useState(() => someExpensiveComputation());
```

The some expensive computation function will only be called when it's needed.

```javascript
React.useEffect(() => {
  window.localStorage.setItem("count", count);
}, [count]);
```

Your component can re-render for a lot of different reasons. For example, if your parent component re-renders.

Remember that the dependency list is how React knows how to call our callback. If we don't provide one then React will call our callback every render. It does this to ensure that the side effect we're performing in the callback doesn't get out of sync with the state of the application.

Would we remember to update the dependency list to include the `key` ?

```javascript
const updateLocalStorage = (name) => window.localStorage.setItem("name", name);

React.useEffect(() => {
  updateLocalStorage(name);
}, [name]);
```

The problem with that though is because `updateLocalStorage` is defined inside
the component function body, it's re-initialized every render, which means it's
brand new every render, which means it changes every render, which means, you
guessed it, our callback will be called every render!

Using a custom hook.

```javascript

function useLocationStorageState(key, defaultValue='', {
  serialize= JSON.stringify,
  deserizalize=JSON.parse
}){
 const [state, setState]=React.useState(
   ()=> {
     const valueInStorage= window.localStorage.getItem(key)
     if(valueInStorage){
        return deserialize(valueInStorage)
     }
     return typeof defaultValue==='function' ? defaultValue() : defaultValue;
   }
 )



 // Gives us an object without triggering renders if the key changes
 const prevKeyRef=React.useRef(key)

 React.useEffect(()=>{
   const prevKey =prevKeyRef.current

   if(prevKey !===key){
     window.localStorage.removeItem(prevKey)
   }
   prevKeyRef.current=key;
   window.localStorage.setItem(key, serialize(state))
 },[key, serialize, state])

return [state, setState]

}


function Greeting(initialName='')
{

const [name, setName]=useLocalStorageState('name', initialName)

function handleChange(event){
  setName(event.target.value)
}
  return (
    <div>
       <input value={name} type='text'
       onChange={handleChange}
       />
    </div>
}


```

1. React initializes.

2. Render.

3. React updates DOM.
4. Cleanup layout effects
5. Runs layout effects

6. Browser Paints screen

7. Cleanup react effects
8. Run react effects

# useEffect: HTTP requests

HTTP requests are another common side-effect that we need to do in applications. This is no different from the side effects we need to apply to a rendered DOM or when interacting with browser APIs like localStorage.

One important thing to note about the `useEffect` hook is that you cannot return anything more than the cleanup function. This has interesting implications with regards to async/await syntax.

```javascript
React.useEffect(async () => {
  const result = await doSomeAsyncThing();
});
```

The reason this doesn't work is because when you make a function async, it automatically returns a promise. This is due to the semantics of aysnc/await.

If you want to use asyc await , the best way to do that is like so:

```javascript
React.useEffect(() => {
  async function effect() {
    const result = await doSomeAsyncThing();
  }
  effect();
});
```

This ensures that you don't return anything but a cleanup function.

### A simple solution for useReducer

```javascript
function countReducer(state, action) {
  switch (action.type) {
    case "INCREMENT": {
      return { count: state.count + action.step };
    }
    default: {
      break;
    }
  }
}

function Counter({ initialCount = 0, step = 3 }) {
  const [state, dispatch] = React.useReducer(countReducer, {
    count: initialCount,
  });

  let { count } = state;

  const increment = () => dispatch({ type: "INCREMENT", step });

  return <button onClick={increment}>{count}</button>;
}
```

### useCallback

Let's recap to see what the dependency list of `useEffect` looks like

```javascript
React.useEffect(() => {
  window.localStorage.setItem("count", count);
});
```

Remember that the dependency list is how React knows how to call your callback. It does to ensure that the side effect you are performing in the app doesn't get out of sync with the state of the application.

```javascript
const updateLocalStorage = React.useCallback(
  () => window.localStorage.setItem("count", count),
  [count]
);

React.useEffect(() => {
  updateLocalStorage();
}, [updateLocalStorage]);
```

Since updateLocalStorage is defined inside the component function body, it's re-initialized every render, which means it brand new every render, which means it changs every render, which means you guessed it, our callback will be called every render.

```javascript
const updateLocalStorage = React.useCallback(
  () => window.localStorage.setItem("count", count),
  [count]
);

React.useEffect(() => {
  updateLocalStorage();
}, [updateLocalStorage]);
```

What that does is we pass React a function and React gives that same function
back to us, but with a catch. On subsequent renders, if the elements in the
dependency list are unchanged, instead of giving the same function back that we
give to it, React will give us the same function it gave us last time.

So while we still create a new function every render (to pass to `useCallback`),
React only gives us the new one if the dependency list changes.

```javascript
const [state, unsafeDispatch] = React.useReducer(countReducer, {
  count: 0,
});

const mountedRef = React.useRef(false);

React.useLayoutEffect(() => {
  mountRef.current = true;

  return () => {
    mountedRef.current = false;
  };
}, []);

const dispatch = React.useCallback((...args) => {
  if (mountedRef.current) {
    unsafeDispatch(...args);
  }
}, []);
```

A common question with this is: "Why don't we just wrap every function in
`useCallback`?" You can read about this in my blog post
[When to useMemo and useCallback](`https://kentcdodds.com/blog/usememo-and-usecallback`).

In Javascript, these statements are valuable to know

```javascript


{}==={}  //false
[]===[]  //false
()=>{}===()=>{}//false


```

When you define an object inside your React function component, it is not going to be referentially equal to the last time that same object was defined.

```javascript
function Foo({ bar, baz }) {
  const options = { bar, baz };

  React.useEffect(() => {
    buzz(options);
  }, [options]);

  return <div>foobar</div>;
}

function Blub() {
  return <Food bar="bar value" baz={3} />;
}
```

### Context in React

```javascript

const CountContext=React.createContext()

function CountProvider(props){
const [count, setCount]=React.useState(0)
const value=[count, setCount]
return <CountContext.Provider.value={value} {...props}/>
}

function CountDisplay(){
  const [count]=React.useContext(CountContext);
  return <div>{`The current Count is ${count}`}</div>
}

function App(){
  return(
    <CountProvider>
       <CountDisplay/>
    </CountProvider>
  )
}



```

### useLayoutEffect

The useEffect hook runs after react renders your component and ensures that your effect callback does not block browser painting.

If your effect is mutating the DOM and the DOM mutation will change the appearance of the DOM node between the time that it is rendered and your effect mutates it, then you don't want to use useEffect. The user could see a flicker when your DOM mutation takes place. This is pretty much the time you want to avoid useEffect.

### useImperativeHandle

This allows us to expose imperative methods to developers who pass a ref prop to
our component which can be useful when you have something that needs to happen
and is hard to deal with declaratively.

```javascript
const MessageDisplay = React.forwardRef(MessageDisplay);

function MessageDisplay() {
  const containerRef = React.useRef();

  React.useLayoutEffect(() => {
    scrollToBottom();
  });

  function scrollToTop() {
    containerRef.current.scrollTop = 0;
  }
  function scrollToBottom() {
    containerRef.current.scrollTop = containerRef.current.scrollHeight;
  }

  // Exposing the api to the parent ref.

  React.useImperativeHandle(ref, () => ({
    scrollToTop,
    scrollToBottom,
  }));
}

function App() {
  const messageDisplayRef = React.useRef();
  const scrollToTop = () => messageDisplayRef.current.scrollToTop();
  const scrollToBottom = () => messageDisplayRef.current.scrollToBottom();

  return (
    <div className="messaging-app">
      <MessageDisplay ref={messageDisplayRef} messages={messages} />
    </div>
  );
}
```

### useDebugValue : useMedia

Use to label custom hooks.

```javascript
const formatCountDebugValue = ({ initialCount, step }) =>
  `init: ${initialCount}; step: ${step}`;

function useCount({ initialCount = 0, step = 1 } = {}) {
  React.useDebugValue({ initialCount, step }, formatCountDebugValue);
  const [count, setCount] = React.useState(0);
  const increment = () => setCount((c) => c + 1);
  return [count, increment];
}
```


### useCallback


```javascript

const updateLocalStorage=() => window.localStorage.setItem("count", count);


React.useEffect(()=>{
  updateLocalStorage()
},[updateLocalStorage])


```