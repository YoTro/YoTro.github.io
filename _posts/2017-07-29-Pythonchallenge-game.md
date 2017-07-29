---
title: The pythonchallenge game
date: 2017-07-29 12:27:31
---
If you want to practice your python,the [pythonchallenge](http://www.pythonchallenge.com/)coulde help you.There are currently 33 levels.

## The 0 level ##

![loading...](/images/pythonchallenge/0.jpg "0")  
```
http://www.pythonchallenge.com/pc/def/0.html  

```
You must be count the num to change the URL

![loading](/images/pythonchallenge/2^36.png "2^^38")

## The 1 level ##

![map](/images/pythonchallenge/map.jpg)  
```
g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj.

```

The prompt is    
General tips:  
* Use the hints. They are helpful, most of the times.  
* Avoid looking for spoilers.

According to the picture we know,we coulde encoded this text first.  
![loading...](/images/pythonchalleng/ord.png "encoding")  
![loading...](/images/pythonchalleng/translated.png "text")  
LOL,if you translate it by hand,so inefficiency!  
Now,it is so easy.Only change `map` to `ocr`  

```
http://www.pythonchallenge.com/pc/def/map.html

```

[RegExr](http://regexr.com/) can use `\w/g` get letters and tell you where they are.

## The 2 level ##

![loading...](/images/pythonchalleng/2.jpg "The 2 level")  
recognize the characters. maybe they are in the book, 
but MAYBE they are in the page source.

We can open the web source  
![loading...](/images/pythonchalleng/string.png "source")  
![loading...](/images/pythonchalleng/equality_code.png "equality_code")
![loading...](/images/pythonchalleng/equality.png "equality")  

We can know the password is `equality`

```
http://www.pythonchallenge.com/pc/def/ocr.html

```

## The 3 level ##

![loading...](/images/pythonchalleng/3.jpg)  
One small letter, surrounded by EXACTLY three big bodyguards on each of its sides.  

This level tell us how to use `re`.