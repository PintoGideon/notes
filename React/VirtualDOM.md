### Virtual DOM

Here is a link to the code sanbox: [VDOM](https://codesandbox.io/s/epic-bird-vvx83)

Virtual DOM usually refer to plain objects representing actual DOMs.

1. Create element just creates a DOM element for you. You pass in the tagName, attributes and the children's array

2. Now the render function takes in the virtual DOM and translates it to a real DOM.

3. Now that we have our real DOM, it needs to be mounted on our page. The mount function just replaces our current node with the target node (obtained by document.getElementById).

If we stop here and use a setIntervalFunction as you see in the code, we are rendering the entire application to the real DOM which is expensive.

Elements will loose their states. For example, <input> will lose their focus whenever the application remounts to the page.

If we take a function diff(oldVTree, newVTree) which calculate the differences between two virtual trees; return a patch function that takes in the real DOM of oldVTree and performs appropriate operations to the real DOM to make the real DOM look like the newVTree.

Now the diff algorithm can have many case to cover:

1. newVtree is undefined
2. If they are both TextNode(string)
3. One of tree is textNode, the other one is ElementNode
4. oldVTree.tagName!==newVTree.tagName- In this case, we assume that the old and new trees are totally different. Instead of trying to find the differences, we will replace the \$node with render(newVTree);

```javascript
const vNewApp = createApp(count);
const patch = diff(vApp, vNewApp);
$rootEl = patch($rootEl);
vApp = vNewApp;
```

**_The Array.from() method creates a new shallow copied array instance from an array-like or iterable object_**

```javascript
console.log(Array.from('foo'));
///["f","o","o"]

console.log(Array.from([1, 2, 3], x => x + x));
// [1,4,6]
```
