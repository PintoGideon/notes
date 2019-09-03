### FeedList View

1. Generate The Tabular UI
2. Fetch the feeds

3. Create a Feed

```javascript
async createFeed(){
    this.setState({saving:true});
}

const tempDirName=this.generateTempDirName();

```

2. Retrieving created Feed and entering the feed name provided by the user

```javascript

const feed= await createdInstance.getFeed();
if(!feed){
    throw 'New Feed is undefined'
}

await feed.put({
    name:this.state.data.feedName
})

// in the index.ts file

put:(data:IFeedData, timeout?:number)=>Promise<Feed>;

```

3. Similarly creating a feed object

```javascript
const feedObject = {
  name: this.state.data.feedName,
  note: this.state.data.feedDescription,
  id: feed.data.id,
  creation_date: data.creation_date,
  modification_date: data.modification_date,
  creator_username: data.creator_username,
  owner: [data.creator_username],
  url: feed.url,
  files: getLinkUrl("files"),
  comments: getLinkUrl("comments"),
  tags: getLinkUrl("tags"),
  taggings: getLinkUrl("taggings"),
  plugin_instances: getLinkUrl("plugininstances")
};

// In the index.ts file

static getLinkRelationUrls:(obj:object, relationName:string)=>string[];

this.props.addFeed(feedObject);
```

4. Dispatching an action

```javascript
//

mapDispatchToProps = (dispatch: Dispatch) => ({
  addFeed: (feed: IFeedItem) => dispatch(addFeed(feed))
});
```

5. In the Reducer

```javascript
export const addFeed = (feed: IfeedItem) =>
  action(FeedActionTypes.ADD_FEED, feed);
```

6. Add the Feed to the existing list of feeds in the array

```javascript

case FeedActionTypes.ADD_FEED:{
    if(state.feeds){
        return{...state,feeds:[action.payload,...state.feeds]}
    }else{
        return{..state,feeds:[action.payload]}
    }
}
```

### How do I think about Add Node then?

1. Basically I am creating a plugin with a plugin name, and a paramter list.

2. The parameter editor is initially supplied with the default parameters.

3. The parameter list has default values as string. The goal is to parse User's input i.e text into an HTML string with classNames for highlights during comparisons.

4. I want to compare the input string with the plugin's output directory

5. Also, at the same time, I want to highlight text that is not present in the output dir. Hence, the input needs to have a className.

```javascript

getPluginInstances(params){

}

```

I receive back a list of list plugin instances. Let's see how my state looks like.

```javascript
{
  plugin: pluginReducer;
}
```

Here is how the reducers looks like.

```javascript
const intialState = {
  selected: undefined,
  descendants: undefined,
  files: undefined,
  parameters: undefined
};
```

Going through the plugin saga to understand how the api behaves. Let's first capture Rudolph's workflow.

If you make a call to the url "http://localhost:8000/api/v1/plugins", we receive a collection.

We can run the the containter. "http://localhost:8000/api/v1/1/instances.

### A plugin workflow

- Get a list of plugins and check for testPlugin (http://localhost:8000/api/v1/plugins/${pluginId})
- Get info on the parameters of the plugin (http://localhost:8000/api/${pluginId}/parameters)
- Run an instance of the plugin (http://localhost:8000/api/v1/plugins/$(pluginId)/instances)
- Query job state ( and also trigger a file registration in CUBE)
- Examine the returned id.
- Call (http://localhost:8000/api/v1/plugins/instances/${pluginId}/) which if the job was successful will return a collection of data.
- The status should be finishedSuccessfully. On first attempt at asking the status of a successfully completed job, CUBE might block for a second or two as it actually registers files in its database.

### A caveat or a catch here to be careful of

There are two types of plugin apps: "fs" and "ds". The former is given to plugin apps that when run will create a new data feed. They don't require a value for the "previous" property of the template object as they are the first app in a pipeline of plugin apps. In opposition, plugin apps of type "ds" require the previous application of another plugin app and act on the outputs of that app.

### Some relevant info for the code to write from Jorge's API design (Top Down approach)

- FeedList ( You can make a GET request to /plugins to receive a plugin List from Feed)
- PluginList ( You can make GET request to for an item which would be a plugin)
- Every Plugin (You can make a GET request to create instances of the Plugin)
- PluginInstanceList (You can make a GET request to get a particular plugin instance)
-

### How has it been done so far?

### The Feed List View Component

When this component mounts, I want to render a table with the current feeds.

I can click on a particular feed and work on it.

### The Feed View Component

The Feed View branches out into three components

- The Feed Graph
- The Node Details
- The Feed Output browser

The Feed Graph needs the plugin from the feed details list.

When the Feed view component mounts, I want the entire feed list data.

This is how the function looks in the saga:

```javascript
function handleGetALlFeeds(action:IActionTypeParam){
    const {name, limit, offset}=action.payload
    try{
        const query=`limit=${limit}&offset=${offset}`;
        const url=!!action.payload.name ? `${process.env.REACT_APP_CHRIS_UI_URL}search/?name=${name}&${query}` : `${process.env.REACT_APP_CHRIS_UI_URL}?${query}`;

        if(res.error){
            console.error(res.error);
        }
        else{
            yield put(getAllFeedsSuccess(res));
        }
    }
    catch(error){
        console.error(error);
    }
}
```

```javascript
const initialState = (IFeedState = {
  details: undefined,
  items: undefined,
  feeds: undefined
});
```

Feeds is now an array of objects. I am just displaying some of the important poperties of the feed object.

```javascript
[
  {
    comments: "http://fnndsc.childrens.harvard.edu:8001/api/v1/2/comments/",
    files: "http://fnndsc.childrens.harvard.edu:8001/api/v1/2/files/",
    note: "http://fnndsc.childrens.harvard.edu:8001/api/v1/note2/",
    plugin_instances:
      "http://fnndsc.childrens.harvard.edu:8001/api/v1/2/plugininstances/",
    url: "http://fnndsc.childrens.harvard.edu:8001/api/v1/2/"
  },
  {}
];
```

I pass down the Feeds object to the feed view because that is where the meat of the stuff happens.

I need the individual feed item. Again through a fetch request, I
need my individual feed's details, Once the request is made, we get a collection. It has a property of plugin instances pointing to a URL.

```javascript
{
  plugin_instances: "http://fnndsc.childrens.harvard.edu:8001/api/v1/1/plugininstances/";
}
```

We call the plugin instances as well, I get an array of objects which are plugin instances. Every plugin has a status.

```javascript

[
    {
        url:"http://fnndsc.childrens.harvard.edu:8001/api/v1/plugins/instances/1",
        descendants:'http://fnndsc.childrens.harvard.edu:8001/api/v1/plugins/instances/1/descendants',
        files:"http://fnndsc.childrens.harvard.edu:8001/api/v1/plugins/instances/1/files/",
        parameters:"http://fnndsc.childrens.harvard.edu:8001/api/v1/plugins/instances/1/parameters/",
        plugin_id:11,
        previous_id:1
        status:"finishedSuccessfully",
        feed:"http://fnndsc.childrens.harvard.edu:8001/api/v1/1/"

    },
    {

    },
    {

    }
]

```

So to recap , the intial state comprises of a list of feeds, a list of plugin instances, and feed details.

So you can run the plugins by making a get request to the url provided above.

### Back to the UI

The Feed Tree.

We pass in the items object to the feed graph.

We create an instance of a new tree using the Tree model. A samall gotcha here. The previous id point to the parent plugin. If the previous id is null, it's a parent node.

```javascript
class TreeModel {
  constructor(items) {
    this.treeChart = {
      nodes: [],
      links: [],
      totalRows: 0
    };
  }
}
```

Whenever, the user clicks on a given node, a request is made to fetch plugin details.

```javascript
export const getPluginDetailsRequest = (item: IPluginItem) =>
  action(PluginActionTypes.GET_PLUGIN_DETAILS, item);
```

We make a request to the url's of item's descendants, item's files and item's parameters.

We set the first node as selected in the Descendants list. Here selected is the intital plugin state for more clarity. The PluginInstanceDescendantList is the list of all plugin instances that have this plugin instance as an ancestor in a pipeline tree. PluginInstanceParameter is the item resource object representing a parameter that the plugin instance was run with.

```javascript
const initialState = {
  selected: undefined,
  descendants: undefined,
  files: undefined,
  paramters: undefined
};
```

Coming back to the Feed View, we set the active node in the feed tree.

Now coming back to the FeedOutputBrowser and Node Details.
Let's debug node details first. I pass down the selected node and the descendants (The plugin instance list).

I want the following information to display of the selected plugin instance.
-createdDate.
-status
-plugin name

The logic here could be thought of like this. When we install a extension on our machine in VS code, we install an instance of it. It only has the info needed to run at that moment, To get additional info, make a request to get the plugin.

The Node details will pave the way to add a new node.

Looking at Matan's code for Add Node.

```javascript
interface AddNodeState={
    open:boolean,
    step:number,
    nodes?:PluginInstance[] //coverted props.nodes
    data:{
        parent?:PluginInstance,
        plugin?:Plugin
    }
}


```

I want to get the selected nodes and the all the plugin instances (items in the feed state).

The selected node will be the parent. All the plugin instances are placed in the tree as transformed nodes. (Could be possibly wrong?).

### 1st step

I want to list all the nodes so the user can select one of the nodes and configure it.

If a plugin is selected , I add it to the state. If a parent is selected, I add it to the state as a parent.

```javascript

data:{
    ...prevState.data,
    plugin
}
```

### 2nd Step

In the 2nd screen , I pass down the plugin. I want the default parameters from this plugin.

```javascript

async fetchParams(){
    const {plugin}=this.props;
    const paramlist= await plugin.getPluginParameters();
    const params=paramList.getItems();

    const paramString=this.generateDefaultParamString(params);
}
```

### The Parameter Editor

React provides the notion of implicitly allowing a child to store state(using the setState functionality).
It is also used to remember DOM state, any tiny ephermal state such as scroll position, text selection etch. It is also used for temporary state such as memoization.

### Back to solving the first issue

1. Parent node selection dropdow, include the instance's ID.

2. In PluginSelect, Only show DS plugins in plugin select list.

3. CreateFeed/BasicInformation--->FetchTagList

### Only show DS plugins

There are two types of plugin apps: "fs" and "ds". The former is given to plugin apps that when run will create a new data feed. They don't require a value for the "previous" property of the template object as they are the first app in a pipeline of plugin apps. In opposition, plugin apps of type "ds" require the previous application of another plugin app and act on the outputs of that app.

### What are my options to create a plugin?

The state of the Add Node Components looks like this

```javascript
this.state = {
  open: false,
  step: 0,
  data: {
    parentNode:{},
    plugin:{}
  },
  nodes: [{},{}.{}]
};


```

```javascript

await client.createPluginInstance(pluginId,{
    title:"test",
    previous_id:selected.id as number,
    paramstringkey:paramstringvalue
})



```

So some plugins can be created using no parameters. The code is successfully doing
that.

The content is generated by the user. we use dangerouslySetInnerHTML to keep the component in sync with its contents. When we use dangerouslySetInnerHTML, it lets React know that the HTML inside of that component is not something it cares about.

When React compares the diff against the actual DOM, it can straight up bypass checking the children of that node because it knows the HTML is coming from other source.

```javascript

npm install --save react-string-replace

```

--pfdcm --msg --indexList --priorHitsTable --PatientID --PACSservice orthanc --pullDirTemplate %SeriesInstanceUID --seriesSummaryKeys --seriesSummaryFile --studySummaryKeys

```javascript

getCommand(plugin:Plugin, params:PluginInstanceParameter[]){
const {dock_image, selfexec}=plugin.data;

let command=`docker run -v $(pwd)/in:/incoming -v $(pwd)/out:outgoing ${dock_image} ${selfexec}`

if(params.length){
  command+="\n" + params.map(param=>`--${param.data.param_name} ${param.data.value}`).join("\n");
}

command=`${command}\n /incoming /outgoing`.trim();

}
```


- I have a set of strings.
- I have a valid flag.

I want to check if the flag consists in the strings.

If no, check the current status of string. If the user types a string, I want the editor to scream that the path is not syntactically valid.


```



;
var