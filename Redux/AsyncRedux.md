### Async Actions

When you call an asynchronous API,there are two crucial moments in time: The moment you start the call and the moment when you receive an answer.

Each of these two moments require a change in the application state. to do that, you need to dispatch normal actions that will be produced by reducers asynchronously. Usually
for any API request you'll want to dispatch atleast three different kinds of actions:

- An action informing the reducers that the request began

- An Action informing the reducers that the request finished successfully.

- An action informing the reducers that the request failed.


### index.js

```javascript

import React from 'react';
import {render} from 'react-dom';
import Root from './containers/Root'


render(<Root/>,document.getElementById('root'))

```

### Action Creators and Constants

```javascript

//actions.js


import fetch from 'cross-fetch';

export REQUEST_POSTS='REQUEST_POSTS';
export RECEIVE_POSTS='RECEIVE_POSTS';


function requestPosts(subreddit){
    return {
        type:REQUEST_POSTS,
        subreddit
    }
}


function receivePosts(subreddit){
    
}




