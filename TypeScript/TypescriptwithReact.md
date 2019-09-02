### Type Checking State

Adding types to each slice of state is a good place to start since it
does not rely on other types.

### Example

```javascript
// src/store/chat/types.js

export interface Message {
	user: string;
	message: string;
	timestamp: number;
}

export interface ChatState {
	messages: Messgae[];
}

export interface SystemState {
	loggedIn: boolean;
	session: string;
	username: string;
}
```

### Type Checking Actions & Action Creators

```javascript
//src/store/chat/types.js

export const SEND_MESSAGE = 'SEND_MESSAGE';
export const DELETE_MESSAGE = 'DELETE_MESSAGE';

interface SendMessageAction {
	type: typeof SEND_MESSAGE;
	payload: messsage;
}

interface DeleteMessageAction {
	type: typeof DELETE_MESSAGE;
	meta: {
		timestamp: number
	};
}

export type ChatActionTypes = SendMessageAction | DeleteMessageAction;
```

**_We have now provide shape to our actions_**

```javascript
//src/store/chat/actions.ts

import {Message, SEND_MESSAGE, DELETE_MESSAGE. ChatActionTypes} from './types';

//TypeScript infers that this function is returning SendMessageAction

export function sendMessage(newMessage:Message):ChatActionTypes{
    return{
        type:SEND_MESSAGE,
        payload:newMessage
    }
}


export function deleteMessage(timestamp:number):ChatActionTypes{
    return{
        type:DELETE_MESSAGE,
        meta:{
            timestamp
        }
    }
}

```

### System Action Constants & Shape

```javascript
//src/store/system/types.js

export const UPDATE_SESSION = 'UPDATE_SESSION';

interface UpdateSessionAction {
	type: typeof UPDATE_SESSION;
	payload: SystemState;
}

export type SytemActionTypes = UpdateSessionAction;
```

```javascript
//src/store/system/actions.ts

import {SystemState, UPDATE_SESSION,SystemActionTypes} from './types';

export function updateSession(newSession:SystemState):SystemActionTypes{
    return{
        type:UPDATE_SESSION,
        payload:
    }
}
```

### Type Checking Reducers

Reducers are just pure functions that take the previous state, an action and return the next state. In this example, we explicitly declare the type of actions this reducer will receive along with what it should return. With these
additions, TypeScript will give rich intellisense on the properties of our actions and state.

**_Type Checked Chat Reducer_**

```javascript
//src/store/chat/reducers.ts

import {
	ChatState,
	ChatActions,
	ChatActionTypes,
	SEND_MESSAGE,
	DELETE_MESSAGE
} from './types';

const initialState: ChatState = {
	messages: []
};

export function chatReducer(
	state = intialState,
	action: ChatActionTypes
): ChatState {
	switch (action.type) {
		case SEND_MESSAGE:
			return {
				messages: [...state.messages, action.payload]
			};
		case DELETE_MESSAGE:
			return {
				messages: state.messages.filter(
					message => message.timestamp !== action.meta.timestamp
				)
			};
		default:
			return state;
	}
}
```

**_Type Checked System Reducer_**

```javascript

//initial state
//action
//return a state


import{
    SystemActions,
    SystemState,
    SystemActionTypes,
    UPDATE_SESSION
} from './types'


const intialState:SystemState={
    loggedIn;:False,
    session:'',
    username:''
}


export function systemReducer(state=initialState, action:SystemActionTypes):SystemState{
swtich(action.type){
    case UPDATE_SESSON:{
        return{
            ...state,
            action.payload
        }
    }
    default:state

}
}

```

We now need to generate the root reducer function which is normally using combineReducers. \*\*\*Note that we do not have to explicitly declare a new interface for AppState. We can use ReturnType to infer state shape from the root reducer

```javascript
//src/store/index.ts

import { systemsReducer } from './systems/reducers';
import { chatReducer } from './chat/reducers';

const rootReducer = combineReducers({
	system: systemReducer,
	chat: chatReducer
});

export type AppState = ReturnType<typeof rootReducer>;
```

### Usage with React Redux

```javascript
npm i @types/react-redux -D

```

```javascript
//src/App.tsx

import { SystemState } from './store/system/types';
import { ChatState } from './store/chat/types';
import { AppState } from './store';

interface AppProps {
	chat: chatState;
	system: SystemState;
}

class App extends React.Component<AllProps> {}

const mapStateToProps = (state: AppState) => ({
	system: state.system,
	chat: state.chat
});
```

### Typescript config

```javascript
npx tsc --init
npm install -D @types/react @types/react-dom @types/react-router
```
