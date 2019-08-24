

### HTML (Note from the official spec)

HTMl documents consist of tree elements and text. Each element is denoted by a start tag such as <body> and and end tag such as </body>.

Elements have attributes that control how elements work.
HTML user agents then parse this markup , turining into a DOM tree. A DOM is representation of a document.

DOM tress contain several kind of nodes, in particular a DOcument Type node, Element nodes, Text nodes , Comment nodes and in some cases ProcessingInstruction nodes.Each element in the DOM tree is represented by an object 
and these objects have APIs so that they can be manipulated. For instance, a link (e.g. The a element in the tree above) can have it's "href" attribute change in several ways:

```javascript
var a=document.links[0];
a.href='sample.html'
a.protocol='https';
a.setAttrbite('href', 'https://example.com')
```

### Important !!

When accepting untrusted input e.g: user generated content such as text comments, value in URL parameters, messages from third-party sites , etc, it is imperative that the data be validated before use, and properly escaped when displayed. Failing to do this can allow a hostile user to perform a variety of attacks, ranging from the potentially benign, such as providing bogus user info like a negative age, to the serious such as running scripts every time a user looks at a page that includes information, potentially propogating the attack in the process, to the catastrophic , such as deleting data in the server.

### Pitfalls to avoid

Scipts in HTML have "run-to-completion" semantics meaning that the browser will generally run the script uninterrupted before doing anything else scuh as firing further events or continuing to parse the document.

### DOM trees

A node A is inserted into a node B when the insertion steps are invoked with A as the argument and the A's new parent is B. Similarly, a node A is removed from B when the removing steps are invoked with A as the removedNode argument and B as the oldParent argument.















