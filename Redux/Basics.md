```javascript
const reducer = (state, action) => {
  return state;
};

const store = createStore(reducer);
```

```javascript
const ADD_USER = "ADD_USER";
const ADD_TASK = "ADD_TASK";

const addTask = (title) => ({ type: ADD_TASK, payload: title });
const addUser = (name) => ({ type: ADD_USER, payload: name });

const userReducer = (users = initialState.users, action) => {
  if (action.type === ADD_USER) {
    return [...users, action.payload];
  }

  return users;
};

const taskReducer = (tasks: initialState.tasks, action) => {
  if (action.type === ADD_TASK) {
    return [...tasks, action.payload];
  }

  return tasks;
};

const reducer = combineReducers({ users: userReducer, tasks: taskReducer });
```

### Some rules for reducers

1. No mutating objects
2. You have to return something and ideally , it should be the unchanged state if there is nothing you need to do it.
3. Prefer flat objects

### Enhancers in Redux

Syntax: Enhancer to log performance metrics in Redux

```javascript

import {createStore} from 'redux';

const reducer = (state) => state;

const monitorEnhancer = (createStore) => (reducer, initialState, enhancer) => {
  const monitoredReducer = (state, action) => {
    const start = performance.now();
    const newState = reducer(state, action);
    const diff = Math.round(end - start);
    const end = performance.now();
    const diff = Math.round(end - start);
    console.log("Diff", diff);
    return newState;
  };
  return createStore(reducer, initialState, enhancer);
};

const store= createStore(reducer, monitorEnhancer);
```

### Hooks


```javascript
import {useDispatch} from 'react-redux';
import {useMemo} from 'react';

export const useActions = (actions) => {
  const dispatch = useDispatch();
  return useMemo(bindActionCreators(actions, dispatch),[actions, dispatch]);
}


```

### How to cache fetched data in react without redux?

When you use React, at a single point in time you can think of the render() function as creating a tree of React elements.
On the next state or props update, that render() function will return a different tree of React Elements.
React then needs to figure out how to efficiently update the UI to match the most recent tree.
React implements a heuristic O(n) algorithm based on two comparisons.

1. Two elements of different types will producde different trees.
2. The developer can hint at which child elements may be stable across different renders with a key prop.

### The diffing algo.

Whenever the root elements have differnt types, React will tear down the old tree and build the new tree from scratch. Going from <a> to <img> will lead to a full rebuild.

When tearing down a tree, old DOM nodes are destroyed. Component instances receive ComponentWillUnmount(). When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive componentWillMount() and then ComponentDidMount(). State assosciate with the old tree is lost,

Re-render in this context means calling render for all components, it doesn't mean React will unmount and remount them.

### Hand crafting caching with reselect

Each selector has a cache limit of one. Instead of keeping a small store of cache results, we can only keep track of the last computer argument-result pair.

Selectors would be dynamically created in each component that uses them, they wouldn't share the cache between themselves. This means that if we want to get the same information in two different components , we'd have to instantiate two selectors and compute it twice, not once.

```javascript
import createCachedSelector from "re-`reselect";
import _ from "lodash";

import localState from "./articles";

const getArticles = (state) => state.articles;
const getTags = (state) => state.tags;

const tagsFromYear = createCachedSelector(
  [getArticles, getTags, (_state, year) => year],
  (articles, tags, year) => {
    const nestedTagIDs = _.filter(
      artilces,
      (article) => article.lastModifiedOn === year
    ).map((article) => article.tags);
    const flatTagIDs = _.flatten(nestedTagIDs);
    const uniqueTagIDs = _.uniq(flatTagIDs);
    const tagNames = uniqueTagIDs.map((tagid) => tags[tagid], name);
    return tagNames;
  }
)((_state, year) => year);
```

```javascript
const getFiles = (state) => state.files;
const getSelected = chris / feed_4 / dircopy_4 / freesurfer_pp_5 / data;
chris / uploads / DICOM / dataset1;
chris / uploads / test_late - temp - 6215;

uploads / DICOM / dataset1;
```

### Using Redux with Typescript

### Initial Scafolding

```javascript

// Action Creators Type
export enum ActionType {
  MOVE_CELL = 'move_cell',
  DELETE_CELL='delete_cell'
}

// Action Payload

interface MoveCellAction {
  type: ActionType.MOVE_CELL
}

interface DeleteCellAction {
  type:ActionType.DELETE_CELL,
  payload: {
    id: string
  }
}

export type Action = MoveCellAction | DeleteCellAction

// Reducer

interface CellsState {
  loading: boolean;
  error:string | null;
  order: string[];
  data: {
    [key:string]:Cell
  }
}


const initialState: CellsState = {
  loading: false,
  error: null,
  order: [],
  data: {}
}

const reducer = (state:CellsState = initialState, action:Action) => {
switch(action.type) {
 case ActionType.MOVE_CELL : return state;
 case ActionType.DELETE_CELL: return state;
 default:
  return state;
}
}

const reducers = combineReducers({
  cells: cellsReducer
})

export default reducers;
export type RootState= ReturnType <typeof reducers>;

import {createStore} from 'redux';

export const store=createStore(reducers, {}, applyMiddleware(thunk))

// Action creators

export const moveCell=()=>{}
export const deleteCell=()=>{}

```

### Avoid Cyclic Dependency

If one file uses a exported type used by another file and vice versa, we create a cyclic dependency.

### Usage in React

```javascript

const App = () => {
  return (
    <Provider store={store}>
      <div>
        <TestApp/>
      <div>
    </Provider>
  )
}

```

### Examples of a data structure in Redux

```javascript

{
  loading:false,
  error:null,
  data : {
    '1': {
      id:"cell-1",
      type:'code',
      content:"const a = 1"
    },
    '2' : {
      id: "cell-2",
      type:"text",
      content:"Documentation for this thing"
    }
  }
}

```

### CRUD operations in Redux

```javascript
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case ActionType.UPDATE_CELL:
      const { id, content } = action.payload;
      return {
        ...state,
        data: {
          ...state.data,
          [id]: {
            ...state.data[id],
            content: content,
          },
        },
      };
  }
};
```

### Using Immer to manage state in a reducer

```javascript
import { produce } from "immer";

const reducer = produce((state = initialState, action) => {
  switch (action.type) {
    case ActionType.UPDATE_CELL:
      const { id, content } = action.payload;
      state.data[id].content = content;
      //  Immer will return the state object for us.
      return state;

    case ActionType.DELETE_CELL:
      delete state.data[action.payload];
      state.order = state.order.filter((id) => id !== action.payload);
      return state;
  }
});
```

### Validating code in our reducer

We can manually get state out of our store and manually dispatch actions
to validate our code. This solution does not replace testing, but helps us get a better understanding of our workflows.

```javascript
export const store = createStore(reducers, {}, applyMiddleware(thunk));

// We can dispatch actions to the store as a test

store.dispatch({
  type: ActionType.DELETE_CELL,
  payload: {
    id: "1",
  },
});

store.getState();
```

### The React side of things.

```jsx
const CellList: React.FC = () => {
  return <div>Cell List </div>;
};

export default CellList;
```

### Creating a Typed Selector

```javascript
import { useSelector, TypedUseSelectorHook } from "react-redux";
import { RootState } from "../state";

export const useTypedSelector: TypeduseSelectorHook<RootState> = useSelector;
```

### Using Typed Selectors (This is just an example)

```javascript
import { useTypedSelector } from "../hooks/use-typed-selector";

const CellList: React.FC = () => {
  useTypedSelector(({ cells: { order, data } }) => {
    return order.map((id) => {
      return data[id];
    });
  });

  return <div>Cell List </div>;
};

export default CellList;
```

### Dispatch Actions in React Components

```javascript
import { useDispatch } from "react-redux";
import { bindActionCreators } from "redux";
import { actionCreators } from "../state";

export const useActions = () => {
  const dispatch = useDispatch();
  return bindActionCreators(actionCreators, dispatch);
};
```

### Using Action Creators in React Components

```javascript
import { useActions } from "./useActions";

const CodeCell: React.FC = () => {
  const { updateCell } = useActions();
};

export default CodeCell;
```

###
