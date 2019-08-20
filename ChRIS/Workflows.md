### FeedList View

1. Generate The Tabular UI
2. Fetch the feeds

1. Create a Feed

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

mapDispatchToProps=(dispatch:Dispatch)=>({
    addFeed:(feed:IFeedItem)=>dispatch(addFeed(feed))
})

```

5. In the Reducer


```javascript

export const addFeed=(feed:IfeedItem)=>action(FeedActionTypes.ADD_FEED, feed )

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

3. The parameter list has default values as string. The goal is to parse User's input i.e text into an HTML string with classNames for highlights during string comparisons.

4. I want to compare the input string with the plugin's output directory

5. Also, at the same time, I want to highlight text that is not present in the output dir. Hence, the input needs to have a className.




