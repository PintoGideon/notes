### Top Level UI State

```javascript
export interface ApplicationState {
  ui: IUiState;
  message: IMessageState;
  feed: IFeedState;
  user: IUserState;
  plugin: IPluginState;
  explorer: IExplorerState;
  gallery: IGalleryState;
}
```

### The Dashboard Page

The Dashboard page needs the user , ui pieces of the state from the store so we create a container component that communicates with the Redux Store

```javascript

<Wrapper>
<PageSection variant={PageSectionVariants.darker}>
<h1>1st Child</h1>
</PageSection>

<PageSection>
<Alert>Welcome to Chris </Alert>
</Alert>
</PageSection>
</Wrapper>

```

A simple workflow to undestanding the working React, Patterfly and TypeScript.

```javascript
// The wrapper returns this markup by passing down the following information
//to the Page Component from Patternfly

onToggle = () => {
  const { isSidebarOpen, onSidebarToggle } = this.props;
  onSidebarToggle(!isSidebarOpen);
};

<Page
  className="pf-background"
  header={<Header onSidebarToggle={this.sidebarToggle} user={user} />}
  sidebar={<Sidebar />}
  onPageResize={this.onPageResize}
>
  {childrens}
</Page>;
```

```javascript
const mapStateToProps = ({ ui, user }: ApplicationState) => {
  return {
    isSidebarOpen: ui.isSidebarOpen,
    loading: ui.loading,
    user
  };
};

const mapDispatchToProps = (dispatch: Dispatch) => {
  return {
    onSidebarToggle: (isOpened: boolean) => dispatch(onSidebarToggle(isOpened))
  };
};
```

// Action is Dispatched and the reducer gives us a new state

### Understanding Login

```javascript
<Route path="/login" component={Login} />
```

1. Create a form
2. Handle Input
3. Handle Form submit
4. Dispatch a async action to create a token for the user

```typescript
<LoginPage>
  <LoginFormComponent />
</LoginPage>
```

```javascript
// A async action creator
const getAuthToken = () => action(UserActionTypes.FETCH_TOKEN, user);

//
```

### Create notes in detail

The browser renders the FeedView component when the user clicks on an individual feed link.

I needs the details of the feed. The feed Details component need not re-render hence we create a seperate component for it.

### Feed Details Component

I pass in details as the prop. The ChRIS JS api takes an object oriented approach in which every API resource object is an instance of a JS class that inherits from either or two base classes.

In particular every object inherits a get method that updates the internal state of the object with the new data fetched from the ChRIS server.

The feed details workflow works like this:

```javascript
import ChrisAPIClient from "../../../api/chrisapiclient";
import { IFeedItem } from "../../api/models/feed.model";

export interface FeedData {
  feedDescription: string;
}

const client = ChrisAPIClient.getClient();
```

Hey Jorge, I am working on displaying the note content
as a part of the Feed Description in the UI.

Please let me know if I am understanding this correctly.

Currently, the note.content can be called in such a way

const note= await feed.getNote();
const {content}=note;

For this, I need the feed from the client. However, the Feed class is currently created as a subclass of ItemResource.

I cannot call client.getFeed(feedId).






class FeedDetails extends React.Component<AllProps, INoteState> {
  constructor(props: AllProps) {
    super(props);
    this.state = {
      note_content: " "
    };
  }

  async componentDidMount() {
    const noteData = await this.fetchNote(2);
    this.setState({
      note_content: noteData
    });
  }