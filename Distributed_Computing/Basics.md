Source for the notes: https://medium.com/baseds/many-nodes-one-distributed-system-9921f85205c4

### Basics

A single system that does not communicate with others and functions on it's own is not a distributed system.
A single process on our computer is a single system which operates on it's own. If a process does not communicate with other processes, then it inherently isn't part of a larger system. 

A distributed system is nothing more than multiple entities that talk to one another in some way, while also performing their own operations. Such a system could be something as simple as smart sensor or a wireless plug in your house that captures and sends data through a wifi network, or even just a wireless keyboard or mouse that can connect to your laptop.

The individual entities in a distributed system are called nodes. If we think about a distributed system being a network of computing elements , then we can visualize that network as a graph made up of interconnected nodes.

Even though the operations within a node occur in order, the moment that multiple nodes have to work together in a distributed system, things can get a little messier. Once we move from a single system/a single node to a distributed system/multiple nodes, then it’s possible for the operations across a group of nodes to render in an incorrect order.
Part of the reason for the orderliness of operations within a node is due to the fact that each node within a system operates according to its own clock.


Source for the notes: https://medium.com/baseds/transparency-illusions-of-a-single-system-part-1-b01c25f7dddd

### Transparency in distributed systems

The transparency of the system is a major factor when it comes to designing a distributed system. Ultimately, transparency is responsible for determining the ways in which the nodes of a system will work together. 
