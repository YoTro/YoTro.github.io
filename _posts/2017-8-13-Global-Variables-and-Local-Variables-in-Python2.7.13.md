---
title: The UnboundlocalError
date: 2017-08-13 10:03:31
---

Python version is 2.7.13  
System is window7

## Global Variables and Local  Variables ##

Now We often meet the Error which is UnboundlocalError.It's a frequent question in the learning python.So I try to research the issue today to kown how the variables work in the function and class.


The function defaults that the variable is a global variable

```
i=8  
def t():  
		print i  

t()  
8
```
