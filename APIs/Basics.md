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

The Story so far: URls idenitify resources. A client makes HTTP requests to those URLs. A server sends representations in response and over time the client builds up a picture of the resource state, as seen through the representations.  The missing piece is Hypermedia. Hypermedia connects resources to each other and describes their capabilities in machine readable ways. 


Hypermedia is a way for the server to tell the client what HTTP requests the client might want to make in the future.

### URI templates


HTML's hypermedia controls have no way of telling a browser how to construct a URL like http://www.youtypeitwepostit.com/search/rest/ But URI Templates, a different hypermedia tech can do this. URI templates are defined in RFC 6570 and they look like this.

