### Resource Link (https://redux.js.org/basics/basic-tutorial)


### Actions

Actions are payloads of information that send data from your application to the store. They are the only source of info for your store.
```javascript
store.dispatch()
```


```javascript

const ADD_TODO='ADD_TODO';

{
    type:ADD_TODO,
    text:'Build my first React app'
}

```

Actions are plain JS objects with a type property that indicates the type of action being performed. Types should typically be defined as string constants.



### Action Creators

Action Creators are functions that create actions. 

```javascript

function addTodo(text){
    return{
        type:ADD_TODO,
        text
    }
}
```

The Actions need to be dispatch to the reducer

```javascript
dispatch(addTodo(text))
```

We will be accessing the dispatch method using react-redux's connect() method. 


```javascript

//action type
export const ADD_TODO='ADD_TODO';

//action  creator

export function addTodo(text){
    return {
        type:ADD_TODO,
        text
    }
}
```

### Reducers

Reducers specify how the application's state changes in response to actions sent to the store. 
In Redux, All the application state is stored as a single object. It's a good idea to think of it's shape before writing any code.



```javascript
{
    visibility:'SHOW ALL',
    todos:{
        text:'Consider using Redux',
        completed:true
  }
}

```


Now that we have decided what our state looks like, we are ready to write a reducer for it.

```javascript

const intialState={
    visibilityFilter:VisibilityFilters.SHOWALL,
    todos:[]
}

function todoApp(state=intialState, action){
    switch(action.type){
        case SET_VISIBILITY_FILTER:
            return Object.assign({}, state,{
                visibilityFilter:action.filter
            }),
            default:
                return states
    }
    case ADD_TODO: return Object.assign({},state,{
        todos:[
            ...state.todos,
            {
                text:action.text,
                completed:true
            }
        ]
    })
    default:return state
}

```






