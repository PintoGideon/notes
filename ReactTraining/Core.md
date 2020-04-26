### Testing

This is an example of an assertion looks like.

```jsx
describe("ContentToggle",()=>{
    it("renders its summary inside a button",()=>{
        const node=document.createElement("div")
        ReactDOM.render(<ContentToggle summary="Tacos"/>,node)
        const button=node.querySelector("button")
        expect(button).not.toBe(null)
        ex
    })
}
```

```jsx
describe("when the button is clicked", () => {
  it("is open", () => {
    const node = document.createElement("div");
    let contentToggleInstance;

    ReactDOM.render(
      <StatefulContentToggle summary="tacos">
        <p> are the best...</p>
      </StatefulContentToggle>,
      node,
      function () {
        contentToggleInstance = this;
      }
    );

    const button = node.querySelector("button");
    Simulate.click(button);
    expect(contentToggleInstance.state.isOpen).toBe(true);
    //expect(node.innerHTML).toContain("The Best");
  });
});
```

Why the simulate module is amazing?

```jsx
handleDragOver = (event) => {
  if (event.dataTransfer.types[0] === "Files") {
    event.preventDefault();
    this.setState({
      acceptDrop: true,
    });
  }
};
```

```jsx
describe("Droppable", () => {
  it.only("works", () => {
    const node = document.createElement("div");
    document.body.appendChild(node);

    ReactDOM.render(<Droppable />, node);
    Simulate.dragOver(node.firstChild, {
      dataTransfer: { types: ["Files"] },
    });
    expect(node.innerHTML).toContain("Drop it");
  });
});
```

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
  const id = ++callCount;
  if(states[id]){
    return states[id]
  }
  const setValue = (newValue) => {
    // assign a new Value;
    states[id][0] = newValue;
    renderPhonyHooks();
  };
  const tuple = [initialValue, setValue];
  states.push(tuple);
  return tuple;
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

With data loading, we can use [] for the first time, but if the uid every changes, we're still rendered. We could be showing the wrong avatar.


We put the uid in there to make sure we always display the right thing. Make sure we 'synchronize' our rendered elements.
