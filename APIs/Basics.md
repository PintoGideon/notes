### The absolute Basics

One web server may host many different URLs and each URL grants access to a different bit of the data on the server. When a web browser sends an HTTP request for a resource, the server sends the document in response. Whatever document the servers sends, we call that document a representation of the resource.

When you enter the URL http://www.youtypeitwepostit.com/ , the web server handles this request and sends a response. The response is a representation of the resource.

Every HTTP response can be split into three parts:

- The status code, sometimes called the response code
- The entity-body, sometimes called the body
- The response headers. The most important header is the Content-type which tells the HTTP client how to understand the entity-body.

### Collection + JSON

Content-Type:application/vnd.collection+json

When you make a GET request to http://www.youtypeitwepostit..com/api/, you don't just get any JSON document, you get a Collection+JSON document. Collection+JSON is a standard for publishing a searchable list of resources over the web. A server can't just serve any JSON document as application/vnd.collection+json. It can only serve a JSON object.

But not just any object, the object has to have a property called collection, which maps to another object:
{"collection":{}}

The "collection" object out to have a property called items that maps to a list:

```javascript

{"collection":{
    "items":[]
}}

```

The items in the "items" list needs to be objects:

```javascript

{"collection":{
    "items":[{},{},{}]
}}

```

Collection+JSON is a way of serving lists - not lists of data structures which you can do with normal JSON, but lists that describe the HTTP resources.

The collection object has an href property and its value is a JSON string. But it's not just any string- it's the url you just sent a GET request to:

```javascript


{
    collection:{
        href:'http://www.youtypeitwepostit.com/api/",
        "items":[{},{},{}]
    }
}
```

The collection+json defines this string as the "address used to retreieve the representation of the document". Each object inside the collection's items list has it's own href property and each value is a string containing URL.

### Writing to this API

To create a new item in the collection, the client first uses the template object to compose a valid item representation and then uses HTTP POST to send that representation to the server for processing. The server provides you with some kind of form (the template) which you will fill to create a document. Then you send that document to the server with a POST request.

```javascript

{
    "template":{
        "data":[
            {"prompt":"Text of message","name":"text"."value":""}
        ]
    }
}
```

To fill out this template, replace the empty string under value with the string I want to publish.

Then send the filled out template as part of an HTTP POST request:

```
POST /api/HTTP/1.1
Host: www.youtypeitwepostit.com
Content-Type:application/vnd.collection+json


```

### How Resources are born

To add a new item to the collection, you send a POST request to the URL of the collection. This isn't just how Collection+JSON does things. A resource is anything that's important enough to be referenced as a thing in itself. If your users might want to create a hypertext link to it, make or refute assertions about it, retrieve or cache a representation of it, include aall or part of it by reference into another representation, annotate it or perform other operations on it, you should make it a resource.

A resource is usually something that can be stored on a computer: An electronic document, a row in a database or thr result of running an algo. Architecture calls these information resources because their native form is a stream of bits. The server sends a representation describing the state of a resource. The client sends a representation describing the state it would like the resource to have.

### Resources with many representations

The Story so far: URls idenitify resources. A client makes HTTP requests to those URLs. A server sends representations in response and over time the client builds up a picture of the resource state, as seen through the representations. The missing piece is Hypermedia. Hypermedia connects resources to each other and describes their capabilities in machine readable ways.

Hypermedia is a way for the server to tell the client what HTTP requests the client might want to make in the future.

### URI templates

HTML's hypermedia controls have no way of telling a browser how to construct a URL like http://www.youtypeitwepostit.com/search/rest/ But URI Templates, a different hypermedia tech can do this. URI templates are defined in RFC 6570 and they look like this.

### What is Hypermedia for?

Hypermedia controls have three jobs:

1. They tell the client how to construct an HTTP request : what HTTP method to use , what URL to use , what HTTP headers and / or entity-body to send.
2. They make promises about the HTTP response, suggesting the status code, the HTTP headers and/or the data the server is likely to send in response to a request.

3. They suggest how the client should integrate the response into it's workflow.

### Guiding the request.

An HTTP request has four parts: the method, the target URL, the HTTP headers and the entity-body.

This HTML <a> tag specifies both the target URL and the HTTP method to use.

```html
<a href="http://www.example.com/">An outbound link</a>
```

The Target url is defined explicitly in the href attribute. The HTTP method is defined implicitly: The HTML spec says that an <a> tag becomes a GET request when the end user clicks the link.

Designs based on Hypermedia have more flexibility. Every time the client makes an HTTP request, the server sends a response explaining which HTTP requests make the most sense as a next step. If the server-side options change, the document changes along with it.

### The collection pattern

```javascript

{
    "collection":{
        "version":"1.0",
        "href":"http://www.youtypeitwepostit.com/api/",
        "items":[
            {
                "href":"http://youtypeitwepostit.com/api/messages",
                "data":[
                    {"name":"text","value":"Test"},
                    {"name":"data_posted","value":"2013-04-22"}
                ]
            }
        ]

    }

}
```

### What's a collection?

A collection is a special kind of resource. A resource is any thing important enough to have been given it's own URL.A resource can be a piece of data, a physical object or an abstract concept--anything at all. All that matters is that it has a URL and the representation- the document the client receives when it sends a GET request to the URL.

A collection resource is a little more specific than that. It exists mainly to group other resources together. It's representation focuses on links to other resources, though it may also include snippets from the representations of those other resources.

Similarly a resource that's described in a collection doesn't suddenly become a special thing called an "item".

The resource has it's own URL and an independent existence outside the collection.

### Collection+JSON

The collection+json standard defines a representation format based on JSON. The collection+JSON standard defines a representation format based on JSON.

It is basically an object with five special properties, predefined slots for application specific data:

- href is a permanent link to the collection itself
- items links to all members of the collection and partial representation of them.
- links Links to other resources related to the collection.
- queries Hypermedia contrils for searching the collection
- template A hypermedia control for adding a new item to the collection.

### Zooming in on items

```javascript

"items":[
    {
        "href":"/api/messages/21212",
        "data":[
            {"name":"text", "value":"Test"},
            {"name":"data_posted","value":"2012-02-01"}
        ],
        "links":[]
    }
]
```

I say that it's the most important field because it makes it clear which items are in the collection. In Collection + JSON, each member is represented by a JSON object. Like the collection itself, the collection itself, each member has a number of predefined slots that can be filled with application-specific data:

- href is a permanent link to the item as a standalone resource
- links Hypermedia links to other resources related to the item
- data Any other information that's an important part of the item's representation.

A member's href attribute is a link to the resource outside the context of it's collection.

If you GET the URL mentioned in the href attribute, the server will send you Collection+JSON representation of a single item.

```javascript

{
    "collection":{
        "version":"1.0",
        "href":"http://www.youtypeitwepostit.com/api/",
        "items":[
        {"href":"/api/messages/21212",
         "data":[
             {"name":"text","value":"Test"}
         ]
        }
        ]
    }
}
```

The simplest of Collection+JSON's hypermedia controls is the href attribute. I covered this earlier; it's a special link that provides a URL the client should use whenever it wants to refer to one specific item.

### The Write Template

Suppose you want to add a new item to a collection. What HTTP request should you make? To answer this question we take a look at the collection's write template.

```javascript

"template":{
    "data":[
        {"prompt":"text of message","name":"text","value":""}
    ]
}
```

The collection+json standard says you add an item to a collection by sending a POST request to the collection

```javascript
"href":"http://www.youtypeitwepostit.com/api",
```

So the post request will look like this:

POST /api/HTTP/1,1
Host: www.youtypeitwepostit.com
Content-Type:application/vnd.collection+json

{
"template":{
"data":[
{"prompt":"Text of the message", "name":"text", "value":"Squid!"}
]
}
}

That means the write template is conceptually equivalent to this HTML form

```html
<form action="http://www.youtypeitwepostit/api/" method="post">
  <label for="text">Text of the message </label>
  <input id="text" />
  <input type="submit" />
</form>
```
