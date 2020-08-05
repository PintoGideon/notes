### System

When you start operations and register JS functions,
you interact with the system.

The system is a part of the node process to keep track of outstanding operations and call JS functions when the operations complete.

The role of a Node programmer is to start operations,
register functions and return control back.

### Returning Control back

The responsibility of the system is to wait for an activity the program has expressed interest in to take place. When the activity happens, call the assosciate JS function.

Failing to return control back results in an unresponsive operation.

### Worker Threads

In a multithreaded program, the whole program is not suspended, but only the thread that is making the blocking call.

This way the main thread runs the Node system and executes callback is kept operational.

