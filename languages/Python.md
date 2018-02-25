# Python 3

## Basics

- Programming language with a focus on readability
- Indentation defines code blocks
- .py files - code
- .pyc files - compiled code

## Minimal Working Example

program.py:
```python
#!/usr/bin/env python
print("Hello World")
```

## Packages

Install a package manager: **pip** (in Arch: `sudo pacman -S python-pip`)

Pip commands:
```bash
pip install --user <packagename>
pip install --user --upgrade <packagename>
pip uninstall <packagename>
```

Since you're installing packages in your user directories, you might need to add `PATH="$HOME/.local/bin:$PATH"` in `~/.bash_profile` or `~/.bashrc`.

### Useful packages:
- `pycodestyle`: linter according to [pep8 styleguide](https://www.python.org/dev/peps/pep-0008/)
- `flake8`: wrapper for advanced linting with pycodestlyle (styleguide) and pyflakes (logic errors like unused imports)

## Modules
common.py:
```python

def do_stuff():
    print("Hello World")

# Run automatically only when called via CLI
if __name__ == '__main__':
    do_stuff()
```

other.py:
```python

from common import do_stuff

# ...
```

## Functions
```python
def fn(name, surname="Doe"):
    # surname is an optional parameter
    print("Hello, {} {}!".format(name, surname))
    
# Anonymous functions (one-liners):
lambda arg1, arg2: return-expression
```

## Variables
```python
my_var = 2
stop = False

def mark_stop():
    global stop
    stop = True
```

## Booleans
```python
t = True
not(t)       # False
2 or True    # 2
False and 2  # False
```

## Strings
```python
hello = 'Hello'
world = "World"
length = len(hello)

print(hello + ' ' + world)

my_number_s = str(42)
print('The number is {}'.format(42))
```

My string quotes style:
- Use single quotes for everything; OR
- Use single quotes for internal values and double quotes for strings shown to the user

## Lists
```python
names = ['John', 'Jane', 'Joe'] # list
names.insert(0, 'Mike')
names.append('Jack')

numbers = [1, 2, 3, 4]
inc1 = list(map(lambda x: x + 1, numbers))
even = list(filter(lambda x: x % 2 == 0, numbers))

from functools import reduce
sum = reduce(lambda x, sum: sum + x, numbers)

fifteen_numbers = [i for i in range(15)]
fifteen_zeroes = [0 for i in range(15)]
```

## Dictionaries
```python
food = {'ham': 'yes', 'egg': 'yes', 'spam': 'no' }
```

## Classes
```python
class MyClass:
    """A simple example class"""
    i = 12345  # class variable shared by all instances
    
    @staticmethod  # optional, to also allow calling this method in the instance, e.g. x.stm()
    def stm():     # static method, called with MyClass.stm()
        return 'ola'

    def __init__(self, j):
        self.j = j  # instance variable unique to each instance
        self._private = 'something'  # convention for "private" variables
        self.__mangled = 'other'  # will be converted to _MyClass__mangled to avoid name clashes

    def f(self):
        return 'hello world'


x = MyClass()
x.f()

class DerivedClassName(BaseClassName, Base2, ...):  # inheritance
    pass

    def __add__(self, other):  # override + operator, others: https://docs.python.org/3/library/operator.html
        pass
```

## Conditionals and Loops
```python
if not stop:
    print("Not stopping now!")

for name in names:
    print("One of the names " + name)

for n in range(10):
    print("Loop number {}".format(n))
```

## Files
```python
with open('myfile.txt', 'w') as file:
    file.write('whatever')

with open('myfile.txt') as file:
    data = file.read() 
```
