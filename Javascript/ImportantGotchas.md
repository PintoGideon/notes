
Side effects come in two flavours: reading and writing. When you're expecting an input from a console, you can't predict the exact value that will come – why would you need an input otherwise. And when you're writing to a console, a file, a database... you're change (mutate!) some external state. Mutation is a standard term, describing a value (or variable) change. It's important to get that a) mutation is a side effect b) any writing side effect can be seen as mutation.


