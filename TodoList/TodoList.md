Currently doing: Studying Redux Sagas

2. Make sense of Ellen's data
1. Use Immer in reducer to simplify state
3. Add resizable hook to the Feed Tree 
4. Optimize performance in the Feed Tree
5.

8. TS Plugin 
9. Design Version Numbers in the Add Node Wizard Better.


 

1. Replace Feed Graph Suspense Fallback component with A Skeleton

3. Web Workers for Rendering Feed Tree


Make use of the async hook.

```javascript
 
const {data:allItems, run} = useAsync({data:[], status:'pending'})

 React.useEffect(()=>{
 run(getItems(inputValue))
 },[inputValue, run])

```


4. Re-visit the react-virtual concept for file browser table
5. \.pdf$,\.pdf$,\.pdf$

Link to solve dependencies:

https://github.com/npm/normalize-package-data