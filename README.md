# Python-Notes-Tips

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
 
