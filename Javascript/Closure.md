tl;dr

You have a closure when a function accesses variables defined outside of it.

```javascript
let users = ["Angelo", "David", "Gideon"];

let user = users.filter((user) => user.startsWith(query));
```
functions can access variables outside of them.

Source : https://whatthefork.is/closure