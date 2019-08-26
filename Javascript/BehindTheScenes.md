### The Javascript Engine

JS engines are programs that convert JS code into something your computer can understand and execute. Modern browsers contain JS engines and so does Node.js

The first JS engine was SpiderMonkey.

The first version of SpiderMonkey was simply a JS interpreter.
An interpreter takes human-friendly JS source code , converts it into a machine-friendly intermediate, and executes it immediately. Interpretation doesn't literally happen line by line because JS and intermediate machine code are pretty different and the interpreter needs to "look ahead" to get the full context of the JS source code.

One downside of interpretation is that because it's a one pass process, it doesn't do any sort of code optimization that might make it run faster. 