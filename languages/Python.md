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

Install a package manager: pip (in Arch: python-pip)

`pip install --user <packagename>`

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
```

## Dictionaries
```python
food = {'ham': 'yes', 'egg': 'yes', 'spam': 'no' }
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

