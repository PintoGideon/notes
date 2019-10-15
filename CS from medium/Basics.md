Source: https://medium.com/basecs/bits-bytes-building-with-binary-13cb4289aafa

### Converting into Binary

Let's try converting the number 27 (in base 10) into binary (base 2)

![Binary_Converstion](https://user-images.githubusercontent.com/15992276/66148729-1f89ef80-e601-11e9-90b7-0505a056128f.jpeg)

### Converting out of Binary

![Base_Conversion](https://user-images.githubusercontent.com/15992276/66148730-1f89ef80-e601-11e9-9408-5f8a1166d006.jpeg)

### How do computers read binary?

A computer has billions of tiny digital circuits, which are incredibly simple. They are made up of switches. And a switch can only have two states: on and off. Another way to think about this is true or false. And we can represent that one/off in binary in yet another way: 1 and 0

What’s even cooler is that everything in computers (and computer science, at that!) is, at the most rudimentary level, based on this on/off paradigm. Little bursts of electricity either pass through or don’t pass through based on whether something is switched on or switched off.A single digit in binary is known as a binary digit. But, you might know it as a bit. Since we know that binary is base 2, and one digit can only ever be either a 0 or a 1, we also can deduce that a bit can only ever be comprised of either a 0 or a 1.

What that means is that our computer has to do everything by building binary numbers, which means using only 0 and 1 and stringing them together. Which seems kind of insane! But it can build bits on top of other bits. And that’s exactly what it does. It strings together 8 bits (8 digits) into a byte. We might have already heard the term “byte” thrown around, or perhaps seen it on Stack Overflow. A byte is so common in the way that computers interpret binary that it is considered a unit of computer memory

I think that bytes are particularly interesting because a single byte can represent 256 different combinations. (Remember powers of 2? 2 to the power of 8 is 256.) And what about if you have two bytes? Two bytes means 16 bits (binary digits), which means that you can represent 65,536 different combinations (2 to the power of 16)! That’s a whole lot of different permutations that you can represent with just 2 bytes! If you think about a single circuit (often called transistors) handling an on/off switch per digit, just 16 transistors can process and interpret a ton of information!

![Byte](https://user-images.githubusercontent.com/15992276/66148728-1f89ef80-e601-11e9-91bd-e19990835be4.jpeg)

Bits, the building blocks of bytes, are incredibly fundamental and worth understanding. They’re important because different computers can process a different number of bits at a time. An 8-bit machine, for example, breaks up and processes 8 bits at a time. A 16-bit machine would break up and process 16 bits at a time. The number of bits that are processed at a time are known as a computer word, so we can think of bits as the “letters” that make up a computer word. Most computers now have a word length of 32 or 64 bits. And now you know what that means: that your machine passes around and processes 32 or 64 bits at a time. In other words, your computer processes binary strings that are 32 or 64 digits long!

Let’s take a single character of a word. That character requires 8 bits (or 1 byte) in order to represent it. So, what about something longer? What about a page of text that’s somewhere around 1,000 words long? That would require a lot more bytes!

Source: https://medium.com/basecs/whats-a-linked-list-anyway-part-1-d8b7e6508b9d

### Linked Lists

One characteristic of linked lists is that they are linear data structures which means that there is a sequence aand an order to how they are constructed and traversed.

In non-linear data structures, items don't have to be arranged in order, which means that we could traverse the data structure non-sequentially.

When an array is created, it needs a certain amount of memory. If we had 7 letters that we needed to store in an array, we would need 7 bytes of memory to represent that array. But, we’d need all of that memory in one contiguous block. That is to say, our computer would need to locate 7 bytes of memory that was free, one byte next to the another, all together, in one place.

The fundamental difference between arrays and linked lists is that arrays are static data structures, while linked lists are dynamic data structures. A static data structure needs all of its resources to be allocated when the structure is created; this means that even if the structure was to grow or shrink in size and elements were to be added or removed, it still always needs a given size and amount of memory. If more elements needed to be added to a static data structure and it didn’t have enough memory, you’d need to copy the data of that array, for example, and recreate it with more memory, so that you could add elements to it.

On the other hand, a dynamic data structure can shrink and grow in memory. It doesn’t need a set amount of memory to be allocated in order to exist, and its size and shape can change, and the amount of memory it needs can change as well.

The starting point of the list is a reference to the first node, which is referred to as the head. Nearly all linked lists must have a head, because this is effectively the only entry point to the list and all of its elements, and without it, you wouldn’t know where to start! The end of the list isn’t a node, but rather a node that points to null, or an empty value.

### Non-Linear Data Structures (Trees)

Source: https://medium.com/basecs/how-to-not-be-stumped-by-trees-5f36208f68a7

In non-linear data structures, the data doesn't really follow an order. Trees like linkedLists are made up of nodes and links.

In tree data structures, you must always have a root, even if you don't have anything else.
A root node can have links to multiple other nodes.

Some terms to note when talking about trees:

1. Root
2. Link/Edge
3. Child
4. Parent
5. Sibling
6. Internal
7. Leaf

### Truths about Trees.

1. If a tree has n nodes, it will always have one less number of edges (n-1).
2. Trees are recursive data structures. Trees can contain trees nested within them, since a child node could be the root of a subtree.

There are two properties that are the most important ones to talk about when it comes to what type of tree we deal with.

### Depth and Height

A simple way to think about the depth of a tree is by answering: How far away is the node from the root of the tree?
There is only one way to traverse or search through a tree by making a path and following the edges/links from the root node down.

A cool thing about the height property in particular is that the root node is automatically the height of the entire tree itself.

### Unearthing Tree roots

In order to truly appreciate the power of a tree, we have to look at them in the context of their application.

The process of assembling a tree is similar to the process of assembling a linkedList.

```python


class Tree:
     def __init__(self,cargo,left=None,right=None):
         self.cargo=cargo
         self.left=left
         self.right=right

    def __repr__(self):
         return str(self.cargo)


left= Tree(2)
right-Tree(1)

tree=Tree(1,left,right)

```
