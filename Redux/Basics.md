### Resource Link (https://redux.js.org/basics/basic-tutorial)

### Payload Structure

Again, You will encounter a best practice that is not mandatory in redux. So far, the payload was dumped without much though in the actions. Now image an action that has a larger payload than a simple todo. For instance, the action payload should clarify to whom the todo is assigned.

```javascript

{
    type:'TODO_ADD_ASSIGNED',
    todo:{id:'0',name:'learn-redux'},
    assignedTo:{id:'99', name:'Robin'}
}
```

The properties would add up horizontally in the object but mask the most important property which is the action type,when they become too many. Therefore, you should action.type and the payload at the same level, but nest the payload itself one level deeper as the two abstract properties.

```
{
    type:'TODO_ADD_ASSIGNED',
    payload:{
        todo:{id:'0',name:'learn-redux'},
        assignedTo:{id:'99',name:'Robin'}
    }
}


```

### Nested Data Structures

The initial state is an empty list of todos. However, in growing applications you want to operate on more than todos. In a growing application, more objects and arrays will gather in the global state. That's why your initial state shouldn't be an empty array but an object that represents the state object. This object has nested properties.

