Strings in Python are immutable

```python
"testing".lower()
>>> 'testing'  # These are not the same strings in memory.
```

### Lists

A list is ordered sequence of items.

The length method calculates the number of items in this list.

```python
len(myList)

## Short hand for substrings

myList=[1,2,3,4,5]
myList[::2] # Here 2 is a step value
>>> 1,3,5

myList[0:1]
>>>1

myList.pop()
>>>5

```

**_Note: Lists are mutable_**

### Tuples and Ranges

Tuples are fixed-width immutable data structures.
You can index the tuples and make modifications to it.

```python

point=(2.0,3.0)
point_3D=point + (4.0)
```

### Dictionaries

Similary to javascript's object which are key value pairs.

```python
ages={'kevin':59,'alex':29,"bob":40}
ages.pop('alex')

ages.get('Kevin')
ages.keys() # Will give you the keys
ages.values() # Will give you values

```

### Boolean Operations

The logic is quite similary to Javascript

```python
firstName="Gideon"

firstName and print("Hey, does not seem like a very big learning curve right?")
>>> Hey, does not seem like a very big learning curve right?
```

### Some caveats in loops

This is similar to destructuring objects in Javascript

```python
family={'Gideon':26, 'Aaron':23, 'Stanny':60,'Sharmila':54}

for name, age in family.items()
  print(f" Member named: {name}")
  print (f"Member aged:{age}")

```

A dictionary is converted into a list of tuples when it is unpacked.

### Working with Files

**_Writing to a file_**

```python
my_file = open('xmen.txt', 'w+')
my_file.write('Beast\n')
my_file.write('Phoenix\n')
my_file.writelines([
    'Cyclops\n'
    'Bishop\n'
    'NighCrawler\n'
])

my_file.close()
```

**_Reading from a file_**

```python

my_file = open('xmen.txt', 'r')
print(my_file.read())
my_file.close()

```

A cursor in the file denotes the character in the file. If you read the file again using the same command, you will see no output as the cursor is at the end
of the line.
If you want to re-read the file, you have to manually move the cursor using the seek command.

### Evironmental Variables

```python
import os

stage = os.getenv['STAGE', 'dev'].upper()
output = "We are running in %s", stage

if stage.startswith("PROD"):
    output = "DANGER!!! -" + output

print(output)
```

### Error Handling in Python

```python
import sys

file_name = "recipes.txt"

try:
    my_file = open(file_name, 'x')
    my_file.write('Meatballs\n')
    my_file.close()

except:
    print(f'The {file_name} file already exists')
    sys.exit()

```

This is more of a global solution. Our error handling could be a bit fine-grained.

```python
import sys

file_name = "recipes.txt"

try:
    my_file = open(file_name, 'x')
    my_file.write('Meatballs\n')
    my_file.close()

except FileExistsError as err:
    print(f'The {file_name} file already exists')
    sys.exit()
except:
    print(f"Unable to write to the file")
    sys.exit(1)


```

### Decorators

This is a part of the functional Programming paradigm

Writing higher-order functions in python.
Parameters with an asterisk (\*) allows you to unpack or repack positional arguments.
(Probably like a rest operator in Javascript)

```python
def inspect(func, *args):
    print(f"Running {func.__name__}")
    val = func(*args)
    print(val)
    return val


def combine(a, b):
    return a*b


inspect(combine, 1, 2)
```

Function composition has a similar mental model to Javascript

Common Decorators we will be using in Python.

1. classmethod
2. staticmethod
3. property

```python
class User:
    base_url = 'http://example.com/api'

    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @classmethod
    def query(cls, query_string):
        return cls.base_url+'?'+query_string

    @staticmethod
    def name():
        return 'Kevin Bacon'

    @property
    def full_name(self):
        return f"{self.first_name}{self.last_name}"


user = User('Keith', 'Thompson')
user.base_url
print(user.full_name)
```

### Python Debugger

This will set a breakpoint in your code.

```python
import pdb

pdb.set_trace()

### Some helper methods

(Pdb) help next
(Pdb) help cont
(Pdb) n
(Pdb)list # The current line in the code

```

In Python 3.7, we also have a breakpoint function where you don't have to run pdb.set_trace()

### Classes

Classes allows us to hold some data and package some functionality.

```python
class Car:
    """
    Docstring describing the class
    """

    def __init__(self):
        """
        Docstring Describing the method
        """
        pass


my_car = Car()
print(my_car)  # <__main__.Car object at 0x7fc83b90bbe0>
# my_car is an instance of our Car
```

You can also add properties to a class once an instance is created.
The class instance is it's own context

### Composition

Building up classes by passing other instances of classes. Composition allows for seperation of concerns.

**_Note about doctests_**

```python
import math


class Tire:
    """
    Tire represents a tire that would be used with an automobile
    """

    def __init__(self, tire_type, width, ratio, diameter, brand='', construction='R'):
        self.tire_type = tire_type
        self.width = width
        self.ratio = ratio
        self.diameter = diameter
        self.brand = brand
        self.construction = construction

    def circumference(self):
        """
        The circumference of the tire in inches
        tire=Tire('P',205,65,15
        tire.circumference()
        80.1
        """
        side_wall_inches = (self.width*(self.ratio/100))/25.4
        total_diameter = side_wall_inches*2+self.diameter
        return round(total_diameter*math.pi, 1)

    def __repr__(self):
        """
        Represent the tire's information in the standard notation
        """
        return (f"{self.tire_type}{self.width}{self.ratio}")
```

Commands for running doctests

```python
cat tire.py

python3.6 -m doctest -v tire.py

```

### Inheritance

The idea of inhertiance is that you can create a class based off a different class.

```python
class SnowTire(Tire):
    def ___init__(self, tire_type, width, ratio, diameter, chain_thickness, brand='', construction='R'):
        super().__init__(tire_type, width, ratio,
                         diameter, brand, construction)

        # Tire.__init__(self, tire_type, width, ratio,
        # diamter, brand, construction)

        self.chain_thickness = chain_thickness

    def circumference(self):
        """
        The circumference of a tire w/ snow chains in inches
        >>>tire=SnowTire('P',205,65,15,2)
        92.7

        """
        total_diameter = (self._side_wall_inches() +
                          self.chain_thickness)*2+self.diameter
        return round(total_diameter*math.pi, 1)

    def __repr__(self):
        return super().__repr__() + "(Snow)"
```

### Working with Third Party Packages

pip is a primary installer while installing modules. On Linux systems, a Python installation will typically be included as part of the distribution. Installing into this Python installation requires root access to the system, and may interfere with the operation of the system package manager and other components of the system if a component is unexpectedly upgraded using pip.

On such systems, it is often better to use a virtual environment or a per-user installation when installing packages with pip.

```python
python3.4 -m pip install SomePackage  # specifically Python 3.4
pip3 freeze > requirements.txt
pip3 install --user -r requirements.txt
```

### Aspects of a Web App

1. Send HTTP Request
2. Route and handle
3. Return Response

### Working with Flask

```python
export FLASK_ENV=development
export FLASK_APP='.'
flask run --host=0.0.0.0 --port=3000
```

### Working with Postgres using Docker

```
docker run --rm   --name pg-docker -e POSTGRES_PASSWORD=docker -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data  postgres
```

— rm: Automatically remove the container and it’s associated file system upon exit. In general, if we are running lots of short term containers, it is good practice to to pass rm flag to the docker run command for automatic cleanup and avoid disk space issues. We can always use the v option (described below) to persist data beyond the lifecycle of a container

— name: An identifying name for the container. We can choose any name we want. Note that two existing (even if they are stopped) containers cannot have the same name. In order to re-use a name, you would either need pass the rm flag to the docker run command or explicitly remove the container by using the command docker rm [container name].

-e: Expose environment variable of name POSTGRES_PASSWORD with value docker to the container. This environment variable sets the superuser password for PostgreSQL. We can set POSTGRES_PASSWORD to anything we like. I just choose it to be docker for demonstration. There are additional environment variables you can set. These include POSTGRES_USER and POSTGRES_DB. POSTGRES_USER sets the superuser name. If not provided, the superuser name defaults to postgres. POSTGRES_DB sets the name of the default database to setup. If not provided, it defaults to the value of POSTGRES_USER.

-d: Launches the container in detached mode or in other words, in the background.

-p: Bind port 5432 on localhost to port 5432 within the container. This option enables applications running out side of the container to be able to connect to the Postgres server running inside the container.

-v: Mount \$HOME/docker/volumes/postgres on the host machine to the container side volume path /var/lib/postgresql/data created inside the container. This ensures that postgres data persists even after the container is removed.

### Connect to Postgres

```python
psql -h localhost -U postgres -d postgres
```

### SQLAlchemy

It is an object relational mapper which means we can take a relational database and we can create object instances based
on the rows.
