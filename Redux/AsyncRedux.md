### Async Redux

In the book, we have only used synchronous actions so far. There is no delay of the action dispatching forward.

A contrived example would be that you want to have a notification for your application user when a todo item was created. The Notification needs to show up but also hide after one sec.

The first action would show the notification, because it sets a isShowingNotification property to true in the Redux store. Afterward, you would want to have a delayed second action to hide the notification again.

```javascript
store.dispatch({
  type: "NOTIFICATION_SHOW",
  text: "Todo created"
});

setTimeout(() => {
  store.dispatch({
    type: "NOTIFICATION_HIDE"
  });
,1000});
```

### Redux Saga

Redux Saga is the most popular asynchronous actions library for Redux. "The mental model is that a saga is like a seperate thread in your application that's solely responsible for side effects. Basically, it outsources the impure functions into seperate threads. These threads can be paused or cancelled with plain Redux action from your core application.

```javascript
const TODO_ADD_WITH_NOTIFICATION = "TODO_ADD_WITH_NOTIFICATION";

function doAddTodoWithNotification(id, name) {
  return {
    type: TODO_ADD_WITH_NOTIFICATION,
    todo: {
      id,
      name
    }
  };
}
```

Now you can introduce your first saga that listens on this particular action because the action is solely used to start the saga thread.

```javascript
import { takeEvery } from "redux-saga-effects";

function* watchAddTodoWithNotification() {
  yield takeEvery(TODO_ADD_WITH_NOTIFICATION);
}
```

Most often you will find one part of the saga watching incoming actions and evaluating them. If an evaluation applies truthfully, it will often call another generator function , identified with the asterisk that handles the side effect. That way, you can keep your side-effect watcher maintainable and don't clutter with business logic.

In the previous example , a takeEvery() effect of Redux saga is used to handle every action with the specified action type.

```javascript


function *handleAddTodoNotification(action){
    const {todo}=action;
    const {id, name}=todo;

    yield put(doAddTodo(id, name));
    yield delay(5000)
    yiled put(doHideNotification(id));
}

```

In JS generators, you can use the yield statement to execute async code synchronously. Only when the function after the yield resolves, the code will execute the next line of code. 
