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
      name,
    },
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
function* handleAddTodoNotification(action) {
  const { todo } = action;
  const { id, name } = todo;

  yield put(doAddTodo(id, name));
  yield delay(5000);
  yield put(doHideNotification(id));
}
```

In JS generators, you can use the yield statement to execute async code synchronously. Only when the function after the yield resolves, the code will execute the next line of code.

### High Level Saga

```javascript

import {take,call,put} from 'redux-saga/effects"

function * authorize(user,password){
//fetches and returns token
}

function* loginFlow(){
while(true)}
const {user,password}=yield take('LOGIN_REQUEST')
const token=yield call(authorize,user,password)
if(token){
  yield call(Api.storeItem,{token})
  yield take('LOGOUT')
  yield call(Api.clearItem,'token')
}
}
```

To express non-blocking calls, the library provides another Effect: fork. When we fork a task, the task is started in the background and the caller can continue its flow without waiting for the forked task to terminate.

So in order for loginFlow to not miss a concurrent LOGOUT, we must not call the authorize task, instead we have to fork it.

### Starting a race between multiple effects

Sometimes we start multiple tasks in parallel but don't want to wait for all
of them, we just need to get the winner. The first one that resolves (or rejects). The `race` effect offers a way of triggering a race between multiple
effects.

```javascript
import {take,call,put,delay} from 'redux-saga/effects';

function* fetchPostWithTimeout(){
  const {posts,timeout}=yield race({
    posts:call(fetchApi,'/posts').
    timeout:delay(1000)
  })
  if(posts){
    yield put({
      type:'POST_RECEIVED',posts
    })
  }
  else
  yield put({type:'Timeout Errpr'})
}

```

Suppose we want the user to finish some game in a limited amount of time.

```javascript
function* game(getState) {
  let finished;
  while (!finished) {
    const [score,timeout]=yield race({
      score:call(play,getState).
      timeout:delay(60000)
    })
  }
  if(!timeout){
    finished=true;
    yield put(showScore(score))
  }
}
```

### Polling with redux-saga/effeccts

```javascript
function* pollTask() {
  while (true) {
    try {
      const plugin = plugin.get();
      if (plugin.data.status === "finishedSuccessfully") {
        yield put(getPluginFiles(plugin));
        yield put(getPluginStatus(plugin.data.summary));
      }
      if (plugin.data.status === "started") {
        yield put(getPluginStatus(plugin.data.summary));
      }
      yield call(delay, 1000);
    }
    catch(err){
      yield put({
        getPluginFetchError(err)
      })
      yield put({type:'STOP_WATCHER_TASK',err})
    }
  }
}

function* pollTaskWatcher(){
  while(true){
    yield take('START_WATCHER_TASK')
    yield race([call(pollTask),take("STOP_WATCHER_TASK")])
  }
}
```

If the `call(pollTask)` resolves, the response will be the result of pollTask
and the result of cancel event will be undefined.
If the action 'STOP_WATCHER_TASK' is dispatch on the store before `pollTask`
completes, the response will be undefined and cancel will be the dispatched action.

```html
<button id="start">Start the Poll</button>
<button id="stop">Stop the Poll</button>
```

```javascript
const start = document.getElementById("start");
const stop = document.getElementById("stop");

start.addEventListener("click", () => {
  store.dispatch(startPoll());
});

const START_POLL = "START_POLL";

const startPoll = () => ({ type: START_POLL });
const stopPoll = () => ({ type: STOP_POLL });
```

### Polling with saga

```javascript
// Watcher

function* pollSaga(action) {
  while (true) {
    try {
      const { data } = yield call(() => axios({ url: ENDPOINT }));
      yield put(getDataSuccessAction(data));
      yield call(delay, 1000);
    } catch (err) {
      yield put(getDataFailureAction(err));
    }
  }
}

//Worker

function* watchPoll() {
  while (true) {
    yield take(START_POLL);
    yield race([call(pollSaga), take(STOP_POLL)]);
  }
}
```
