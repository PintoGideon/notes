### Middleware in Redux

In Redux, you can use a middleware. Every dispatched action flows through a middleware. You can opt-in a specific feature in between of dispatching an action and the moment it reaches the reducer.

It is the Redux store which can be initialized with it. The createStore() functionality from Redux takes as third argument a so called enhancer. The redux library comes with one of the enhancers: applyMiddleware().

```javascript
import {applyMiddleware, createStore} from 'redux';
const store=createStore(reducer, undefined, applyMiddleware(....))
```

### Redux logger

```javascript
const logger = createLogger();
const store = createStore(reducer, undefined, applyMiddleware(logger));
```

The applyMiddleware() takes any number of middleware. The action will flow through all middleware before it reaches the reducers.

### Normalized State

A best practice in Redux is a flat state structure. You don't want to maintain an immutable structure for your state when it is deeply nested. It becomes tedious and unreadable even with spread operators.
