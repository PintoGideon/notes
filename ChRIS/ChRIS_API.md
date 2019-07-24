The ChRIS Rest API uses the standard Collection+JSON hypermedia type to exchange resource representations with clients. 

### HATEOAS Links

Hypermedia as the Engine of Application State(HATEOAS) is a contraint of the REST application architecture that distinguished it from other network application
architectures.

```javascript

{  
  "links": [{
    "href": "https://api.paypal.com/v1/payments/sale/36C38912MN9658832",
    "rel": "self",
    "method": "GET"
  }, {
    "href": "https://api.paypal.com/v1/payments/sale/36C38912MN9658832/refund",
    "rel": "refund",
    "method": "POST"
  }, {
    "href": "https://api.paypal.com/v1/payments/payment/PAY-5YK922393D847794YKER7MUI",
    "rel": "parent_payment",
    "method": "GET"
  }]
}

```

In Recent years, REST has been at the forefront of the modern API design. Every enterprise has defined their own custom API format, usually a JSON response that
maps neatly to their own data model. A Facebook API client cannot communicate with a Twitter API and vice versa.
By using hypermedia in our responses we can offer links between API endpoints and documentation, potential actions and related enpoints. Furthermore, by standardizing on a
hypermedia type clients  developed for one API can understand the format of another API and communicate with minimal duplicated effort.

### The Model

```javascript
GET https://api.example.com/player/1234567890

Player {
    playerId;string,
    alias:string,
    displayName:string,
    url"https://api.example.com/player/1234567890/avatar.png"
}


And the list of this player’s friends could be retrieved with a separate API call.

GET https://api.example.com/player/1234567890/friends


[
{
    "playerId": "1895638109",
    "alias": "sdong",
    "displayName": "Sheldon Dong",
    "profilePhotoUrl": "https://api.example.com/player/1895638109/avatar.png"
},
{
    "playerId": "8371023509",
    "alias": "mliu",
    "displayName": "Martin Liu",
    "profilePhotoUrl": "https://api.example.com/player/8371023509/avatar.png"
}
]
```



Let’s take a look at how this API can be represented using hypermedia types.


# Collection + JSON

A collection object with a version and a URI pointing to itself.

### collection

The collection object contains all the records in the representation. The collection object should have a version property. The collecion object should have an href
property. The href property must contain an valid URI. This URI should represent the address used to retrieve a representation of the document.


```javascript

[
    "collection":{
        "version":"1.0",
        "href":URI,
        "links":[ARRAY],
        "items":[ARRAY],
        "queries":[ARRAY],
        "templates":{OBJECT},
        "error":{OBJECT}
    }
]
```
### template

```javascriot

{
    "template":[
        "data":[ARRAY]
    ]
}

```

### items
Each element in the items array SHOULD contain an href property. The href property MUST contain a URI. This URI MAY be used to retrieve a Collection+JSON document representing the associated item. It MAY be used to edit or delete the associated item.

```javascript
{
  "collection" :
  {
    "version" : "1.0",
    "href" : URI,
    "items" :
    [
      {
        "href" : URI,
        "data" : [ARRAY],
        "links" : [ARRAY]
      },
      ...
      {
        "href" : URI,
        "data" : [ARRAY],
        "links" : [ARRAY]
      }
    ]
  }
}
```

### data

The data array is a child property of the items array and the template object. The data array SHOULD contain one or more anonymous objects. Each object is MAY have any of three possible properties: name (REQUIRED), value (OPTIONAL), and prompt (OPTIONAL).

```javascript
// sample data array
{
  "template" :
  {
    "data" :
    [
      {"prompt" : STRING, "name" : STRING, "value" : VALUE},
      {"prompt" : STRING, "name" : STRING, "value" : VALUE},
      ...
      {"prompt" : STRING, "name" : STRING, "value" : VALUE}
    ]
  }
}
```



```javascript
GET https://api.example.com/player/1234567890
{
    "collection": {
        "version": "1.0",
        "href": "https://api.example.com/player/1234567890"
    }
}
```


Typically, the response would include a list of items in the collection. For a single resource, this collection would be a list of a single element. The properties of each element are given by explicit name/value pairs within a data attribute as in the following example.

```javascript
Player {
    playerId:string,
    alias:string,
    displayName:string,
    url:"https://api.example.com/player/1234567890/avatar.png"
}




GET https://api.example.com/player/1234567890
{
    "collection": {
        "version": "1.0",
        "href": "https://api.example.com/player",
        "items": [
            {
                "href": "https://api.example.com/player/1234567890",
                "data": [
                      { "name": "playerId", "value": "1234567890", "prompt": "Identifier" },
                      { "name": "name", "value": "Kevin Sookocheff", "prompt": "Full Name" },
                      { "name": "alternateName", "value": "soofaloofa", "prompt": "Alias" }
                ]
            }
        ]
    }
}

```

As the name would imply, Collection+JSON is uniquely suited to handling collections. Templates are one aspect of this. A template is an object that represents an item in the collection. The client can then fill in this template and POST it to the collection to add an element, or PUT it to update an existing item.

```javascript

GET https://api.example.com/player/1234567800/friends

{
    "collection":
    {
        "version": "1.0",
        "href": "https://api.example.com/player/1234567890/friends",
        "links": [
            {"rel": "next", "href": "https://api.example.com/player/1234567890/friends?page=2"}
        ],
        "items": [
            {
                "href": "https://api.example.com/player/1895638109",
                "data": [
                      {"name": "playerId", "value": "1895638109", "prompt": "Identifier"},
                      {"name": "name", "value": "Sheldon Dong", "prompt": "Full Name"},
                      {"name": "alternateName", "value": "sdong", "prompt": "Alias"}
                ],
                "links": [
                    {"rel": "image", "href": "https://api.example.com/player/1895638109/avatar.png", "prompt": "Avatar", "render": "image" },
                    {"rel": "friends", "href": "https://api.example.com/player/1895638109/friends", "prompt": "Friends" }
                ]
            },
            {
                "href": "https://api.example.com/player/8371023509",
                "data": [
                      {"name": "playerId", "value": "8371023509", "prompt": "Identifier"},
                      {"name": "name", "value": "Martin Liu", "prompt": "Full Name"},
                      {"name": "alternateName", "value": "mliu", "prompt": "Alias"}
                ],
                "links": [
                    {"rel": "image", "href": "https://api.example.com/player/8371023509/avatar.png", "prompt": "Avatar", "render": "image" },
                    {"rel": "friends", "href": "https://api.example.com/player/8371023509/friends", "prompt": "Friends" }
                ]
            }
        ],
        "template": {
            "data": [
                {"name": "playerId", "value": "", "prompt": "Identifier"},
                {"name": "name", "value": "", "prompt": "Full Name"},
                {"name": "alternateName", "value": "", "prompt": "Alias"},
                {"name": "image", "value": "", "prompt": "Avatar"}
            ]
        }

    }
}
```
### Queries

By defining the template and queries within the response Collection+JSON makes navigation by a new API user relatively simple without needing to understand the full meaning of the API. It also provides a level of interoperability between APIs using the Collection+JSON media type

### Reading Collections

To get a list of items in a collection, the client sends a HTTP GET request to the URI of a collection.A Collection+JSON Document is returned which contains one or more item objects in an array. 


```javascript

GET /my-collection/HTTP/1.1
Host:www.example.org
Accept:application/vnd.collection+json

```

```javascript
{
    "collection":{},....

}

```


### Adding an Item.

To create a new Item in the collection, the client first uses the template object to compose a valid item representation and then uses HTTP POST to send that representation
to the server for processing.

```javascript

{template:{"data":[......]}}

***RESPONSE***
201 Created
Location: http://www.example.org/my-collections/1

```

### Reading an Item

```javascript
GET /my-collection/1 HTTP/1.1
Host:www.example.org
Content-Type:application/vnd.collection.json

