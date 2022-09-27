# Python-Notes-Tips

* Everything in Python is an object (even types such as int and str.), and each object is stored at a specific memory location. This is true on an implementation level in CPython. Every value assigned to a variable in Python exists as an object on the heap. It takes time to allocate these objects because our memory manager needs to do work. When an integer is assigned to a variable in Python, a corresponding PyObject needs to exist on the heap.  There is a struct (a custom data type that groups together different data types in C, it’s like a class with attributes and no methods) called a PyObject, which every other object in CPython uses. You can use id() to check the identity of an object.

* REPL stands for Read, Evaluate, Print, Loop. The REPL is how you interact with the Python Interpreter. Unlike running a file containing Python code, in the REPL you can type commands and instantly see the output printed out. To start the REPL in VS code, open the command palette and search for and select “Start REPL”. If you’d like to start the REPL from the command line outside of the editor, type python in your shell. 
">>>" is the prompt. In example code, lines starting with >>> means they are input.

* if __name__ == '__main__':
It's a check if this file is being run directly by Python or it is being imported. If we run the script directly, run the code under this conditional. If we import this module, doesn't run it "implicitly" (otherwise every module we imported would be run by Python automatically because we imported it). Whenever Python runs a file, it first sets a few special variables before even runs any code, and __ name __ is one of those special variables. When Python runs a pyhton file directly, it sets __ name __ == __ main __. When we import a module, Python sets __ name __ to the name of the file. 


### Python Style Convention
* Variable, Object(Instance), Module names: lowercase_underscore
* Class names: CapWords
* Class member names, which are used only in the class (for internal use, not for external use) (private, protected members): _private_member

### Built-in Functions, Data Structures
* [Python’s Standard Library:](https://docs.python.org/3/library/functions.html) By default, Python comes with a lot of functionality that’s just an import statement away. 

* type(object_name): get type of the object
* isinstance(object_name, object_type): to test type of an object
* issubclass(object_type, object_type): to test whether a class is a subclass of another class

Function parameters: When you define a function, you use parameters. (formal parameters)
Function arguments: When you call a function, you pass arguments (values). (actual parameters)

You should always avoid using a mutable data type such as a list or a dictionary as a default value when defining a function with optional parameters.

When none of the parameters in a function definition has default values, you can order the parameters in any way you wish. The same applies when all the parameters have default values. However, when you have some parameters with default values and others without, the order in which you define the parameters is important. The parameters with no default value must always come before those that have a default value.

args and kwargs: It is possible to define a function that accepts any number of optional arguments. You can even define functions that accept any number of keyword arguments. Keyword arguments are arguments that have a keyword and a value associated with them.

When the asterisk or star symbol ( * ) is used immediately before a sequence, it unpacks the sequence into its individual components. When a sequence such as a list is unpacked, its items are extracted and treated as individual objects. There is nothing special about the name args. It is the preceding * that gives this parameter its particular properties.  Often, it’s better to use a parameter name that suits your needs best to make the code more readable.
```python
part_list = {}
def add_parts(part_list, *items_name):
    for item_name in items_name:
        part_list[item_name] = 1
    return part_list
    
part_list = add_parts(part_list, "screw", "nut", "washer", "oil")
```

When you display the data type, you can see that item_names is a tuple. Therefore, all the additional arguments are assigned as items in the tuple item_names. You can then use this tuple within the function definition as you did in the main definition of add_items() above, in which you’re iterating through the tuple item_names using a for loop.

When you define a function with parameters, you have a choice of calling the function using either non-keyword arguments or keyword arguments:
```python
def test_arguments(a, b):
    pass

test_arguments("BMW", "Porsche")      #  the arguments are passed by position
test_arguments(a="BMW", b="Porsche")  #  the arguments are passed by keyword (order is not important in this type of call)
```
The double star ( ** ) is used to unpack items from a mapping. A mapping is a data type that has paired values as items, such as a dictionary. The double star or asterisk operates similarly to the single asterisk you used earlier to unpack items from a sequence. The parameter name kwargs is often used in function definitions, but the parameter can have any other name as long as it’s preceded by the ** operator.

```python
part_list = {}
def add_parts(part_list, **items_to_add):
    for item_name, quantity in items_to_add.items():
        part_list[item_name] = quantity
    return part_list
    
part_list = add_parts(part_list, screw=1, nut=2, washer=2, oil=1)
```

Earlier, you learned that args is a tuple, and the optional non-keyword arguments used in the function call are stored as items in the tuple. The optional keyword arguments are stored in a dictionary, and the keyword arguments are stored as key-value pairs in this dictionary.

Generally, lists are for looping; tuples for structs. Lists are homogeneous; tuples heterogeneous. Lists for variable length. 
A tuple is an iterable and seems like simply a immutable list, it's really the Python equivalent of a C struct:
```c
struct{
    int a;
    char b;
    float c;
} foo;

struct foo x = { 9, 'k', 5.3 };
```
```python
x = (9, 'k', 5.3)
```

- By default, sorted() has sorted the input in ascending order, and the reverse keyword argument causes it to sort in descending order. It’s worth knowing about the optional keyword argument key that lets you specify a function (usually lambda) that will be called on every element prior to sorting. Adding a function allows custom sorting rules, which are especially helpful if you want to sort more complex data types e.g. dictionaries:
```python
cars = [{"Car": "Pagani", "power": 850},
        {"Car": "Porsche", "power": 450},
        {"Car": "BMW", "power": 250},
        {"Car": "Ferrari", "power": 650},]
print(sorted(cars, key=lambda dl: dl["power"], reverse=True))

cars_d = {"Pagani": 850, "BMW": 250, "Ferrari": 650, "Porsche": 450}
print(sorted(cars_d.items(), key=lambda x:x[-1]))
```

- requests library: It allows us to make http requests to get information from websites. For example, requests.get is used to go to a website and pull down the required informations from the website. If it returns ok, then we can get the information with text() method:
```python
import requests
def get_info():
    response = requests.get(f"<url>")   
    if response.ok:  # response.status_code < 400 responses
        return response.text  # content of the response, in unicode
    else:
        return "Bad Response"
```
To test the library, there is [this website](http://httpbin.org/) from the developer of the library. 

Python has JSON (JavaScript Object Notation) standard library. JSON is very common data format for storing some Information. JSON is used a lot when fetching data from online APIs. It can also be used for configuration files. [It's very similar to Python dictionary.](https://docs.python.org/3/library/json.html#encoders-and-decoders) 

load(), loads() -> to get Python object (dictionary) from JSON object

dump(), dumps() -> convert from Python object to JSON object

### Object-Oriented Programming (OOP)
OOP is a programming paradigm, or a specific way of designing a program. It allows us to think of the data in our program in terms of real-world objects, with both properties and behaviors. These objects can be passed around throughout our program. Properties define the state of the object. This is the data that the object stores. This data can be a built-in type like int, or even our own custom types we’ll create later. Behaviors are the actions our object can take. Oftentimes, this involves using or modifying the properties of our object.

Custom objects are mutable by default. An object is mutable if it can be altered dynamically. For example, lists and dictionaries are mutable, but strings and tuples are immutable.

### Classes 

Aristotle once said, "Knowing self is the beginning of all wisdom."
```python
class BMW:
    def get_power(self):
        pass
        
car = BMW()
#  following two lines do the exact same thing
car.get_power() # instance (car = self) is getting passed automatically as the first argument in regular class methods
BMW.get_power(car)  # we need to pass the instance to the function manually
```

* def __ init __ (self): (Constructor) first function that is called when an instance of the class is created. The functions which starts with double underscores are called magic functions (because they have special meaning to Python) or dunder functions (because of double underscores). The properties that all class objects must have are defined in this method. Every time a new object of the class is created, __ init __() sets the initial state of the object by assigning the values of the object’s properties. That is, __ init __() initializes each new instance of the class.

* Instance attributes: Attributes created in __ init __() are called instance attributes. An instance attribute’s value is specific to a particular instance of the class. 
* Class attributes: Attributes that have the same value for all class instances. We can define a class attribute by assigning a value to a variable name outside of __ init __() (usually before of it). Class attributes are defined directly beneath the first line of the class name and are indented by four spaces. They must always be assigned an initial value. When an instance of the class is created, class attributes are automatically created and assigned to their initial values. When we want to access a class attribute in a method, we need to either access it through the class itself (class namespace) or an instance of the class (self). Accessing with self is generally preferrable because that will give us the ability to change the value of the class attribute for a single instance (overwriting). However, if you want a variable have the same value for all instances of the class, then you should access it through class itself, instead of self. 
```python
class BMW:
    car_brand = "BMW"  # class attribute
    def __init__(self, color):
        self.color = color  # instance attribute     
    def show_brand(self):
        print(f"Brand is: {self.car_brand}")  # accessing with self
```

Use class attributes to define properties that should have the same value for every class instance. Use instance attributes for properties that vary from one instance to another. Use attributes and methods to define the properties and behaviors of an object.

* You can access the parent class from inside a method of a child class by using super(). super() searches the parent class for a method or an attribute. For example, you can use super() to call __ init __() method or any other method of inherited class. It's specifically useful when you make multiple inheritance. When you inherit from one class, using super() or calling the base class __ init __() function doesn't make a difference. When we call the init() method of the parent class, we let that metheod to handle the common arguments e.g. model_name argument in the below example:
```python
class BMW(Car):
    def __init__(self, model_name, extra_param):
        super().__init__(model_name)  # or Car.__init__(self, model_name) 
        self.extra_param = extra_param  # we handle the new parameter manually
```

Regular (instance) methods need a class instance and can access the instance through self. They can read and modify an objects state freely. Class methods don’t need a class instance. They can’t access the instance (self) but they have access to the class itself via cls. Static methods don’t have access to cls or self. They work like regular functions but belong to the class’s namespace.

* __*property*__ (attribute, instance variable, field): It's a value that is part of an object. It's kind of method, but you don't call it like function with (). @property decorator allows us to define a method, but we can access it like an attribute. With __*@property*__ decorator (Object getter (@method_name.getter) and setter (@method_name.setter) properties), we convert a class method to a property. So when we use this property, we implicitly call the related method (and show the return value).  

* __*@classmethod*__: Class methods, marked with the @classmethod decorator, don’t need a class instance. They can’t access the instance (self) but they have access to the class itself via cls. Instead of accepting a self parameter (instance of the class), class methods take a cls parameter (class itself) that points to the class, and not the object/instance of the class, when the method is called. This is useful when we want to work with class attributes/variables. It's common to run class methods from class itself, not from instances. Since the class method only has access to this cls argument, it can’t modify object instance state. That would require access to self. However, class methods can still modify class state that applies across all instances of the class. 

We can use class methods as factory functions (to create new objects from the class using cls argument as constructor). Use of class methods can also allow you __*to define alternative constructors for your classes*__. Python only allows one __ init __ method per class. Using class methods it’s possible to add as many alternative constructors as necessary. A popular convention within the Python community is to use the from preposition to name constructors that you create as class methods.
```python
class Circle:
    def __init__(self, radius):
        self.radius = radius
        
    @classmethod
    def from_diameter(cls, diameter):  # its first argument receives a reference to the containing class, Circle.
        return cls(radius=diameter/2)  # instantiate Circle by calling cls (cls = Circle, actually) with the radius 
                                         that results from the diameter argument.
                                         
circle = Circle.from_diameter(84)   # construct an instance using the diameter instead of the radius
```

With this technique, you have the option to select the right name for each alternative constructor that you provide, which can make your code more readable and maintainable.

* __*@staticmethod*__: Static methods, marked with the @staticmethod decorator, don’t have access to cls or self. They work like regular functions but belong to the class’s namespace, because they have some logical connection with the class. We use them when we need some functionality not w.r.t. an object but w.r.t. the complete class. This means, a static method can be called without creating an object for that class. Static methods don't take self as an argument while all other class methods take self as an argument. That's why, static method is simply basically a normal function but it is attached to a class definition. They are specifically good to implement factory functions, which generate objects from the class. This type of method takes neither a self nor a cls parameter (but of course it’s free to accept an arbitrary number of other parameters). Therefore a static method can neither modify object state nor class state. Static methods are restricted in what data they can access - and they’re primarily a way to namespace your methods. Particular method is independent from everything else around it. If you don't access the instance or the class anywhere within the method, then it would be appropriate to make it static method.   

* __*@abstractmethod*__: A method whose declaration is known but body cannot be provided is called abstract method, and the class which contains at least one such abstract method is called abstract class (in C++, virtual functions does this). 
```python
from abc import ABC, abstractmethod
class Shape(ABC):
    def __init__(self, name):
        self._name = name
        
    @abstractmethod
    def draw(self):
        pass
```

* def __ len __ (self): with this function definition, we can use len(class_instance).

Objects that have a len() will be falsy when the result of len() is 0. It doesn’t matter if they’re lists, tuples, sets, strings, or byte strings. 

* def __ getitem __ (self, key): to support indexing for our object. 

* def __ iter __ (self)_ to make for loop work with our object properly. It returns an iterator that allows you to loop through the object. 

* def __ contains __ (self, member): to be able to use __*in*__ operator with the object correctly.  

* def __ file __ (self): returns the path relative to where the initial Python script was called. If you need the full system path, you can use os.getcwd() to get the current working directory of your executing code.

* def __ repr __ (self): A useful form to express "this is everything you need to know about this instance". Use repr() (or %r formatting character, equivalently) inside __ repr __ implementation. When you run <your_object>, you will get a desription that you defined in __ repr() __ method.

* def __ str __ (self): for "pretty print" functionality.  if __ repr __ is defined, and __ str __ is not, the object will behave as though __ str __ = __ repr __ . When writing your own classes, it’s a good idea to have a method that returns a string containing useful information about an instance of the class (like .description()). The pythonic way of to do is using __ str() __ method. When you run print(<your_object>), you will get a desription that you defined in __ str() __ method.

* def __ call ___ (self): It turns the instances of the class into callable objects. In other words, you can call the instances of the class like you call any regular function.

* __ dict __: A python object saves its attributes in the class dictionary (namespace of the instance). We can simply update the attributes using the class dictionary. This can be handy when we assign/make equal an object to another object in the same type.
```python
self.__dict__.update(other_obj.__dict__)  # make self equal to other_obj
```

```python
Class Numbers:
    def __init__(self):
        self._container = [1, 9, 2, 3]
    def __len__(self):
        return len(self._container)
    def __getitem__(self, key):
        return self._container[key]
    def __iter__(self):
        while self._container:
            yield self._container.pop()
        
numbers_obj = Numbers()
print(len(numbers_obj))
```

__*yield*__ is like a return statement but it doesn't end the function. It merely suspends the function, and next time the function will resume. That's called generator function. When the Python yield statement is hit, the program suspends function execution and returns the yielded value to the caller. (In contrast, return stops function execution completely.) When a function is suspended, the state of that function is saved. This includes any variable bindings local to the generator, the instruction pointer, the internal stack, and any exception handling. This allows you to resume function execution whenever you call one of the generator’s methods. In this way, all function evaluation picks back up right after yield.

### Decorators
Decorators wrap a function, modifying its behavior.
```python
def my_decorator(func):  # function that takes a function reference as argument
    def wrapper():
        print("before function")
        func()
        print("after function")
    return wrapper  # returns a function reference
    
def say_hi():
    print("Hi!")

say_hi = my_decorator(say_hi)  # decoration happens here, say_hi now points to the wrapper() inner function. However, wrapper() has a reference to the original say_hi() as func, and calls that function. So decorators wrap a function and modifies its behavior. If you call now say_hi(), its behaviour will be the modified one. 
```

Syntactic Sugar: The way we decorated say_hi() above is a little clunky. The decoration gets a bit hidden away below the definition of the function. Instead, Python allows you to use decorators in a simpler way with the @ symbol, sometimes called the “pie” syntax.
```python
def my_decorator(func):  # function that takes a function reference as argument
    def wrapper():
        print("before function")
        func()
        print("after function")
    return wrapper  # returns a function reference

@my_decorator
def say_hi():
    print("Hi!")
```
So, @my_decorator is just an easier way of saying say_hi = my_decorator(say_hi). It’s how you apply a decorator to a function.

### File Path Handling
With paths represented by strings, it is possible, but usually a bad idea, to use regular string methods. For instance, instead of joining two paths with + like regular strings, you should use os.path.join(), which joins paths using the correct path separator on the operating system. Subsequent folders are separated by a forward slash / (Unix - Mac and Linux) or backslash \ (Windows). 

The objects returned by pathlib.Path are either PosixPath or WindowsPath objects depending on the OS. pathlib.Path() objects have an .iterdir() method for creating an iterator of all files and folders in a directory. A little tip for dealing with Windows paths: on Windows, the path separator is a backslash, \. However, in many contexts, backslash is also used as an escape character in order to represent non-printable characters. To avoid problems, use __*raw string literals*__ to represent Windows paths. These are string literals that have an r prepended to them. In raw string literals the \ represents a literal backslash: r'C:\Users'.
```python
import pathlib
pathlib.Path(r'C:\Users\koray\python\file.txt')  # a path is explicitly created from its string representation
```
Relative path: home/koray/Desktop
Absolute Path: home/koray/Desktop (starts with a leading /)

* Import modules: When we import a module, Python checks multiple locations and location that it checks is within a list called sys.path. We can see this list:
```python
import sys
print(sys.path)
```
First element is the directory containing the script that we are running, so you can always import modules from the same directory. We can append directories to this list with sys.path.append(). The better way is adding the directory to Python path environment variable (PYTHONPATH). 

### Memory Management
The default Python implementation, CPython, is actually written in the C programming language. You need something to interpret written code based on the rules in the manual, and CPython interprets Python bytecode (output is .pyc file or a __pycache__ folder). You also need something to actually execute interpreted code on a computer. CPython (or other alternatives like IronPython or PyPy) fulfills both of those requirements. It converts your Python code into instructions that it then runs on a virtual machine. The memory management algorithms and structures exist in the CPython code, in C.

### Big O Complexity (Average required time complexity)
* O(1) means constant time. For example, appending an element to a list, getting an element with an index, search an element in a set
* O(n) means that the average time complexity will grow along with the size e.g. of a list (traversing the whole list,  all elements must be once considered). For example, inserting an element at the beginning of the list, searching for a specific element (index()), summing all elements(sum()), just for loop itself (for i in range(n)).
O(n^2) means that as the number of elements grows, the number of lookups grows quadratically (time complexity grows on the order of O(N²)). For example,  two nested for loops, if-elif-else (O(1)-O(n)-O(n^2)) block.

- collections.deque (pronounced “deck”) uses an implementation of a linked list in which you can access, insert, or remove elements from the beginning or end of a list with constant O(1) performance.

### Pythonic Tricks
- __*enumerate()*__ function returns a list of indices and values in a list. Use this function if you wanna walk through a list and at the same time keep track of the positions in a list. You can start your count at an offset using the optional "start" parameter:  
```python
for i, val in enumerate(myList, start=10):
    print(i, val)
```
By using the start parameter, we access all of the elements, starting with the first index, but now our count starts from the specified integer value.

- If you wanna walk through two lists at the same time, we can use the __*zip()*__ function. zip() function takes 2 or more lists and zips them together. We get the items which have the same index in their lists as pairs. We want to use it when two lists are related to each other.  
```python
for x, y in zip(x_list, y_list):
    print(x, y)
```
zip() returns a zip object, not the entire list of zipped items. It's good for efficiency, because it can be a huge list, so we save memory. If you wanna get all those values in a list, you can cast them to a list with list(). 

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
- We can use __*setdefault*__ function when you want to update the dictionary with a default value. It checks if the key exists in the dictionary, and if so it returns that value. Otherwise, it sets key to the given value and returns the new value.
```python
    myValue = myDict.setdefault('myKey', 'defaultValue') # if myKey is not in the myDict dictionary, add it, then set it to defaultValue and return defaultValue
```

We can use default dictionary from the collections package. It is pretty powerful object if we have a big dictionary. It extends standard dict functionality to allow you to set a default value that will be operated upon if the key doesn’t exist. Leveraging a defaultdict can lead to cleaner application code because you don’t have to worry about default values at the key level. Instead, you can handle them once at the defaultdict level and afterwards act as if the key is always present. Default value is indicated by a function:
```python
import collections
d = collections.defaultdict(lambda : 0)  # lambda returns always 0 as default value in this case
d.update(myDict) # copy all the contents from myDict into the default dictionary d
myValue = d[myKey]  # if myKey doesn't exist, returns 0

# lambda : 0 is the same as
def dummy():
    return 0
d = collections.defaultdict(dummy)

# list() returns empty list([]). If you want to initialize values as []:
d = collections.defaultdict(list)
```
- with continue, we break a single loop cycle, resuming immediately with the next cycle. You can accomplish the same with nested if statements, but continue is a way to avoid your code from becoming deeply intended. 
```python
for act, (nat, hit) some_dictionary.item():
    if act == x and nat == y and hit == z:
        # do some stuff
       
# instead of this, we can do this:
for act, (nat, hit) some_dictionary.items():  # items() method of a dictionary gives you a list of (key, value) tuples.  
    if act != x:
        continue # is act is not x, then ignore this cycle
    if nat != y:
        continue
    if hit != z:
        continue
    # do some stuff
```

- Python for loops have an else statement. Else statement is only executed if no break statement occured during the loop. It can be useful if you have some kind of code that you want to execute if everything went okay. 
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
We can use the __*with*__ statement (when we are working with files. The with statement initiates a context manager. The context manager opens the file and manages the file resource as long as the context is active. File is automatically closed after everything under the with statement is executed or an exception raised. The with statement automatically takes care of closing the file once it leaves the with block, even in cases of error. No need to bother cleaning up. 
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

" * " unpacks list, " ** " unpacks dictionary. 
```python
fourNumbers = [1, 2, 3, 4]
first, _, _, last = fourNumbers

hundredNumbers = [1, 2, ..., 100]
first, *rest, last = hundredNumbers # everything in between first and last element should be captured in a variable called rest, works also with tuples
```

- List comprehensions instead of map() and filter():
Python supports list comprehensions, which are often easier to read and support the same functionality as map() and filter().
```python
map() and its equivalent list comprehension
my_list = [-5, 4, -3, 2]
l1 = [abs(x) for x in my_list]
l2 = list(map(abs, my_list))
>>> l1 == l2  # returns True

# filter() and its equivalent list comprehension
l1f = [x for x in my_list if x>0]
l2f = list(filter(lambda x: x>0, my_list))
>>> l1f == l2f  # returns True
```

 - List comprehensions return full lists, while generator expressions return generators. Unlike lists, lazy iterators do not store their contents in memory, so generators are a great way to optimize memory. However, if speed is an issue and memory isn’t, then a list comprehension is likely a better tool for the job than a generator. Generator expressions are often slower than list comprehensions because of the overhead of function calls (next()). 
Generator expressions are perfect for when you know you want to retrieve data from a sequence, but you don’t need to access all of it at the same time. The design allows generators to be used on massive sequences of data, because only one element exists in memory at a time.

Generators/iterator objects can be exhausted, and that means that we can loop through and access their values one time but we can't do it again. It all comes down to performance and efficiency. For example, when you cast a generator to a list with list(), you iterate over the generator and make it exhausted. Then you cannot loop through that generator again, because it's exhausted anymore. To be able to use it again, assign it the result of list casting to a variable and use that variable then. 

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

Functions and methods are always truthy. You might encounter this if a parenthesis is missing when you call a function or method. 
```python
    def always_false():
        return None
    if always_false():  # Check Function return
        print("Never enters here")   
    if always_false:  # Check Function object
        print("It enters here")  
```  

- eval() function: You can pass a function and its arguments as string, then eval() will parse this string(expressin) and evaluate it as a Python expression. 
```python
def testf(arg1):
    pass
   
function = "testf"
x = eval(function + "(5)")  # everything has to be in string format
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

- Use of assert 
when the result is False, assert returns AssertionError. If you connect the assert with comma ( , ) and string, then the string gets displayed after the error:
```python
assert <Expression to evaluate>, "assert message here to display when expression is False"
# We can do simple argument check for functions with assert 
def test_func(mode="test")
    assert mode in ["test", "val"], "mode only takes either train or test"
```

### "is" vs "==" operator
A Singleton pattern in python is a design pattern that allows you to create just one instance of a class, throughout the lifetime of a program. True and False are singleton objects in Python, so, they are exist as only one object in the project (Some objects that are interned by default are None, True, False, commonly-used values (for example, the integers -5 to 256). It’s usually better to explicitly check for identity with the Python identity operator "is":
```python
if x is None:  #  good practice to use the Python is operator for comparing with None by memory address than it is by using class methods (it doesn’t                       depend on the logic of any __eq__() class methods which can be overwritten). 
    ...  #  ellipsis
```
As a rule of thumb, you should always use the equality operators == and !=, except when you’re comparing to None. The Python equality operator "==" checks if two objects have the same value / compares the value or equality of two objects. The identity operator "is" checks if two variables are the same/identical (point to the same object in memory/same memory address). At a higher level, comparing objects using "is" can be likened to comparing the location of the objects in the heap. For example, when you intern two strings with sys.intern(), you ensure that they point to the same object in memory. When you compare e.g. strings which are interned with "is", it will compare their memory addresses rather than comparing the strings character-by-character). You can use sys.intern() to optimize memory usage and comparison times for strings, although the chances are that Python already automatically handles this for you behind-the-scenes.
 ```python   
    from sys import intern
    a = intern("BMW M4 is such a nice car")
    b = intern("BMW M4 is such a nice car")
    >>> a is b  # True
```
    
In the vast majority of cases, this means you should use the equality operators == and !=, except when you’re comparing to None. Keep in mind that most of the time, different objects with the same value will be stored at separate memory addresses. This means you should not use the Python "is" operator to compare values. Everything is an object in Python, so be careful when using is to compare.
    
* Use the equality operators == and != if you want to check whether or not two objects have the same value, regardless of where they’re stored in memory.
* Use the Python is and is not operators only when you want to check whether two variables point to the same memory address. 
```python
list1 = []
list2 = []
>>> list1 == list2  # True
>>> list1 is list2  # False, they are different objects
```
    
- Shallow copy and Deepcopy:
When you use the assignment operator (=) to make one variable equal to the other, you make these variables point to the same object in memory. When we assign an object to another object, we don't create an object, but a reference to that object (like using & operator in C++). This is a typical failure when we manipulate similar objects. To be able to create a copy, we should explicitly indicate it.  
```python
x = [1, 9, 2, 3]
y = x.copy  # Both variables will have the same value, but each will be stored at a different memory address. Only y = x would make y a reference to x.
```

- Don't use mutable default values as function parameters, initialize them rather as None. Python’s default arguments are evaluated once when the function is defined/created, not each time the function is called.  This means that if you use a mutable default argument and mutate it (e. g. a list), you will and have mutated that object for all future calls to the function as well. Create a new list each time the function is called, by using a default arg to signal that no argument was provided (None is a good choice):
```python
def test(x = None):  # don't do x = [] here, otherwise you will have one list in the memory and each time you call the function, you'll always use this                          same memory address and manipulate the same object.
x = x or []  # if x is None, then create a new empty list, so that the outputs of each function call will be independent from each other 
```
Sometimes None can be useful in combination with short-circuit evaluation in order to have a default. In the case of "and" and "or", in addition to short-circuit evaluation, they also return the value at which they stopped evaluating. This can come handy when, for example, you want to give values defaults.:
```python
    def test(list_in=None)
        return len(list_in or [])  # an empty list won’t be created if list_in is a non-empty list, since "or" will short-circuit before it evaluates []
```
This example takes advantage of the falsiness of None and the fact that or not only short-circuits but also returns the last value to be evaluated.
    
### Type/Function Annotations
When a function parameter can be one of several types, we can use Union (also called a sum type). For example, Union[int, str] means it could be an int; or it could be a str; but it cannot be a float, or a list, or another dict, or anything other than an int or str.

When a function returns multiple values, we can use Tuple. For example, -> Tuple[bool, str] means function returns a bool and a string (as tuple, actually, you are always returning one object). 

When a parameter is optional (a Python optional argument is an argument with a default value), you can use "Optional" as type annotation. For example, Optional[int]
```python
from typing import List, Tuple, Union, Optional
def testf(x: int, y: Optional[bool], z: Union[int, str], w: dict) -> Tuple[bool, int]
    pass
```
