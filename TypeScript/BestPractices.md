Notes from the React & Typescript workshop.
(https://github.com/stevekinney/react-and-typescript-projects/blob/master/examples/26-color-swatch-refactored/src/Application.tsx)

```jsx
type NameTagProps = {
    name:string
}

const NameTag= (props: NameTagProps){
    return(
        <div>
          Hi {props.name}
        </div>
    )
}

const Application = () => <NameTag name='Gideon'/>

```

Some contrived examples of types.

```jsx
type Item = {
  id: string,
  title: string,
};

type Dictionary = {
  [key: number]: string,
};

type ContrivedExample = {
  onchange: (id: number) => void,
};
```

### What should the type of children be?

Anything that would work as a HTML child which can be a string, React node, we can use `React.ReactNode` as the type.

```jsx

export default Application(){
    return (
        <Box>
          Just a String
        </Box>
    )
}

function Box (children:React.ReactNode){
    return props.children
}

```

### Setting State without a default Value

```jsx
import React from "react";
const Application = () => {
  const [character, setCharacter] =
    (React.useState < CharacterType) | (null > null);
  const [loading, setLoading] = React.useState(true);

  React.useEffect(() => {
    fetchCharacter().then((c) => {
      setCharacter(c);
    });
  });

  return <main>{character && <Character character={character} />}</main>;
};

export default Application;
```

### Typing Reducers

Typing actions in reducer functions can be a bit tricky.

```javascript
type BasicAction = {
  type: "INCREMENT" | "DECREMENT",
};

type SetAction = {
  type: "SET",
  payload: number,
};

type CounterState = {
  value: number,
};

const reducer = (state: CounterState, action: BasicAction | SetAction) => {
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value + 1 };
    case "SET":
      return { value: action.payload };
    default:
      return state;
  }
};
```

Typing the Context API

```jsx
import React from 'react';

type Themes = {
    [key:string]: React.CSSProperties
}

const defaultTheme = Themes = {
     light:{
      backgroundColor:'white',
      color:'black'
    },
    dark:{
        bacgkourndColor:'#555',
        color:'white'
    }

}

const ThemeContext = React.CreateContext<Themes>(defaultTheme);

export ThemeProvider=({children}:{children:React.ReactNode})=>{
    return <ThemeContext.Provider value={children}></ThemeContext.Provider>
})


```

In the Reducer

```jsx

export type AdjustmentAction = {
    type:"ADJUST_RED" | "ADJUST_GREEN";
    payload:number
}

export const reducer = {
    state:RGBColorType,
    action:AdjustmentAction
}:RGBColorType => {
//....
}

```

In the context

```ts
interface RGBContextType extends RGBColorType {
  dispatch: React.Dispatch<AdjustmentAction>;
}

export const RGBContext = React.createContext<RGBContextType>(
  initialState as RGBContextType
);
```

### Side note about Generics

```jsx
type Link<T> = {
  value: T,
  next?: Link<T>,
};

const node: Link<string> = {
  value: "wow",
};
```

### Some sample snippets of Utility Types

```jsx

const currentUser = {
    displayName:'J Mascis',
    accountId:'123',
    isVerified:true
}

const friends = UserModel[] = {
    {displayName: 'ChRIS', accountId:'234', isVerified:false}
}

type CurrentUserProps= Omit<UserMode, 'accountId'>

const CurrentUser = ({displayName, isVerified}:CurrentUserProps)=>{
  return(
      <div>
          {displayName}{isVerified}
      </div>
  )
}

export default CurrentUser;


```

### Higher Order Components

```javascript


type WithCharacterProps = {
  character:CharacterType;
}


function withCharacter (Component: React.ComponentType<T>) {
  return (props:Omit<T, keyof WithCharacterProps>) => {
    const [character, setCharacter] = React.useState<CharacterType | null> (null);
    const [loading, setLoading] = React.useState(true);

    React.useEffect(()=>{
       //fetch and set character
    },[])

    return <Component {...(props as T)} character = {character}/>
  }
};

const CharacterInfoWithCharacter = withCharacter(CharacterInformation);

const Application = () => {
  return <CharacterInfoWithCharacter />;
};


export default Application;
```

```javascript
var linkData = [
  {
    id: "1",
    name: "test_plugin",
    parent_id: [
      {
        id: "3",
        name: "test_plugin1",
      },
      {
        id: "4",
        name: "test_plugin2",
      },
    ],
  },
  {
    id:"2".
    name:"test_plugin4",
    parentId:[{id:"4", name:"test_plugin5"}]
  }
];

```


