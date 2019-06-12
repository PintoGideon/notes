### Async JS

const Http=new XMLHttpRequest();

```javascript
const url='https://.....'
Http.open('GET',url);
Http.send();

Http.onreadstatechange=function(){
if(this.readyState==4 && this.status==200){
console.log(Http.responseText
}

}

```

### Fetch Api

```javascript
const url="https://jsonplaceholder.typicode.com/post"

fetch(url).then(function response(data){
return data.json();
}).then(function displayData(response){
console.log(response)
}
```

**_With fetch, you can pass optional parameters_**

```javascript

const otherParam={
headers:{
    "content-type:"application/json; charset=UTF-8"
},
body:Data,
method:"POST"
}
```

### Axios

```javascript

const url='https://jsonplaceholder.typicode.com/posts";

axios.get(url).then(data=>console.log(data)).catch(err=>console.log(err))

axios.post({
    method:'post',
    url:url,
    data:{
        user
    }
})

```

### A small refresher on functions before going further

```javascript

function add(x, y) {
	return x + y;
}
add(3, 5);

 Passing a reference of add to const me
// It doesn't matter in JS, who invokes the function. We can create as many references to the functions we want.
const me = add;
me(4, 5);

function addFice(x, addReference) {
	return addReference(5, x);
}
addFive(5, add);
```

### Callbacks

In Callbacks, we are delaying the execution of a function until an event happens later on. This sounds like a great usecase for fetching data.

```javascript
axios({
	method: 'get',
	url: 'http://bit.ly/2mTM3nY',
	responseType: 'stream'
});
```

### Callback hell

Our brains think sequentially naturally and the above example is forcing us out of our natural way of thinking.

Inverstion of control- The way that we interface with a lot of API's is that we pass our callback function as a parameter. We are assuming the third party library will invoke the function in a responsible manner. But the problem is we don't have control over the function invocation. If the underlying api changes to which they call
our function differently, our app breaks.

### Promises

-pending
-fulfilled
-rejected

Promises exist to make the complexity of async requests more manageable.

```javascript
const promise = new Promise((resolve, reject) => {});
```

When the status of the promise changes to fulfilled, the function that you pass to then is invoked. When the status of the promises to rejected when we call reject, the function passed to catch will be invoked.

```javascript
const promise = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(); //fulfilled
	}, 2000);
});

promise.then(onSuccess);
promise.catch(onError);
```


