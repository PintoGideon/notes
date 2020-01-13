1. Fixing Feed Output Browser Refresh

2. Performance issue with Chris File Select (Creating a Cache)

3. Modal inconsistency

4. The Feed Graph window is too small and causes an error when the tree becomes too big.
















### Create Feed

```javascript
// State
data = {
  feedName: "",
  feedDescription: "",
  tags: [],
  chrisFiles: [],
  localFiles: []
};

<ChrisFileSelect
  files={data.chrisFiles}
  handleFileAdd={this.handleChrisFileAdd}
  handleFileRemove={this.handleChrisFileRemove}
/>;



```
