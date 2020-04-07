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

export const SEND_MESSAGE = "SEND_MESSAGE";
export const DELETE_MESSAGE = "DELETE_MESSAGE";

interface SendMessageAction {
  type: typeof SEND_MESSAGE;
  payload: messsage;
}

interface DeleteMessageAction {
  type: typeof DELETE_MESSAGE;
  meta: {
    timestamp: number,
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

export const UPDATE_SESSION = "UPDATE_SESSION";

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
  DELETE_MESSAGE,
} from "./types";

const initialState: ChatState = {
  messages: [],
};

export function chatReducer(
  state = intialState,
  action: ChatActionTypes
): ChatState {
  switch (action.type) {
    case SEND_MESSAGE:
      return {
        messages: [...state.messages, action.payload],
      };
    case DELETE_MESSAGE:
      return {
        messages: state.messages.filter(
          (message) => message.timestamp !== action.meta.timestamp
        ),
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

import { systemsReducer } from "./systems/reducers";
import { chatReducer } from "./chat/reducers";

const rootReducer = combineReducers({
  system: systemReducer,
  chat: chatReducer,
});

export type AppState = ReturnType<typeof rootReducer>;
```

### Usage with React Redux

```javascript
npm i @types/react-redux -D

```

```javascript
//src/App.tsx

import { SystemState } from "./store/system/types";
import { ChatState } from "./store/chat/types";
import { AppState } from "./store";

interface AppProps {
  chat: chatState;
  system: SystemState;
}

class App extends React.Component<AllProps> {}

const mapStateToProps = (state: AppState) => ({
  system: state.system,
  chat: state.chat,
});
```

### Typescript config

```javascript
npx tsc --init
npm install -D @types/react @types/react-dom @types/react-router
```

#### Typing Higher Order Component in React

```ts
import { Substract } from "utility-types";
export interface InjectedCounterProps {
  value: number;
  onIncrement(): void;
  onDecrement(): void;
}

interface makeCounterState {
  value: number;
}

const makeCounter = <P extends InjectedCounterProps>(
  Component: React.ComponentType<P>
) =>
  class MakeCounter extends React.Component<
    Substract<P, InjectedCounterProps>,
    makeCounterState
  > {};
```

An interface is being declared for the props that will be injected into the component.
It is exported so that they can be used by the component that the HOC wraps.

```ts
import makeCounter, { InjectedCounterProps } from "./makeCounter";

interface CounterProps extends InjectedCounterProps {
  style?: React.CSSProperties;
}

const Counter = (props: CounterProps) => {};

export default makeCounter(counter);
```

```ts
<P extends InjectedCounterProps>(Component:React.ComponentType<P>)
```

Again, we use a generic. This time it ensures that the component passed into the HOC includes
the props that are going to be injected by it. if it doesn't, you will receive a compilation error.

### Injecting Props

1. WrappedComponent to have new props injected automatically
2. Injected props not be required on the created EnhancedComponent
3. Any other props on WrappedComponent to be forwarded from EnhancedComponent
4. TypeScript to infer the above with no manual specification

```ts
interface ExpanderProps {
  isExpanded: boolean;
  toggleExpanded(): void;
}
```

Our Wrapped component will need to accept these props but it might define some others as well.
Let's say it has a title that needs to be specified.

```ts
interface WrappedComponentProps {
  isExpanded: boolean;
  toogleExpanded(): void;
  title: string;
  className?: string;
}
```

We could also implement this as WrappedComponentProps extends ExpanderProps.

```ts
const EnhancedComponent = withExpander(WrappedComponent);

const usage = <EnhancedComponent title="the title" className="class-name" />;
```

Giving the Wrapped Component a name.

```tsx
interface ExpandedComponentProps extends ExpanderProps {
  title: string;
}

class ExpanderComponent extends PureComponent<ExpanderComponentProps> {
  render() {
    const { isExpanded, toggleExpanded, title, children } = this.props;
    return (
      <>
        <button onClick={toggleExpanded}>{title}</button>
      </>
    );
  }
}
```

### Create the Enhancer

Now we want to create the enhancer function that we can use to inject `isExpanded` and `toggleExpanded`.

**_ComponentType_** is a react type that allows you to pass either a component class or a stateless functional component that has props of T.

```tsx
function withExpander(WrappedComponent: ComponentType<ExpanderProps>) {
  return class WithExpander extends PureComponent<{}, { isExpanded: boolean }> {
    private toggleExpanded = () =>
      this.setState((state) => ({ isExpanded: !state.isExpanded }));

    render() {
      return (
        <WrappedComponent
          {...this.state}
          toggleExpanded={this.toggleExpanded}
        />
      );
    }
  };
}
```

```tsx
const EnhancedComponent = withExpander(ExpanderComponent);
```

However, the exapander component requires a title prop.

Any other props from the wrapped component should be forwarded from the enhanced component. In this case, we want to have the title prop be exposed on enhanced component.

The first step is getting to that point is allow the WrappedComponent parameter of our HoC to accept those props that aren't on ExpanderProps. In fact, we need our wrapped component to accept props of both ExpanderProps and whatever else it wants.

```tsx
function withExpander<TWrappedComponentProps extends ExpanderProps>(
  WrappedComponent: ComponentType<TWrappedComponentProps>
) {
  //....
}
```

```tsx
const EnhancedComponent = withExpander(ExpanderComponent);
const usage = <EnhancedComponent title="Some Title" />;
```

We managed to write our HoC so that it allows extra props but all it does is ignore them.

### Pick and Exclude

Pick takes the props from T specified in U. Pick creates a type with only the `isExpanded` property.

Exclude takes everything from T except U. e.g. Exclude creates a type `'one' | 'three'`

By combining these with keyof, we can build up `all props that are on the inner component but excluding the one we're providing` type.

```tsx
function withExpander<TWrappedComponentProps extends ExpanderProps>(
  WrappedComponent: ComponentTYpe<TWrapeedComponentProps>
) {
	type WrappedComponentPropsExceptProvided=Exclude<keyof TWrappedComponentPropsm keyof ExpanderProps>
	type ForwardedProps=Pick<TWrappedComponentProps , WrappedComponentPropsExceptProvided>
}
```

```tsx
return class WithExpander extends PureComponent<
  ForwardedProps,
  { isExpanded: boolean }
> {
  render() {
    //....
  }
};
```

### Usage:

```tsx
const usage = <EnhancedComponent title="title"></EnhancedComponent>;
```


