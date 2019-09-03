### Handling parameters when adding a node

When adding a plugin or pipeline, parameters may need to be set by the user. Pipelines have default values for each parameter-- plugins don't necessarily have default values for each parameter and some parameters are required.

While the target parent node is selected, click on a 'add new node' button in the right hand panel of the graph view.

A floating dialog appears docked to the right hand side of the screen. It will ask which plugin/pipeline to add, providing searchable lists of plugins/pipelines for the user to choose from. The user clicks next.

The second page of the floating dialog is the users opportunity to fill in required parameters without defaults as well as review the parameters that are optional and/or have defaults in order to provide values for those as well. The user clicks "add node"

### Mapping the workflow

### The Node Details Component

I get the selected plugin from the plugin state.

```javascript
const initialState: IPluginState = {
  selected: undefined,
  descendants: undefined,
  files: undefined,
  parameters: undefined
};
```

```javascript
async fetchPluginData(){
  const {id, plugin_id}=this.props.selected;
  const client=ChrisAPIClient.getClient();
  const plugin=await client.getPlugin(plugin_id);
  const params=await(await client.getPluginInstance(id as number)).getParameters();

  this.setState({
    plugin,
    params:params.getItems()
  })
}
```

### Add Node Component

In Add Node, I want the selected plugin and all the nodes in my feed

```javascript
const mapStateToProps = (state: ApplicationState) => ({
  selected: state.plugin.selected,
  nodes: state.feed.items
});
```

I want the selected node to be the parent.

The state of the Add Node Components looks like this

```javascript
this.state = {
  open: false,
  step: 0,
  data: {
    parentNode:{}
  },
  nodes: [{},{}.{}]
};

```

### Breaking down the AddNode Component

```javascript
<AddNodeModal>
<ScreenOne>
</AddNodeModal>

```

### The first screen in the Modal

I need the following details here as props from Add Node.

1. The Parent node (With a dropdown to choose parent from the list of plugins)

2. Types of nodes to add

3. Recently used Plugins

4. List of all plugins that can be added as a node

I am going to batch the last to into their seperate components

5. Basically we are creating a plugin

### A slight detour to analyze a JS implementation of adding a node.

```javascript
function Node(data) {
  this.data = data;
  this.children = [];
}

function Tree() {
  this.root = null;
}

Tree.prototype.add = function(data, toNodeData) {
  //Add data to toNodeData's children array

  var node = new Node(data); // create a node instance
  var parent = toNodeData ? this.findBFS(toNodeData) : null;
  if (parent) {
    parent.children.push(node);
  } else {
    if (!this.root) {
      this.root = node;
    } else {
      return "Root Node is already assigned";
    }
  }
};

Tree.prototype.findBFS = function(data) {
  var queue = [this.root];
  while (queue.length) {
    var node = queue.shift();
    if ((node.data = data)) {
      return node;
    }
    // Loop through the children node and add them to the queue to find them in the array

    for (var i = 0; i < node.children.length; i++) {
      queue.push(node.children[i]);
    }
  }
  return null;
};

var tree = new Tree(); //Create a new instance of the tree.
tree.add("ceo");
tree.add("cto", "ceo"); // Want to add cto to ceo's children's array
```

### Content Editable

```html
<div contenteditable="true">
  This text can be edited by the user
</div>
```

The goal with the parameter editor is to render an HTML

When an HTML element has contenteditable set to true, the document.execCommand() method is made available. This lets you run commands to manipulate the contents of the editable region. Most commands affects the document's selection for example, by applying a style to the text, while other insert new elements or affect an entire line. When using contentEditable, calling execCommand() will affect the currenly active editable element.

DOM space is the set of all web pages that you can express in HTML. All pages can be represented as a tree of elements, with text nodes as the leaves of those trees.

Visible space is the set of all visible pages. The browser's rendering engine is a mapping from DOM space onto Visible space.

Any selection in the text editor is expressed as two points. Each point is a paragraph index and text offset into that paragraph and a type. Most selections are text-type selections. We also have media-type selections.

With content editable , DOM===State

### Some good parts of Contenteditable

1. Works in all browsers
2. Native cursor and selection behavior
3. Native input events
4. Any rich text features we want
5. Automatic autogrowing
6. Accessible

My Goal- How do I implement this in react?

dangerouslySetInnerHTML

dangerouslySetInnerHTML is React's replacement for using innerHTML in the browser DOM. In general, setting HTML from code is risky, because it's easy to inadvertently exporse your users to a cross-site scripting (XSS) attack.

You can set HTML directly from React, but you have to type out dangerouslySetInnerHTML and pass an object with a \_\_html key to remind yourself it's dangerous.

The matchAll() method returns an interator of all results matching a string against a regular expression, including capturing groups.

### Chaining Plugins

- Run the first plugin
- Run the FS data through pl-freesurfer_pp to create a FreeSurfer output
- Run the FreeSurfer output through three parallel pl-mpcs plugin , each generating a different output
- Run each of the 3 outputs through a pl-z2labelmap to create on of three sets of heat maps.

- Get all the plugins
- Search for the plugin_id
- Create plugin instances
- the instance id is 1.
- We can query the status of the plugin job.
- Get a list of file produced by the plugin

Next in line is a Freesurfer plugin. Copy to it's output directory the results of a precalculated run. In this case, the pre-populated run corresponds to the DICOM data pulled by the FS plugin.

In the context of CUBE, determing the instance ID of the pl-freesurfer_pp.

Let's assume this is plugin id 14. The equivalent call to the above CLI snippet in CUBE, using the previous id 1 for incoming. becomes

```javascript
{
  "data":[
    {
      "name":"ageSpec",
      "value":"10-06-01
    }
    {
      "name":"previous_id",
      "value":"1"
    }
  ]
}



```

The instance id is 2.

Now we can call the pl-mcps plugin with a plugin_id of 11 using the previous instance id of 2.

```javascript
{
"data":[
  {"name":"random",
  "value":true
  }
  {
    "name":"previous_id",
    "value":"2"
  }
]

}
```

The createPluginInstance method takes the pluginID, request data object which is plugin-specific, data,previous.id.

```javascript

createPluginInstance(pluginId,{
{"name":"",
"value":""
},
{"name":"",
"value":""
}
}, previous.id)

```

The parameter list looks like this for every plugin

```javascript
[
  {
    default: "0",
    Flag: "--sleepLength"
  },
  {
    default: "",
    flag: "--prefix"
  },
  {
    default: false,
    flag: "--ignoreInputDir"
  }
];
```
https://codesandbox.io/s/yqlz84rnyv



