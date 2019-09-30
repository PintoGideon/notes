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
**init** is the intializer and is where fields are defined.

def defines methods on the classes (instance and static)

```python
class Creature:
    def __init__(self,name,level):
         self.name=name
         self.level=level

    def walk(self):
        print('{} walks around'.format(self.name))
```

<<<<<<< HEAD
### Concept: Dictionaries

```python

lookup={}
lookup=dict()
lookup={'age':42,'loc':'Italy'}
lookup=dict(age=42, loc='Italy')

print(lookup)
print(lookup['loc'])

lookup['cat']='Fun code demos'

if 'cat' in lookup:
    print(lookup['cat'])
```

### A real world example

```python

import collections

User= collections.namedTuple('User', 'id,name,email')

users=[
  User(1,'user1','user1@talkpython.fm'),
  User(2,'user2','user2@talkpython.fm'),
  User(3,'user3','user3@talkpython.fm')
]

lookup=dict()

for user in users:
    lookup[user.email]=user

print(lookup['user3@talkpython.fm'])

```

Dictonaries store data by key and provide extraordinary performance on lookup.

### Concept: Lambdas

Lambdas are small inline methods

```python

def find_sig_nums(nums,predicate):
  for n in nums:
     if predicate(n):
         yield n

numbers=[1,2,3,5,8,13,21,24]
sig= find_sig_nums(numbers, lambda x:x%2==1)
```

### Programming 101

```python
# Calculating the average price of the house

prices=[]

for pur in  data:
    prices.append(pur.price)

avg_price=statistics.mean(prices)
print("The average price is ${:.}".format(int(avg_price)))

```

### Concept: List Comprehension

A procedural user search might look like this

```python
users=get_active_customers()
paying_usernames=[]

for u in users:
    if u.last_purchase==today:
       paying_usernames.append(u.name)
```

Using a list comprehension

```python
paying_usernames=[
  u.name #projection
  for u in get_active_customers(): #source
   if u.last_purchage==today  # filter

]

```

### Concept: Generator Expressions

```python

paying_usernames=(
  u.name
  for u in get_active_customers()
  if u.last_purchase===today
)

# () indicate a generator expression
```

### Concept: Data pipelines + generators

Task 1: Find all houses that match my requirements(3 bedroom, near a city etc)

Task 2: Find all houses that are for sale that I might buy in theory

Task 3: Find all houses that I might buy near me

```python

all_transactions=get_tx_stream()
interesting_tx=(
  tx
  for tx in all_transactions
  if is_interesting(tx)
)
```

### Concept: Classes vs Objects

Classes are blueprints for creating objects

```python
class Creature:
   def __init__(self,power):
      # ....

   def walk(self):
     # ...
```

Objects are created via Classes

```python
squirrel=Creature(7)
dragon=Creature(50)

squirrel.walk()
dragon.walk()

```

### Concept: Inheritance

Dragon is a specialization of Creature. Indicated via class Type(BaseType) syntax.

```python
class Dragon(Creature):
    def __init__(self,name,level,scale_thickness):
        super().__init__(name,level)
        self.scale_thickness=scale_thickness

    def breath_fire()
```

**_Aside_**
The strip() method returns a copy of the string in which all chars have been stripped from the beginning and the end of the string (default whitespace characters).

### Concept: Recursion

A recursive function calls iteself with modified data.

```python

def factorial(n):
    if n<=1:
    return 1

    return n* factorial(n-1)
```

### Concept:Generator Methods

```python

def fibonacci(limit):
   current=0
   current=1

   while current < limit:
        current,next=next,next+current
        yield current
```

The yield keyword returns one element of a sequence
After the item is returned and processed, execution returns and resumes.

***Aside***
The '.' assumes we are using the data directory right above the working directory
WS