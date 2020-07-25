### Testing notes

Jest looks for files in the src/ directory that run with the file extension
`.test.js`.

```js
import { render } from "@testing-library/react";

test("renders learn react link", () => {
  const { getByText } = render(<App />);
});
```

What does Enzyme do?

1. Search through the DOM using jQuery style selectors.
2. Simulate simple events
3. Render components only one level deep.
4. Render parent, but use placeholders for children.

```
npm install --save-dev enzyme jest-enzyme enzyme-adapter-react-16
```

Configuring Enzyme.

```javascript
import Enzyme from "enzyme";
import EnzymeAdapter from "enzyme-adapter-react-16";

Enzyme.configure({ adapter: new EnzymeAdapter() });

test("Renders without crashing", () => {
  const wrapper = shallow(<App />);
  console.log(wrapper.debug());
});
```

Jest has it's own assertion library built in.

### Setting up Testing structures

```javascript
const setup=(props={},state=null)=>{
    return shallow(<App {...props}>)
}

```

### Setting up structures for Testing Connected Components

1. Create a `storeFactory` utility
2. Creates a testing store with app reducers
3. Add it as prop to the connected component
4. Use shallow to create a virtual DOM
5. use .dive() to get child component

We can also use `redux-mock-store` can test intermediate actions.

```javascript
```

### How moxios works

1. Test installs moxios
2. Axios will now send requests to moxios instead of http.
3. Test specifies moxios response.
4. Test calls action creator.
5. Action creator calls axios.
6. axios uses moxios instead of http for request.
7. Action creator receives moxios response from axios.

### Mental Model for Tests

1. Test Function Starts
2. Exits before Promise resolves

1) Assertion runs after promise resolves
2) After test has already passed

### What is a mock function

function that runs instead of a real function.

Jest replaces real function with mock.

Can assert on:

1. How many times mock ran during tests
2. with what arguments

### Triggering a Mock Function

1. Run componentDidMount()
2. Check to see that mock function runs.

In redux, we only spied on mock functions.
We can also make replacement functions.

### Mocks serve three purposes..

1. Prevent side effects
2. Spy on function to see when it is called
3. Provide return values

**We should not destructure import in non-test code**.

### Testing `useEffect`

We will trigger an update with Enzyme `setProps()`.

### Testing Context

1. Mock useContext and mock the return value

   Pro's: It is a isolated unit test
   We can use shallow

   Con's: We can only mock useContext once. It is very brittle as it will break out tests if we don't specify the order of the return value.

2. Wrap component in Provider in setup and set the value with the value prop.
   Pro's: Closer to the actual app
   Con's: Extra functionality , Tests depend on children of component under test



### Test run in Node

It is important to note that your tests run in the Node environment- not an actual browser. One of the missing this is a history API. 

Install `history` as a dev dependency.

```
npm install -dev history
```

### act()

To prepare a component for assertions, wrap the code rendering it and performing updates on act() call. This makes your test run closer to how React
works in the browser.


