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

The Actions need to be dispatched to the reducer

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

export function toggleTodo(index){
    return {type:TOGGLE_TODO, index}
}

export function setVisibilityFilter(filter){
    return {type:SET_VISIBILITY_FILTER,filter}
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
                return state
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

    // We want an array of todos where the completed property in each object in the array is completed

    case TOGGLE_TODO: return Object.assign({},state,{
        todos: state.todos.map((todo,index)=>{
            if(index===action.index){
                return Object.assign({},todo,{
                    completed:!todo.completed
                })
            }
            return todo;
        })
        
    })
    default:return state
}

```

### Reducer Composition using combine reducers
### Complete Summary


```javascript 

//actions.js

export const VisibilityFilters={
    SHOW_ALL:'SHOW_ALL',
    SHOW_COMPLETED:'SHOW_COMPELTED',
    SHOW_ACTIVE:'SHOW_ACTIVE'
}

export function addTodo(text) {
  return { type: ADD_TODO, text }
}

export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}




//reducers.js




import {combineReducers} from 'redux';
import {
    ADD_TODO,
    TOGGLE_TODO,
    SET_VISIBILITY_FILTER,
    visibilityFilters
} from './actions'


const {SHOW_ALL}=VisibilityFilters

function visibilityFilter(state=SHOW_ALL,action){
    switch(action.type){
        case SET_VISIBILITY_FILTER:
        return action.filter

        default:return state    
    }
}


function todos(state=[],action){
    switch(action.type){
        case ADD_TODO:
        return [
            ...state,
            {
                text:action.text,
                completed:false
            }
        ]

        default:return state
    }
}


const todoApp=combineReducers({
    visibilityFilter,
    todo
})

export default todoApp;

```

### Store

Actions represent the facts about what happenedd and the reducers
update the state according to those actions.


The store is the object that brings them together.

- Holds application state
- Allows access to state via getState()
- Allows state to be updated via dispatch(action)
- Registers listeners via subscribe(listener)
- Handles unregistering of listeners


```javascript

import {createStore} from 'redux';
import todoApp from './reducers';
const store=createStore(todoApp)

console.log(store.getState())

//Every time the state changes, log it

const unsubscribe=store.subscribe(()=>store.getState())


//Dispatch some actions

store.dispatch(addTodo('Learn About Actions'))
store.dispatch(setVisibilityFilters(VisibilityFilters.SHOW_COMPLETED))

```


### Complete Data Flow

1. You call store.dispatch(action)

An action is a plain object that describes that happened.


2. The Redux Store calls the reducer function you gave it

The store will pass two arguments to the reducer. The current state
tree and the action. 

```javascript
let previousState={
    visibilityTodoFilter:'SHOW_ALL',
    todos:[
        {
            text:'Read the docs',
             completed:false
        }
    ]
}

let action={
    type:'ADD_TODO',
    text:'Understand the flow'
}


let nextState=todoApp(previousState,action)

```

3. The root reducer may combone the output of multiple reducers into a single state tree.


```javascript

let todoApp=combineReducers({
    todos,
    visibilityTodoFilter
})

```

4. The Redux stores saves the complete state tree returned by the root reducer. Every listener registered with store.subscribe(listener) will now be invoked, listeners may call store.getState() to get the current state.



### Usage with React

React bindings for Redux seperate presentational components from 
container components. 


- Presentational Components (How things look)
- Container Components (How things work)

Most of the components we'll create will be presentational, but we will need to generate a few container componets to connect them to the Redux store. 


### Thinking in React

Suppose our JSON API returns some data that looks like this

```javascript

[
{category:"Sporting goods",price:"$49.99",stocked:true, name:"Football"}.
{category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"}
]
```
Since you're often displaying a JSON data model to a user, you'll find
that if your model was built correctly. your UI will map nicely. Your UI and data models tend to adhere to the same information architecture.


### Three questions to ask about each piece of data

1. Is it passed in from a parent via props
2. Does it remain unchanged over time? If so, it probably isn't state
3. Can you compute it based on any other state or props in your component? if so it isn't state.


### For each piece of state in your application

1. Identify every component that renders something based on that state
2. Find a common owner component (a single component above all the components that need the state in the hierarchy).
3. Either the common owner or another component higher up in the hierarchy should own the state.
4. If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common owner component.


### Designing other Components


We also need some container components to connect the presentational components to Redux. 
A container component is just a React Compnent that uses store.subscribe() to read a part of the Redux State tree and supply props to a presentational component it renders. You could write a container component
by hand but we suggest instead generating container components with React Redux's library connect().

To use connect(), you need to use mapStateToProps() that describes how to transform the current Redux store state into the props you want to pass to a presentational component. 

In addition to reading the state, container components can dispatch actions. In a similar fashion, one can define a function called mapDispatchToProps() that receives the dispatch() method and returns callback props that you want to inject into the presentational component.


```javascript

const mapStateToProps=state=>{
    return {
        todos:getVisibileTodos(state.todos,state.visibilityFilter)
    }
}



const mapDispatchToProps=dispatch=>{
    return{
        onToDoClick=id=>{
            dispatch(toggleTodo(id))
        }
    }
}

```

### Complete App


```javascript

// index.js

import React from 'react';
import {render} from 'react-dom';
import  {Provider} from 'react-redux';
import {createStore} from 'redux';
import rootReducer from './reducers'
import App from './components/App'

const store=createStore(rootReducer)


ReactDOM.render(<Provder store={store}></App></Provder>,document.getElementById('root'));

//App.js



import React from 'react';
import Footer from './Footer';
import AddTodo from '../containers/VisibileTodoList';
import VisibileTodoList from './containers/VisibleTodoList'


const App=()=>(
    <div>
       <AddTodo/>
       <VisibleTodoList/>
       <Footer/>

    </div>

)


export default App



```



### Taking a detour to setup my action creators and reducers


```javascript
//actions.js

let nextTodoId=0;

export const addtodo=(text)=>{
    return {
        type:ADD_TODO,
        id:nextTodoId++,
        text
    }
}

export const serVisibilityFilter=(filter)=>{
    return {
        type:SET_VISIBILITY_FILTER,
        filter
    }
}

export const toggleTodo=id=>{
    return{
        type:'TOGGLE_TODO',
        id
    }
}


export const VisibilityFilters={
    SHOW_ALL:'SHOW_ALL',
    SHOW_COMPLETED:'SHOW_COMPLETED',
    SHOW_ACTIVE:'SHOW_ACTIVE'
}

```




### Reducers

```javascript
//reducers/todos.js

const todos=(state=[],action)=>{
    switch(action.type){
        case 'ADD_TODO':
        return [
          ...state,
          {   
              id:action.id,
              text:action.text,
              completed:false
          }
        ]

       case 'TOGGLE_TODO':
       return state.map(todo=>
       todo.id===action.id?{...todo,comoleted:!todo.completed}:todo
       ) 

       default:return state
    }
}


//reducers/visibilityFilter.js

import {visibilityFilters} from './actions';


const visibilityFilter=(state=VisibilityFilters.SHOW_ALL,action)=>{
    switch(action.type){
        case 'SET_VISIBILITY_FILTER':
        return action.filter
        
        default:return state
    }
}

export default visibilityFilter




//reducers/index.js


import {combineReducers} from 'redux';

import todos from './todos';
import visibilityFilter from './visibilityFilter'



export default combineReducers({
    todos,
    visibilityFilter
})














```


```javascript
// AddTodo.js (Container Component)

import React from 'react';
import {connect} from 'react-redux';
import {addTodo} from '../actions';


const AddTodo=({dispatch})=>{
    let input

    return(
        <div> 
        <form onSubmit={e=>{
            e.preventDefault();
              if(!input.value.trim()){
                  return
              }
              dispatch(addTodo(input.value))
              input.value=''

        }}>
        <input ref={node=>(input=(input=node)}
        <button type="submit">Add Todo</button>
        </form>
        </div>
    )
}

export default connect()(AddTodo);

```

```javascript

//containers/VisibileTodoList.js

import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { VisibilityFilters } from '../actions'

const getVisibileTodos=(todos,filter)=>{
switch(filter){
    case VisibilityFilters.SHOW_ALL:
    return todos
    case VisibilityFilters.SHOW_COMPLETED:
    return todos.filter(t=>t.completed)
    case VisibilityFilters.SHOW_ACTIVE:
    return todos.filter(t=>!t.completed)
    default:
    throw new Error('Unknown Filter'+filter)
}


const mapStateToProps=state=>({
  todos:getVisibileTodos(state.todos,state.visibilityFilter)  
})

const mapDispatchToProps=dispatch=>({
    toggleTodo:id=>dispatch(toggleTodo(id))
})
}

export default connect(
    mapStateToProps,
    mapDispatchToProps
)(TodoList)



//components/TodoList.js


import React from 'react';
import PropTypes from 'prop-types'
import Todo from './Todo'


const TodoList=((todos, toggleTodo))=>(
    <ul>
       {todos.map(todo=>(
           <Todo key={todo.id} {...todo} onclick={()=>toggleTodo(todo.id)}/>
       ))}

    </ul>
)


export default TodoList





//components/Todo.js


import React from 'react';
import PropTypes from 'prop-types'


const Todo=({onClick, completed,text})=>(
    <li onClick={onClick}
    style={{textDecoration:completed?'line-through':'none'}}
    
    >
{text}
    </li>
)

export default Todo;




//components/Footer.js



import React from 'react'
import FilterLink from '../containers/FilterLink'
import { VisibilityFilters } from '../actions'

const Footer = () => (
  <div>
    <span>Show: </span>
    <FilterLink filter={VisibilityFilters.SHOW_ALL}>All</FilterLink>
    <FilterLink filter={VisibilityFilters.SHOW_ACTIVE}>Active</FilterLink>
    <FilterLink filter={VisibilityFilters.SHOW_COMPLETED}>Completed</FilterLink>
  </div>
)

export default Footer



//containers/FilterLink.js

import { connect } from 'react-redux'
import { setVisibilityFilter } from '../actions'
import Link from '../components/Link'

const mapStateToProps = (state, ownProps) => ({
  active: ownProps.filter === state.visibilityFilter
})

const mapDispatchToProps = (dispatch, ownProps) => ({
  onClick: () => dispatch(setVisibilityFilter(ownProps.filter))
})

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Link)



//Components/Link.js


import React from 'react'
import PropTypes from 'prop-types'

const Link = ({ active, children, onClick }) => (
  <button
    onClick={onClick}
    disabled={active}
    style={{
      marginLeft: '4px'
    }}
  >
    {children}
  </button>
)

Link.propTypes = {
  active: PropTypes.bool.isRequired,
  children: PropTypes.node.isRequired,
  onClick: PropTypes.func.isRequired
}

export default Link








