---
title: How to Solve The UnboundlocalError of Python
date: 2017-08-13 10:03:31
tags: [Variables,python27]
---

Python version is 2.7.13  
System is window7

## Global Variables and Local  Variables ##

Now We often meet the Error which is UnboundlocalError.It's a frequent question in the learning python.So I try to research the issue today to kown how the variables work in the function and class.


- The function defaults that the variable is a global variable

```
i=8  
def t():  
		print i  
t()  
8
```
The `'i`'s value is 8 which is comfire the result.

- If the Global variable is not named in the function,it would report error `UnboundLocalError: local variable 'i' referenced before assignment`

```
i=4
def t():
		i+=1
		print i
t()
``` 

- You could name the variable use in keyword `global`.

```
i=4
def t():
		global i
		i+=1
		print i
t()
5
i
5
```
The global variable `i` is change too.  

- If you don't want to change the global variable,you could assign it to another local variable.

```
i=4
def t():
		global i
		t=i
		t+=1
		print t
t()
5
i
4
```  

- You can also reference global variables on function arguments  

```
i=4
def t(i):
		print i
		i=2
		print t
t()
4
2
```
