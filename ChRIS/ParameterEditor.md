The draft-js editor needs some initial content. There are two required props.

1. The editorState
2. onChange

EditorState is an immutable record that represents the entire state of the Draft Editor.

1. The current Text content state
2. The current selection state
3. The full decorated representation of the contents
4. Undo/redo stacks
   5 The most recent type of change made to the contents

The RichUtils.toggleCode method lets us make a block into a code block.

### Writing a draft-js plugin

Draft-js plugins make it fairly easy to extends your editors functionality without coupling.

The keybindingFn method gets called for every key press. This provides an opportunity for us to return a new key command.

What makes draftjs truly pluggable that the entire functionality is exposed to plugins.
When we have handled a key command, we need to let draft.js know, so we return either the string 'handled' or true.

### Draft.js on the server

To be able to save the draft.js content to the server, you'll need to first produce a data structure that is persistable. The draft.js library comes with handy utility functions to serialize and unserialize it's immutable data structure to a plain JS object.

1. covertFromRaw converts raw JS object to contentState
2. convertToRaw converts contentState to raw JS object.

Here is the flow of persisting our contentState.

1. When content changes, editorState--->editorState.getCurrentContent()----->converToRaw---->JSON.stringify---->localStorate.setItem

2. When Component initalizes
   localStorage.getItem--->JSON.parse---->convertFromRa--->EditorState.createWithContent--->editorState

### Draft.js with an api

```javascript

componentDidMount(){
    fetch('/content').then(val=>val.json)
    .then(rawContent=>{
        if(rawContent){
            this.setState({editorState:EditorState.createWithContent(convertFromRaw(rawContent))})
        }
        else{
            this.setState({editorState:EditorState.createEmpty()});
        }
    })
}
```

### ContentBlocks & Decorators

ContentBlock is basically the equivalent of a paragraph. When the draft.js editor renders, it loops over all of the documents ContentBlock's and applies it's decorators.

Decorator is a plain object that has a strategy and a component.
The decorators concept is based on scanning the contents of a given ContentBlock for ranges of text that match a defined strategy, then rendering them with a specified React Component.

Decorator strategy:
The strategy is a function that receives two arguments, the current contentBlock and a callback. The callback is what tells the decorator which parts of the text to decorate, you need to pass it a start and end position.

For example, with "Hello World", the parameters to the callback would be (6,10). Typically we use a regex to find the start and end position.

When the decorator strategy invokes it's callback, it tells the editor to wrap the text inside the decorator component. The decorator component is a normal react component and the contents for a decorator component are passed through the children prop.

We also create a new regex containing our highlightTerm , this regex we'll use to find the text ranges that we want to decorate.
The findWithRegex is a function that we implement as well. It takes a regex, a contentBlock and our callback and will invoke the callback with the ranges for all the matches found.

### Draft.js explained by the engineers at Sendgrid

1. Powerful library that provides APIs for manipulating text styles withing a content editable HTML element.

### Editor State

EditorState is the top level editor state object , Draft.js utilizes immutable.js so it's technically a record which comprises.

1. The current text content state
2. The current selection state
3. The fully decorated representation of the contents
4. Undo/Redo stacks
5. The most recent type of change made to the contents

### Inline Styles

An inline style appears to a certain range of characters within a block. Examples of inline styles include the bold, italic, underline, font size and font color properties of the selected text. However styles like font size and font color map to a multitude of values where bold and italic can either be on or off.

### Entities

One might think of an entity as having a combination of both inline and block style properties. Entities correspond to a discrete range of characters like an inline style, but also provide a way to annotate these selections with metadata (e.g. like the data property described above) which can represent features and properties of the given selection.

Entities are often used in combination with decorators which can scan a block of text for the existence of an entity pattern (e.g. the link type or a matching href RegEx) and then wrap them in a custom component which defines how those entities should appear within the editor.

### Modifier

MThe Draft.js Modifier module provides a static set of methods for manipulating the content and styles of the current Editor State. The methods return a new content state object which can then be pushed on to the Editor State.

The Modifier module comes in handy when needing to build up a new content state on which to apply a new style. In our case, we use it extensively to remove multi-value styles like font size and font color before applying a new value of the respective style.

See the getResetStateForMultiOptionStyle() method below for an example of how this is done.


### Lessons Learned

