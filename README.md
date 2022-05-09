# Python-Notes-Tips

## Data Classes: 
a utility tool to make structured classes specially for storing data. These classes hold certain properties and functions to deal specifically with the data and its representation. The DataClasses are implemented by using decorators with classes. Attributes are declared using Type Hints in Python which is essentially, specifying data type for variables in python. Without a __init__() constructor, the class accepted values and assigned it to appropriate variables.

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
 
