React is a JS library for building user interfaces. At it's core lies the mechanism that tracks changes in a component state and projects the updated state to the screen. In React we know this process as reconciliation.

There are various activities React performs during reconciliation.

### React Elements

Once a template goes through the JSZ compiler, you end up with a bunch of React elements. During reconciliation, data from every React element returned from the
render method is merged into the tree of fiber nodes. Every React element has a corresponding fiber node.
Fiber nodes aren't recreated on every render. These are mutable data structures that hold components state and the DOM.

Every React element is converted into a Fibe node of corresponding type that describes the work that needs to be done

### React