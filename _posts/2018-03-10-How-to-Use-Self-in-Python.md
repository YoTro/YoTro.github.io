---
title:Introducing the detail of self in Python
date: 2018-03-10 16:30:31
layout: post
tags: [python,self]
---
# How to use self in python 

Before introducing Pyhton's self,we should to talk about class and instance.

We kown that the most concept of object-oriendted is the class and instance.The class is an abstract template.For examplem the student is a abstract object can be represented by a student class.An instance is a concrete "object" crected from classes.Each object inherits the same method,but the respective data may be different.

-----

1. You can see the Class `Student`:
```python
	class Student(oject):
		pass
```
Object indicates which class the class inherits from,and the Object class is a class that all classes inherits.

2. Instance: How to create a Student instance after define a Student class.You can create by the class name + (),pass to a variable:
```python
student=Student()

```

3. Because the class is a template, we can bind the  attributes into the instance when we create.Here we us the `__init__()` which is a built-in function in Python:
```python
 1 |Class Student(object):
 2 | Def __init__(self, name, score):
 3 |     Self.name = name
 4 |     Self.score = score
```

*Note*:

- The first parameter of the `__init__ ()` function is always `self`,it meams that **self is the instance of created class itself**.So we can bind the attributes with self in `__init__()`.
	
- If you define the `__init__()`, We must pass in parameters that match the `__init__()` function, cannot pass empty parameters when we create instance.But self does not need to be passed,the Python interpreter will passed itself.
        ```python
	>>>student = Student("Hugh", 99)
	>>>student.name
	"Hugh"
	>>>student.score
	99
	```
