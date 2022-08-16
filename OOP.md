---
title: OOP
updated: 2022-08-16 17:38:25Z
created: 2022-08-15 14:41:59Z
---

## Defining Attributes in Classes
```
class MyClass:
	language = 'Python'
	version = '3.6'
```
- MyClass is a class -> it is an *object* (of type `type`)
- Python automatically creates attributes for us e.g. `__name__` with a state of `MyClass`
- It also has *language* and *version* attribtutes.

## Retrieve Attribute Values from Objects
```
getattr function
# getaatr(object_symbol, attribute_name, optional_default)
```
for e.g
```
getattr(MyClass, 'language') -> Python
getattr(MyClass, 'x') -> AttributeError exception
getattr(MyClass, 'x', 'N/A') -> 'N/A' (handling exception if the atrribute doesn't exist)
```
- **dot notation**
```
MyClass.language -> Python
MyClass.x -> AttributeError (no method to handle the default value handling)
```

## Setting Attribute Values in Objects
```
setattr function 
# setattr(object_symbol, attribute_name, attribute_value)
```
```
setattr(MyClass,'version','3.7')
# modifies the state of MyClass -> MyClass was mutate
```
`getattr(MyClass, 'version') -> 3.7`  
- What happend if we call `setattr` for an attribute we didn't define in our class?
	- Due to dynamic nature of python, attribute will be *added*
`setattr(MyClass,'x',100)`
This will create a new attribute *x* with a state *100*

## Where is the state stored
- State of a class is stored in a dictionary
```MyClass.__dict__```
- Returns a `readonly` dictionary (`<class 'mappingproxy'>`) and not mutatble and the whole return is called `namespace`.
- ensure *keys* are strings(helps in speeding things up for Python)
- Use `setattr` to modify the state of the class.

## Delete attributes
- This will remove the attribute from the namespace also
```
delattr(obj_symbol, attribute_name)
# delattr('MyClass', 'version') 
# or 
# del MyClass.version
```

## Accessing the Namespace Directly
- Class namespace uses a dictionary, which we can request using `__dict__` attribute of the class.
- The `__dict__` attribute returns a `mappingproxy` object which is not a dict, but still a `hashmap`, so we can atleast read access the class namespace directly.

## Setting Attribute value to a Callable
- Attribute values can be any object 
	- other classes
	- any callable

```
class MyClass:
	language = 'Python'
	#callable
	def say_hello():
		print('Hello World')
```
- `say_hello` is also an attribute of the class and its *value* happend to be a `callable`
- How to call it?
	- get it from the `namespace` dictionary
	- `getattr(MyClass,'say_hello')() -> 'Hello World'`

## Classes are Callable
- When a class is created using the `class` keyword, python automatically adds behaviours to the class.
- It adds something to make the class `callable`
- The return value of that callable is an `object`
	- The `type` of that object is the `class object` i.e. instantiation of the class
```
my_obj = MyClass()
type(my_obj) -> MyClass
isinstance(my_obj, MyClass) -> True
```
## Instantiating Classes
- When we call a class, a class `instance` is created
- The class instance object has `its own namespace`
	- `distinct` from the `namespace` of the class that was used to create the object.
- The object has some automatically implemented attribute:
	- `__dict__` is the object's local namespace
	- `__class__` tells us which class was used to instantiate the object.
		- prefer using `type(obj)` instead of `obj.__class__` due to property safety since the latter can be tweaked by the developers.
```
class MyClass:
	__class__ = str
	
m = MyClass()
m.__class__ , type(m) -> (str, __main__.MyClass)
isinstance(m, MyClass) -> True
isinstance(m,str) -> True
# as can be seen the class is also recognizes as string. But **type** is the safe option
```

## Data Attributes , Classes and Instances

```
class MyClass:
	language = 'Python' # class attribute
	
my_obj = MyClass()
```
```
# namespace
MyClass.__dict__ -> {'language':'Python'}
my_obj.__dict__ -> {}
```

- `MyClass.language ->`  Python starts looking for language attribute in `MyClass` namespace and returns
`MyClass.langugage -> 'Python'`

- `my_obj.language ->` Python starts looking in `my_obj` namespace and If it doesn't find it, start looking in the `type(class)` of `my_obj i.e. MyClass`

- Setting up instance attribute i.e. using instance's namespace rather class's 
- Every object has its own **namespace**.
```
my_obj.language = 'java'
my_obj.__dict__ -> {'language':'java'} # instance attribute

my_obj.language -> 'java'
# Python alwasy looks for the value at instance's namespace then move upwards if the value is not found.

MyClass.language -> 'Python'

other_obj = MyClass()
other_obj.__dict__ = {}
other_obj.language -> 'Python'
# since other_obj doesn't have any value in its own namespace hence looks for the value in class namespace and returns the value
```
- Class attributes are common to all instances but instance attribute can be that instance specific.
```
type(MyClass.__dict__) -> mappingproxy

obj1 = MyClass()
type(obj1.__dict__) -> dict
# The object dict is mutable and so we can manipulate , add attributes at object level(**override**)
i.e. only specific to instance
obj1.__dict__ -> {}
obj1.version = '3' # setting the value
obj1.__dict__ -> '3'
```

## Function Attributes, Class and Instance

```
class MyClass:
	def say_hello():
	''' 
		attribute of class but a function type 
		'''
		print('Hello World')
	
my_obj = MyClass() -> <function MyClass.say_hello at 0x7fa85718e3a0>
my_obj.say_hello -> <bound method MyClass.say_hello of <__main__.MyClass object at 0x7fa8572e34c0>>
```
```
MyClass.say_hello() -> "Hello World"
my_obj.say_hello() -> TypeError; say_hello() takes 0 positional arguments but 1 was given
```
- What is bound method?
	- Method is an actual `object` type in Python.
	- Like function, it is callable but unlike a function, it is `bound` to some object, and that object is passed to the method as its `first parameter.`
	```
	my_obj.say_hello()
		-> say_hello is a method object
		-> It is bound to my_obj
		-> When my_obj.say_hello is called, the bound object my_obj is injected as the first parameter to the method say_hello.
		-> So, it is similar to calling
		MyClass.say_hello(my_obj)
		-> say_hello now has a handle to the object's namespace.
	```
- Methods are objects that combine `instance of some class` and `function`
- like any object it has attributes
- `__self__` the instance the method is bound to
- `__func__` the original function defined in the class
```
calling obj.method(args) -> method.__func__(method.__self__, args)
```
```
class Person:
	def hello(self):
		pass
		
p = Person() -> p.hello.__self__ -> Refers to same object i.e. p

p.hello.__func__ -> refers to function hello
```

## Instance Methods
- We have to account for that **extra argument** when we define functions in our classes - otherwise we cannot use them as methods *bound to our instances.*
```
# When we define function in the class, we have the first argument as object to use it as instance method
class Person:
	def hello(obj): <- Receives instance
		print('hello world')
		
my_obj = MyClass()

my_obj.say_hello 
# When this call happens, the function becomes the method and is bound to my_obj, an instance of MyClass
```
- Function in classes can have their own **parameters**
- When we call the corresponding instance method with arguments are passed to the method as well and the method still receive the instance object reference as the first argument.
```
class Person:
	language = Python
	def hello(obj, name): <- Receives instance
		print('hello world {name}')
		
python = MyClass()
python.say_hello('John') -> calling the bound method -> MyClass.say_hello(python, 'John')
```