### Async Actions

When you call an asynchronous API,there are two crucial moments in time: The moment you start the call and the moment when you receive an answer.

Each of these two moments require a change in the application state. to do that, you need to dispatch normal actions that will be produced by reducers asynchronously. Usually
for any API request you'll want to dispatch atleast three different kinds of actions:

- An action informing the reducers that the request began

- An Action informing the reducers that the request finished successfully.

- An action informing the reducers that the request failed.


### Adding Middleware


### Logger

```javascript

const logger=function(store){
return function (next){
    return function(action){
        console.log(`{Middleware} Dispatching`, action)
        const result=next(action);
        return result;
    }
}

}
```


### Applying the middleware to the store

```javascript

const store=createStore(rootReducer,applyMiddleware(logger))

```


### Async Redux

### Action Creators using Redux-Thunk

```javascript
export addPost=function(postData){
    return function(dispatch){
      axios.post('/api/posts',postData).then(res=>dispatch({type:'ADD_POST',payload:res.data})).catch(err=>dispatch({
          type:'GET_ERRORS',
          payload:err.response.data
      }))
      )
    }
}



```

### Redux Saga

redux-saga is a library that aims to make application side-effects (i.e asynchronous things like data fetching and impure things like accessing the browser cache)
easier to manage , more efficient to execute, easy to test and better at handling features.



saga is like a seperate thread in your app that's solely responsible for side effects. redux-saga is a redux middleware which means this thread can be started, paused,
and cancelled from the main application with normal redux actions. 

It uses an ES6 feature called Generators to make those async flows easy to read, write and test.

```javascript

class UserComponent extends React.Component{

onSomeButtonclicked(){
    const {userId, dispatch}=this.props
    dispatch({type:'USER_FETCH_REQUESTED',payload:{userId}})
}

}
```


### saga.js






```javascript

import {call, put, takeEvery, takeLatest} from 'redux-saga/effects';

// worker Saga: Will be fired on USER_FETCH_REQUESTED actions

function* fetchUser(action){
    const user=yield call(Api.fetchUser, action.payload.userId)
    yield.put({type:"USER FETCH REQUESTED",user:user})
} catch(e)
{
    yield put({type:"USER_FETCH_FAILED",message:message})
}



```

### A peek into generator functions

```javascript

function* foo(){
    var x-1 + (yield "foo")
    console.log(x)
}

```
The yield "foo" expression will send the "foo" string value out when pausing the generator function at that point, and whenever the generator is restarted, whatever
value is sent in will be the result of that expression, which will then get added to 1 and assigned to the x variable.


```javascript

import createSagaMiddleware from 'redux-saga';

const sagaMiddleware=createSagaMiddleware()

const store=createStore(reducer, applyMiddleware(sagaMiddleware))
sagaMiddleware.run(helloSaga)

const action=type=>stpre.dispatch({type})

```


```javascript

//saga.js

import {put, takeEvery} from 'redux-saga/effects'


//Creating a delay function that returns a Promise that will resolve after a set number of milliseconds.
const delay=ms=>new Promise(res=>setTimeout(res,ms))

export function* incrementAsync(){
   //The yielded objects are a kind of instruction to be interpreted by the middleware. When a promise is yielded by the middleware, the middleware
   //will  suspend the saga until the promise completes. Saga is suspended until the Promise returned by delay resolves which will happen after 1 second.

    yield delay(1000);
    yield.put({type:'INCREMENT'})
    }


function *watchIncrementAsync(){
    yield takeEvery('increment action',incrementAsync)
}


export default function* rootSaga(){
    yiled all([
        helloSaga(),
        watchIncrementAsync()
    ])
}

```
### Using Call and Delay

```javascript

export const delay=ms=>new Promise(res=>setTimeout(res,ms))


export function* incrementAsync(){
yield.call(delay,1000)
yield.put({type:'INCREMENT'})

}
```

### Understanding the Saga pattern

A saga is a collection of Sub-Transactions.
Each Sub Transaction has a compensating Transaction.

redux-saga provides some helper effects wrapping internal functions to spawn tasks when some specific actions are dispatched to the store.

```javascript

import {call,put} from 'redyx-saga/effects';

export function* fetchData(action){
    try {
        const data=yield call(Api.fetchUser, action.payload.url)
        yield.put({type:'Fetch Suceeded',data})
    }catch(error){
        yield put ({type:"FETCH FAILED",error})
    }
}


function* watchFetchData(){
    yield takeEvery('FETCH_REQUESTED',fetchData)
}


function watchFetchData(){
    yield takeLatest('FETCH_REQUESTED', fetchData)
}



```
 

###















```