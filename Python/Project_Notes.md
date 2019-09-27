Displaying a list with an index by converting into a tuple first.

```python

def list_entries(data):
    print("Printing Journal Entries......")
    entries = reversed(data)
    for idx, entry in enumerate(entries):
        print('*[{}]{}'.format(idx, entry))
```

### Import

Modules must be imported before using them.

```python
import os # retain namesspace
from os import path # get individual entities
from os import *  # import all direc

os.path.exists(filename)

```

### The Core Concept: File i/o

open creates a file stream, with adds auto-closing safely.

```python
items=['cat','mouse','mat']

with open(filename, 'w') as fout:
     for e in items:
        fout.write(e+'\n')
```

### Concept: Complex Conditionals

```python
if not x and (z!=2 or not y):
  # if code here
```

### Concept: Doc strings

```python

def some_method():
"""
Some method will book a hotel and flight for the user in one shot
"""

```

A string with triple-quotes at the beginning of the method/class/modules is a doc string.

This description is used in generating docs and intellisense.

```python

print(__file__) # gives you the full file path
print(__name__)

if __name__=='__main__':
     main()

```

### Concept: **name**

```python

module.py
print("Hello world. from {}".format(__name__))

program,py
import module

> python module.py
Hello world , from __main__

> python program.pu
Hello world, from __module__
```

### Concept: Python Package Index

The Python Package index is a repository of software for the Python programming language. There have 75883 pacakges currently.

### Concept: slicing

slices are like array access, but for ranges

```python
nums=[2,3,4,5,7,11,13,17,19,23]
first_prime=nums[0]
last_prime=nums[-1]


lowest_four=nums[0:4]
lowest_four=nums[:4]

middle=nums[3:6]  #[7,11,13]

last_four=nums[5:9]  #[13,17,19,23]
last_four=nums[5:]


last_four=nums[-4]
```

This is a programming idiom unique to Python.

### Concept:Named Tuples

Tupes group disparate types of data into one bound item

The comma defines the tuples.

```python
m=(22.5,44.234,19.02,'string')
temp=m[0]
quality=m[3]

### Alternatively

m=22.5,44.234,19.02,'strong'
print(m)
t,la,lo,q=m

# Tuples can be unpacked back into single variables
```

```python
# Named Tuple

Measurement=collections.namedtuple("Measurement",'temp,lat,long,quantity')
m=Measurement(22.5,44.234,19.02,'strong')
temp=m[0]
temp=m.temp
quality=m.quality
print(m)
```

### Concept:Virtual Envirnonments

```python
pip3 install virtualenv

python3 -m virtualenv ./local_env
cd local_env/bin
. activate
```


### Concept: class Structure

Classes are defined with the class Keyword. self is explicitly passed everywhere.
__init__ is the intializer and is where fields are defined.

def defines methods on the classes (instance and static)

```python
class Creature:
    def __init__(self,name,level):
         self.name=name
         self.level=level

    def walk(self):
        print('{} walks around'.format(self.name))
```        