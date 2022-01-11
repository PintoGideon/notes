### Cleaning up the mess

### Redux sagas needed ?

Create slice

```javascript
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

interface CounterState {
  value: number;
}

const initialState: CounterState = {
  value: 0,
};

const counterSlice = createSlice({
  name: "Counter",
  initialState,
  reducers: {
    //ES6 object syntax
    incremented(state) {
      // We are not mutating state here as we are using immer under the hood
      state.value++;
    },
    amountAdded(state, action: PayloadAction<number>) {
      state.value += action.payload;
    },
  },
});

export const { incremented, amountAdded } = counterSlice.actions;
export default counterSlice.reducer;
```

```javascript
// Configuring the store

import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "../features/counter/counter-slice";
import { apiSlice } from "../../";

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: (getDefaultMiddleware) => {
    return getDefaultMiddleware().concat(apiSlice.middleware);
  },
});

export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;
```

```javascript
// In App.js

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

Create predefined hooks for react-redux toolkit

```javascript
import {TypedUseSelectorHook, useDispatch , useSelector} from 'react-redux';
import {RootState, AppDispatch} from './state';


export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector:TypedUseSelectorHook <RootState> = useSelector;


```

```javascript
import { incrementedActionCreator, amountAdded } from "../";
import { useFetchBreedQuery } from "../../";

const App = () => {
  const count = useAppSelector((state) => state.counter.value);
  const dispatch = useAppDispatch();
  const { data = [], isFetching } = useFetchBreedQuery();

  return (
    <>
      <Button
        onClick={() => {
          dispatch(incrementedActionCreator());
        }}
      >
        {count}
      </Button>
      <div>Number of Dogs fetched: {data.length}</div>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Pictures</th>
          </tr>
        </thead>
        <tbody>
          {
              data.map((breed)=>{
                  return (<tr key={breed.id}>
                    <td>{breed.name}</td>
                    <td><img src={breed.image.url}/></td>
                  </tr>)
              })
          }
        <tbody>
      </table>
    </>
  );
};
```

```javascript
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";
const DOGS_API_KEY = "test";

interface Breed {
  id: string;
  name: string;
  image: {
    url: string,
  };
}

export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({
    baseUrl: "https://api.thedogapi.com/v1",
    prepareHeaders(headers) {
      headers.set("x-api-key", DOGS_API_KEY);
      return headers;
    },
  }),
  endponts(builder){
      return {
          fetchBreeds:builder.query<Breed[], number|void>({
              query(limit=10){
                  return `/breeds?limit=${limit}`
              }
          })
      }
  }
});


export const {useFetchBreedsQuery}= apiSlice
```
