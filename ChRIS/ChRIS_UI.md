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
// The wrapper returns this markup by passing down the following information to the Page Component from Patternfly

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
