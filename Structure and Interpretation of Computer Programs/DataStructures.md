
![Big O cheatsheet](https://user-images.githubusercontent.com/15992276/59464076-7de25680-8df5-11e9-9ab0-d1affb40d5d2.JPG)



### Stacks and Queues

1. Stack- Stores itms in a last-in first out (LIFO) order
2. Queue- Stores items in a first in, first out (FIFO) order

Examples: JavaScript engines have a call stack and a message queue that executes your code at runtime. When you hit 'undo' in your text editor or 'back' in your browser, you are using a stack.

### LinkedList

1. Organizes items sequentially with each item storing a pointer to the next
2. Linked Lists are often the underlying data strucuture for a stack or queue

```javascript

const linkedList={
head:{
    value:1
    next:{
        value:2
        next:{
            value:3,
            next:niull
        }
    }
  }
}

/*......Alternate......*/

class Node{
    constructor(val){
        this.value=value;
        this.next=null;
        this.previous=null;
    }
}

```

### Hash Tables

1. Organizes data for quick lookup on values for a given array

Examples: {}, Map, Set

### Implementation Details

**_Stack_**

```javascript
class Stack {
	constructor() {
		this._storage = {};
		this._length = 0;
	}
	push(value) {
		this._storage[this._length] = value;
		this._length++;
	}

	pop() {
		if (this._length) {
			const lastVal = this._storage[this._length - 1];
			this._storage[this._length - 1] = undefined;
			this._length--;
			return lastVal;
		}
	}

	peek() {
		if (this._length) {
			const lastVal = this._storage[this._length - 1];
			return lastVal;
		}
	}
}

const myStack = new Stack();
myStack.push('zero');
myStack.push('one');
```

### Queues

```javascript
class Queue {
	constructor() {
		this._storage = {};
		this._length = 0;
		this._headIndex = 0;
	}

	enqueue(value) {
		const lastIndex = this.length + this._headIndex;
		this._storage[lastIndex] = value;
		this._length++;
	}

	dequeue() {
		const firstValue = this._storage[this._headIndex];
		delete this._storage[this._headIndex];
		this._length--;
		this._headIndex++;
	}
}

var myQueue = new Queue();
myQueue.enqueue('one');
myQueue.enqueue('two');

/*

{0:'one'} length=1, headIndex=0
{0:'one', 1:'two'} length=2, headIndex=0;

*/

myQueue.dequeue();

/*
{1:'two'}  length=1,headIndex=1;
*/

myQueue.enqueue('three');

/*
length=length+headIndex
length=2
{1:'two', 2:'three'}
*/
```

### LinkedList

```javascript
class LinkedList {
	constructor(value) {
		this.head = {
            value,next:null;
        };
        this.tail=this.head;
    }

    insert(value){
        //update tail
       const node={
           value, next:null
       }

        this.tail.next=node;
        this.tail=node;
    }

}

const myList = new LinkedList(); //initiate??
```
