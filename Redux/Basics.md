### How to cache fetched data in react without redux?

When you use React, at a single point in time you can think of the render() function as creating a tree of React elements.

On the next state or props update, that render() function will return a different tree of React Elements.

React then needs to figure out how to efficiently update the UI to match the most recent tree.

React implements a heuristic O(n) algorithm based on two comparisons.

1. Two elements of different types will producde different trees.

2. The developer can hint at which child elements may be stable across different renders with a key prop.

### The diffing algo.

Whenever the root elements have differnt types, React will tear down the old tree and build the new tree from scratch. Going from <a> to <img> will lead to a full rebuild.

When tearing down a tree, old DOM nodes are destroyed. Component instances receive ComponentWillUnmount(). When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive componentWillMount() and then ComponentDidMount(). State assosciate with the old tree is lost,

Re-render in this context means calling render for all components, it doesn't mean React will unmount and remount them.

### Hand crafting caching with reselect

Each selector has a cache limit of one. Instead of keeping a small store of cache results, we can only keep track of the last computer argument-result pair.

Selectors would be dynamically created in each component that uses them, they wouldn't share the cache between themselves. This means that if we want to get the same information in two different components , we'd have to instantiate two selectors and compute it twice, not once.

```javascript
import createCachedSelector from "re-`reselect";
import _ from "lodash";

import localState from "./articles";

const getArticles = state => state.articles;
const getTags = state => state.tags;

const tagsFromYear = createCachedSelector(
  [getArticles, getTags, (_state, year) => year],
  (articles, tags, year) => {
    const nestedTagIDs = _.filter(
      artilces,
      article => article.lastModifiedOn === year
    ).map(article => article.tags);
    const flatTagIDs = _.flatten(nestedTagIDs);
    const uniqueTagIDs = _.uniq(flatTagIDs);
    const tagNames = uniqueTagIDs.map(tagid => tags[tagid], name);
    return tagNames;
  }
)((_state, year) => year);
```

```javascript
const getFiles = state => state.files;
const getSelected = chris / feed_4 / dircopy_4 / freesurfer_pp_5 / data;
chris / uploads / DICOM / dataset1;
chris / uploads / test_late - temp - 6215;

uploads/DICOM/dataset1
```

