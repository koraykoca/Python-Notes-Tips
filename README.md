# Python-Notes-Tips

### Pythonic Tricks
- __*enumerate()*__ function returns a list of indices and values in a list. Use this function if you wanna walk through a list and at the same time keep track of the positions in a list.   
```python
for i, val in enumerate(myList):
    print(i, val)
```

- If you wanna walk through two lists at the same time, we can use the __*zip()*__ function. zip() function takes 2 or more lists and zips them together. We get the items which have the same index in their lists as pairs.  
```python
for x, y in zip(x_list, y_list):
    print(x, y)
```
- To swap values, there is a trick which is particular to Python. It's actually tuple (particular Python data type) entpacking. Write just:
```python
x, y = y, x
```
We can also compare to tuples directly. This also works with any kind of iterable objects such as lists: 
```python
a = (3, 5, 2)
b = (3, 5, 1)
msg = 'Update available' if a > b else 'Up to date' # inline if statement (for sequence comparision)
```

- We can use __*get()*__ function when you wanna get a value of a key from a dictionary, but you are not 100% sure that that key is in the dictionary, you can return a default value for it and prevent happening the Keyerror. 
```python
    myValue = myDict.get('myKey', 'defaultValue') # if myKey is not in the myDict dictionary, return defaultValue
```

We can use default dictionary from the collections package. It is pretty powerful object if we have a big dictionary. Default value is indicated by a function:
```python
import collections
d = collections.defaultdict(lambda : 0)  # lambda returns always 0 as default value in this case
d.update(myDict) # copy all the contents from myDict into the default dictionary d
myValue = d[myKey] # if myKey doesn't exist, return 0

# lambda : 0 is the same as
def dummy():
    return 0
d = collections.defaultdict(dummy)
```


- Python for loops have an else statement. Else statement is only executed if no break statement occured during the loop. 
```python
for name in myList:
    if name == 'Koray'
    break 
else: # if no break occured
    print('Not Found!)
```

- We can loop directly through a file object. It's an iterator. 
```python
f = open('a_file.txt') # file object, better way 
for line in f:
    print(f)
f.close()
```
We can use the __*with*__ statement when we are working with files. File is automatically closed after everything under the with statement is executed. No need to bother cleaning up. 
```python
with open('a_file.txt') as f: # file object, best way 
    for line in f:
        print(f)
```

- Try-except statements are very useful to prevent the whole program crash. We capture the error that occured, but let the program continue. 
```python
try: 
    ... # try to do something
except:
    ... # if it didn't worked, capture the error
else:
    ... # if it worked (if no-except / if no exception occured)
finally:
    ... # Always executed regardless of whether exception occured or not  
```

finally may be necessary when you don't use except and let the program crash if an error occured, but still want to execute something as final before the program crashes, because it is executed even if an exception occured. 
```python
try:
    ... # try to do something
finally:
    ... # execute even if the program crashes
```

- We can use inline if statements when if-else block is not complicated.
```python
    value = x if x > y else y
```

- Unpacking:
```python
fourNumbers = [1, 2, 3, 4]
first, _, _, last = fourNumbers

hundredNumbers = [1, 2, ..., 100]
first, *rest, last = hundredNumbers # everything in between first and last element should be captured in a variable called rest, works also with tuples
```

- Dictionary comprehensions:
```python
d = { key : value for key, value in zip(keyList, valueList) }

# This is the same thing as
d = {}
for key, value in zip(keyList, valueList): # explicit for loop
    d[key] = value
    
# or
d = dict(zip(keyList, valueList)) # dict factory function
```

- The order in dictionaries in Python is not preserved. It does change all the time. Order of elements in a dictionary changes randomly. If order is important, Python has an object called __*OrderedDict*__
```python
import collections
d = collections.OrderedDict() # values will be ordered starting from the smallest
for key, value in zip(keyList, valueList):
     d[key] = value  
```

### __init.py__
When importing the package, Python searches through the directories on sys.path looking for the package subdirectory. 
```python
import sys
for path in sys.path:
    print(path)
```
The __init__.py files are required to make Python treat directories containing the file as packages. This prevents directories with a common name, such as string, unintentionally hiding valid modules that occur later on the module search path. In the simplest case, __init__.py can just be an empty file, but it can also execute initialization code for the package or set the __all__ variable. The import statement uses the following convention: if a package’s __init__.py code defines a list named __all__, it is taken to be the list of module names that should be imported when __*"from package import *"*__ is encountered. For example, if __init__.py contains the following code: 
```python
__all__ = ["x", "y", "z"]
```
This would mean that from my_package import * would import the three named submodules of the my_package package.

Put a directory into the contents of the PYTHONPATH environment variable to make sure the directory is always on the Python sys.path list when you run Python. The PYTHONPATH variable has a value that is a string with a list of directories that Python should add to the sys.path directory list.
We can make the PYTHONPATH value be set for any terminal session, by setting the environment variable default. For this, add the command in ~/.bashrc:
export PYTHONPATH=$PYTHONPATH:<path>
    
### Dictionaries
Usually, a Python dictionary throws a KeyError if you try to get an item with a key that is not currently in the dictionary. The defaultdict in contrast will simply create any items that you try to access (provided of course they do not exist yet). To create such a "default" item, it calls the function object that you pass to the constructor (more precisely, it's an arbitrary "callable" object, which includes function and type objects). 
```python
from collections import defaultdict
d_int = defaultdict(int)  # default items are created using int(), which will return the integer object 0    
s = 'mississippi'
for k in s:
    d_int[k] += 1
    
>>> d_int.items()
[('i', 4), ('p', 2), ('s', 4), ('m', 1)]
    
d_list = defaultdict(list) # default items are created using list(), which returns a new empty list object.
s2 = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
for k, v in s2:
    d_list[k].append(v)
    
>>> d_list.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```
    
d.get(key, default) won't ever modify your dictionary – it will just return the default and leave the dictionary unchanged. defaultdict, on the other hand, will insert a key into the dictionary if it isn't there yet. 
    
### Data Classes: 
A utility tool to make structured classes specially for storing data. These classes hold certain properties and functions to deal specifically with the data and its representation. The DataClasses are implemented by using decorators with classes. Attributes are declared using Type Hints in Python which is essentially, specifying data type for variables in python. Without a __init__() constructor, the class accepted values and assigned it to appropriate variables.

Equality between two objects using == operator in python checks for the same memory location. Equality between DataClass objects checks for the equality of data present in it. 

```python
@dataclass
class Box:
    x:           float
    y:           float
    width:       float
    length:      float
    orientation: float
```
 
