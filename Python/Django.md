### Model in the MVC architecture

Models exist independent to the rest of the system and are designed to be used by any application that has access to them.
A View accepts user input , behaves according to the application's interaction logic, and returns a display that is suitable for users to access the data represented by models.

### Template

While views are technically responsible for presenting data to the user, The task of how that data is presented is generally delegated to templates.

### URL Configuration

As a framework for the web, Django provides a seperate layer of glue to make views available to the outside world at specific URLs.
By supplying a regular expression as the URL component, a single declaration can accommodate a wide variety of specific URLs, in a highly readable and maintainable manner.

### Reusbable Applications

One of the most valuable aspects of Django is its focus on application based development. Rather than building each site from scratch, developers should write applications for specific purposes and then combine them to build a site.

### Django is Python

Some of the most advanced Python advanced Python techniques that Django relies on are related to how Python constructs it's classes. When the python interpreter encounters a class definition, it reads its contents just as it would any other code. Python then creates a new namespace for the class and executes all the code within it, writing any vairable assignments to that new namespace. Class definitions generally contain variables , methods and other classes, all of which are basically assignments to the namespace for the class.

Once the contents have finished executing, Python will have a class object that is ordinarily placed in the namespace where it was defined where it is then passed around or called to create instances of that class.

### Building a class programtically

The constructor for type accepts three arguments which represent the entire class declaration:

1. name- The name provided for a class, as a string
2. bases- A tuple of classes in the inheritance chain of the class ; may be empty
3. attrs- A dictionary of the class namespace.

The following code demonstrates a way to declare a class at runtime,

```python

DynamicClass=type('DynamicClass',(),{'spam':'eggs
})


```

type is actually a metaclass-a class that creates other classes.

If a class definition includes a seperate class for its metaclass option, that metaclass will be called to create the class, rather than the built-in type object.

```python


class MetaClass(type):
   def __init__(cls,name,bases,attrs):
       for(name, value) in attrs.items():
           print('Name: %s' %name)


class RealClass(object, metaclass=MetaClass):
      spam='eggs'



class SubClass(RealClass):
     pass

```

### Class Declaration

```python

from django.db import models

class Contact(models.Model):

     name=models.CharField(max_length=255)
     email=models.EmailField()

```

The attribute classes are provided from that same based module and are instantiated when assigned to the model.

Django's HttpResponse object exhibits both of these behaviors, as well as mimicking an open file object.

### Dictionaries

A dictionary is a mapping between keys and values within a single object. Python provides a number of methods for more fine grained manipulation of the underlying mapping.

**contains**(self, key)
Used by the in operator, this returns True if the speicified key is present in the underlying mapping, and returns False otherwise. this should never raise an execeptin

**getitem**(self, key)
This returns the value referenced by the specified key, if it exists.

**setitem**(self,key,value)

This stores the specified value to be referenced later by the specified key.

**iter**(self)
This method is called implicitly by iter() and is responsible for returning an iterator that Python can use to retrieve items from the object. The iterator returned is often implied by defining this method as a generator function.

```python

class Fibonacci(object):
      def __init__(self,count):
          self.count=count

      def __iter__(self):
          a,b=0,1
          for x in range(self.count):
              if x<2:
                 yield x
              else:
                 c=a+b
                 yield c
                 a,b=b,c


for x in Fibonacci(5):
    print x
```

For large collections, accessing items one by one is much more efficient than first gathering them all into list.

### Positional Arguments

Using a single asterisk before an argument name allows the function to accept any number of positional arguments.

```python

def multiply(*args):
   total=1
   for arg in args:
       total*=arg
    return total

>>> multiply(2,3)
6
```

Python collects the arguments into a tuple, which is then accessible to the variable args. If no positional arguments are provided beyond those explicitly declared, this argument will be populated with an empty tuple.

### Keyword Arguments

Python uses two asterisks before the argument name to support arbitary keyword arguments.

```python
def accept(**kwargs):
    for keyword, value in kwargs.items()
    print("%s--->%r"% (keyword,value))
```

### Decorators

Another common way to alter the way a function behaves is to "decorate" with another function. This is often called as "wrapping" a function , as decorators are designed to execute additional code before or after the original function gets called.

The key principle behind decorators is that they accept callables and return new callables. The function returned by the decorator is the one that will be executed when the decorated function is called later.

```python

def decorate(func):
    def wrapped(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapped


### Newer syntax

@decorate
def test(a,b);
   return a+b


test(12,72)
```

Typically, functions are called with all the necessary arguments at the time the function should be executed.

Sometimes, however arguments may be known in advance, long before the function will be called. 



