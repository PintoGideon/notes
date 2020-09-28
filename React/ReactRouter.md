```javascript
import { BrowserRouter as Router } from "react-router-dom";

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("app")
);
```

### Link

```javascript
<nav>
  <Link to="/">Home</Link>
  <Link to="/messages">Messages</Link>
</nav>
```

Dynamic url parameters

```javascript
<Route path="/:handle">
  <Profile />
</Route>
```

We get access to the parameters using the useParams hook.

```javascript
import { useParams } from "react-router-dom";

function Profile() {
  const [user, setUser] = React.useState(null);
  const { handle } = useParams();
}
```

### useRouteMatch

```javascript
const { path, url } = useRouteMatch();

function topic({ topics }) {
  const topic = topics.find(({ id }) => id === topicId);

  return (
    <div>
      <h2>{topic.name}</h2>
      <p>{topic.description}</p>
      <ul>
        {topic.resources.map((sub) => {
          <li key={sub.id}>
            <Link to={`${url}/${sub.id}`}>{sub.name}</Link>
          </li>;
        })}
      </ul>

      <Route path={`${path}/:subId`}>
        <Resource />
      </Route>
    </div>
  );
}
```

### Render props

```javascript
// Version 4

<Route path="/dashboard" render={() => <Dashboard authed={true} />} />

// Version 5

<Route path="/dashboard">
<Dashboard authed={true}>
</Route>
```

### Programatically navigating using React Router

```javascript
import { useHistory } from "react-router-dom";

function Register() {
  const history = useHistory();

  return (
    <div>
      <h1>Register</h1>
      <Form afterSubmit={() => history.push("/dashboard")} />
    </div>
  );
}
```

### Processing Query Strings in React Router

```javascript
import { useLocation } from "react-router-dom";
import queryString from "query-string";

export default function useLocation() {
  const { search } = useLocation();
  const { name, age } = queryString.parse(search);

  return (
    <div>
      <h1>Name: {name}</h1>
      <h3>Age: {age} </h3>
    </div>
  );
}
```

### The Switch Component

```jsx

export default function App(){
return(
  <Router>
     <div>
      <ul>
       <Link to ="/">Home<Link>
      </ul>
      <Switch>
       <Route exact path='/'><Home/></Route>
      </Switch>
     </div>
  </Router>
)
}

```

With Switch, The React router will match the first route in the list of Route components.

```jsx
import React from "react";
import { BrowserRouter as Router, Link } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <div>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/notifications">Notifications</Link>
        </li>
        <li>
          <Link to="/tylemcginnis">Tyler (dynamic) </Link>
        </li>
      </div>
    </Router>
  );
}
```

The routes look like this:

```jsx
<Route exact path="/"><Home/><Route>
<Route exact path="/notifications"><Notifications/></Route>
<Route path='/:handle'><Profile></Route>

```

The problem, as mentioned earlier, is every time you navigate to /notifications , not only does the Notifications component render, but so does the Profile component since /:handle is also matching. What we need is a way to tell React Router v5 to not match on /:handle if /notifications already matched. Another way to put that is we only want to render the first Route that matches, not every Route that matches which is the default behavior.`

### Customizing the Link Component with React router

```jsx
import React from 'react'
import {useRouteMatch} from 'react-router-dom'

export default function App() {
  returm(
    <Router>
      <div>
        <MyCustomLink exact={true} to="/">
          Home
        </MyCustomLink>
      </div>

      <Route exactPath="/">
        <Home />
      </Route>
    </Router>
  );
}

function MyCustomLink({ children, to, exact }) {
  const match=useRouteMatch({
    exact,
    path=to
  })
  return(
    <div className={match?'active':''}>
       {
         match?'>':''
       }
       <Link to={to}>
       {children}
       </Link>
    </div>
  )
}
```

### Code Splitting

Es modules are static which means we must specify what we import or export at compile time, not run time.
This also means you can't dynamically import a module based on some condition.

But what if we want to load modules on demand?

```jsx
import Home from "./Home";
import Topics from "./Topics";
import Settings from "./Settings";

export default function App() {
  return (
    <Router>
      <div>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/topics">Topics</Link>
        </li>
        <li>
          <Link to="/settings">Settings</Link>
        </li>
      </div>

      <Route exact path="/">
        <Home />
      </Route>
      <Route path="/topics">
        <Topics />
      </Route>
      <Route path="/settings">
        <Settings />
      </Route>
    </Router>
  );
}
```

Now, say our /settings route was super heavy. It contains a rich text editor, an original copy of Super Mario Brothers, and an HD image of Guy Fieri. We don’t want the user to have to download all of that when they’re not on the /settings route. We’ve already learned about Dynamic Imports but there’s still one piece of information we’re missing, React.lazy .

React.lazy takes in a single argument- a function that invokes a dynamic import.

```jsx
const LazyHomeComponent = React.lazy(() => import("./Home"));

function App() {
  return (
    <div>
      <React.Suspense fallback={<Loading />}>
        <LazyHomeComponent />
      </React.Suspense>
    </div>
  );
}
```

### Protected Routes in React

```jsx
import React from 'react';

function PrivateRoute({ children, ...rest }) {
  return(
<Route {...rest} render={({location})=>{
return fakeAuth.isAuthenticated ? children : <Redirect to={{
  pathname:'/login', state={
    from:location
  } a
}}/>
}}/>
)
}
```
