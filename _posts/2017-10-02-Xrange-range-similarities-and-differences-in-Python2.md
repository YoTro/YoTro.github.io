---
title: xrange and  range in Python2
date: 2017-10-2 10:57:31
---

We all kown `Python2` have two Built-in function which are `range` and `xrange`,they are both  use in `for` looping.

## Similarities ##
1. They are both use in forloop 
2. They are both built-in function in Python2
3. They all accept 3 arguments [start,stop,step],start is default 0,step is default 1,and if `stop` is bigger than `start`,it will return a Empty list.

```
>>> x=range(0,10,1)  
>>> x  
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]  

``` 


## Differences ##

1. `range` will be built a list immediately if it is generated,but the `xrange` is gerenate a object like this
```
t=xrange(10)  
t  
xrange(10)  
type(t)  
<type 'xrange'>  

```
so `xrange` is use less memory and it has faster processing speed if the loop is especially large.And gerenates the numbers in the range on demand.

So it has not a slice becaus of it is a object not  list 
```
t[:]

Traceback (most recent call last):  
  File "<pyshell#11>", line 1, in <module>  
    t[:]
TypeError: sequence index must be integer, not 'slice'

```
The implementation method of `range` has been removed in `Python3`,but `xrange`'s is keeped,`Python3` is remove the `xrange` name.
So `range()` is more power in Python3.

## Official document ##

>Help on class xrange in module __builtin__:
>
>class xrange(object)
> |  xrange(stop) -> xrange object
> |  xrange(start, stop[, step]) -> xrange object
> |  
> |  Like range(), but instead of returning a list, returns an object that
> |  generates the numbers in the range on demand.  For looping, this is 
> |  slightly faster than range() and more memory efficient.

