---
title: Introducing the detail of self in Python
date: 2018-03-10 16:30:31
tags: [python,self]

---

# How to use self in python 

Before introducing Pyhton's self,we should to talk about `class` and `instance`.

We kown that the most concept of object-oriendted is the class and instance.The class is an abstract template.For examplem the People is a abstract object can be represented by a People class.An instance is a concrete "object" crected from classes.Each object inherits the same method,but the respective data may be different.

-----

1. You can see the Class `People`:  
```python
	class People(oject):
		pass
```
Object indicates which class the class inherits from,and the Object class is a class that all classes inherits.

2. Instance: How to create a People instance after define a People class.You can create by the class name + (),pass to a variable:  
```python
People=People()
```

3. Because the class is a template, we can bind the  attributes into the instance when we create.Here we us the `__init__()` which is a built-in function in Python:  
```python
 1 |Class People(object):
 2 | Def __init__(self, name, High):
 3 |     Self.name = name
 4 |     Self.High = High
```

**Note**:

- The first parameter of the `__init__ ()` function is always `self`,it meams that **self is the instance of created class**.So we can bind the attributes with self in `__init__()`.
	
- If you define the `__init__()`, We must pass in parameters that match the `__init__()` function, cannot pass empty parameters when we create instance.But self does not need to be passed,the Python interpreter will passed itself.  
    ```
	1|>>>People = People("Tom", 166)
	2|>>>People.name 
	3|"Tom"
	4|>>>People.High
	5|166
	```
In this way we can package this class very well. The external only needs to call the inside variable without knowing the internal implementation details inside.(**Everything is an object in Python**)  
To make the class more secure, we can call these private attributes  and public attributes.
Like This:  
| :----: || :----: |
|private attributes|"__name"|
|public attributes |"name"  |

```python
class People(object):
    def set_High(self, High):
        self.__High = High
```
```
>>> People = People('Tom', 166)
>>> People.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'People' object has no attribute '__name'

```
Cannot arbitrarily change internal important attributes, safe and robust.
