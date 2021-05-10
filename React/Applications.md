### Authentication in Applications

Applications without user authentication cannot reliably store
and present data tied to a specific
user.

We need to use a 'token' which represents the user's current session. That way the token can be invalidated and the user doesn't have to change their password.

In every request, the client makes must include this token to make authenticated requests. Most backend clients will give you a mechanism for retrieving a token when the user opens the application and we can use the token to make authenticated requests to the backend. 

1. Call a API to retrieve a token
2. If there's a token, send it along with the requests we make.

When the app loads in the first place, we'll call our auth provider's API to retrieve a token if the user is already logged in. Then we can show a loading screen while we request the user's data before rendering anything else. If they aren't, then we can render the login screen right away.


### Routing

```javascript
import * as React from 'react';
import ReactDOM from 'react-dom';

import {BrowserRouter as Router, Routes, Route,
useParams, Link
} from 'react-router-dom'


function App(){
    return(
        <div>
          <h1>Welcome</h1>
          <Nav/>
          <Routes>
          <Route path='/' element={<Home/>}/>
          <Route path='/about' element={<About/>}/>
          <Route path='/dog/:dogId' element={<Dog/>}/>
          <Route path="*" element={<YouLost/>}>


          </Routes>

        </div>
    )
}



```



### Cache Management

State can be lumped into two buckets:

1. UI state: Modal is open, item is highlighted, etc.
2. Server cache: User data, tweets, contacts etc.

### Using React query

```javascript
import {useQuery} from 'react-query'
import axios from 'axios'

export default function App(){
    const queryInfo=useQuery('pokemon',async()=>{
        await new Promise(resolve=>setTimeout(resolve,1000))
        return axios.get('https://pokeapi.co/api/v2/pokemon').then(res=>res.data.results)
    })

    return queryInfo.isLoading ? (
        'Loading...'
    ):(
        <div>
           {
               queryInfo.data?.map(result=>{
                   return <div key={result.name}>{result.name}</div>
               })
           }
           <br/>
           {
               queryInfo.isFetching ? 'Updating...' : null
           }

        </div>

    )
}


```

What makes a query stale? React-query marks the query stale as soon as it
is resolved. It will refetch the query as soon as the window is focused.
Seems a little big agressive?

We can add a stale time configuration option. This means for that particular
time the query will be marked as fresh.
Stale time can range from 0 to infinity.

```javascript

yarn add react-query-devtools

```




