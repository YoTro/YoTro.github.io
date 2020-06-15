---
title: How to Learn Python Fastly By Game
date: 2017-07-29 12:27:31
tags: [python,game]
---
#  The pythonchallenge game #

If you want to practice your python,the [pythonchallenge](http://www.pythonchallenge.com/) coulde help you.There are currently 33 levels.

python -v 2.7.13 

---

## The 0 level ##

![loading...](/images/pythonchallenge/0.jpg "0")  

[http://www.pythonchallenge.com/pc/def/0.html](http://www.pythonchallenge.com/pc/def/0.html)  

You must be count the num to change the URL

### Answer ###

![loading...](/images/pythonchallenge/2^38.png "2^^38")

---

## The 1 level ##

![map](/images/pythonchallenge/map.jpg)  
```
g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj.

```

The prompt is    
>K-->M
>o-->Q
>E-->G

According to the picture we know,we coulde encoded this text first.  

### Answer ###

![loading...](/images/pythonchallenge/ord.png "encoding")  
```
I  h o p e   y o u   d i d n t   t r a n s l a t e   i t   b y   h a n d .    
 t h a t s   w h a t   c o m p u t e r s   a r e   f o r .     
 d o i n g   i t   i n   b y   h a n d   i s   i n e f f i c i e n t   a n d   t h a t ' s   w h y   t h i s   t e x t   i s   s o   l o n g .     
 u s i n g   s t r i n g .   
 m a k e t r a n s ( )   i s   r e c o m m e n d e d .     
 n o w   a p p l y   o n   t h e   u r l .  
``` 
LOL,if you translate it by hand,so inefficiency!  
Now,it is so easy.Only change `map` to `ocr`  

[http://www.pythonchallenge.com/pc/def/map.html](http://www.pythonchallenge.com/pc/def/map.html)



[RegExr](http://regexr.com/) can use `\w/g` get letters and tell you where they are.

### [source code](https://github.com/YoTro/Python_repository/blob/master/Pygame/1.py)

---

## The 2 level ##

![loading...](/images/pythonchallenge/2.jpg "The 2 level")  
```
recognize the characters. maybe they are in the book, 
but MAYBE they are in the page source.

```
We can open the web source  
![loading...](/images/pythonchallenge/string.png "source") 

### Answer ###

![loading...](/images/pythonchallenge/equality_code.png "equality_code")  

![loading...](/images/pythonchallenge/equality.png "equality")

We can know the password is `equality`

[http://www.pythonchallenge.com/pc/def/ocr.html](http://www.pythonchallenge.com/pc/def/ocr.html)

### [source code](https://github.com/YoTro/Python_repository/blob/master/Pygame/2.py)
```python 
str="g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."
key=[chr(i) for i in range(97,123)]#字典键值为a-z
value=[chr(i) for i in range(99,123)]
for i in range(97,99):
	value.append(chr(i))
#打包到一个字典里
z=dict(zip(key,value))
#添加字典
z[' ']=' '
z['(']='('
z[')']=')'
z['.']='.'
z['\'']='\''
t=False

u=list(str)
k=len(u)+1
for i in range(k):
        
        print z[u[i]],
        if z[u[i]]=='.':
                print '\n'
        if i==k:
                t=True#循环到最后一个字符退出循环
```

---

## The 3 level ##

![loading...](/images/pythonchallenge/3.jpg)  
```
One small letter, surrounded by EXACTLY three big bodyguards on each of its sides.  
```
This level tell us how to use `re` which like `xXXXxXXXx`.  
![loading...](/images/pythonchallenge/re_3.png)  

### Answer ###

![loading...](/images/pythonchallenge/3_code.png "re")  

![loading...](/images/pythonchallenge/linkedlist.png "linkedlist")  

[http://www.pythonchallenge.com/pc/def/equality.html](http://www.pythonchallenge.com/pc/def/equality.html)


But the web change from `html` to `php`

### [source code](https://github.com/YoTro/Python_repository/blob/master/Pygame/3.py)

---

## The 4 level ##

![loading...](/images/pythonchallenge/4.jpg)

Check in the picture you will see the url `http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=12345` and the body is `and the next nothing is 44827`

I found this is a hundreds loop,so I check in the web source.The prompt is   
```
<!-- urllib may help. DON'T TRY ALL NOTHINGS, since it will never 
end. 400 times is more than enough. -->
```
### Answer ###

![loading...](/images/pythonchallenge/4_urllib2.png "urllib2")

If you don't know what is [re](https://www.runoob.com/python/python-reg-expressions.html) you can check here.
 
![loading...](/images/pythonchallenge/peak.png "peak")

```
http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=(\d+)
```
### [source code](https://github.com/YoTro/Python_repository/blob/master/Pygame/4.py)
```python
import urllib2,re 
r=re.compile(r'.*?(\d+)$')  
nextnothing='12345'  
i=1
t=True
while t:
                try:

                        f=urllib2.urlopen('http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=%s'% nextnothing)
                        result=f.read()
                        f.close()
                        print i,result
                        nextnothing=r.search(result).group(1)
                        if  nextnothing=='16044':
                                nextnothing='8022'
                        if result=='peak.html':
                                t=False
                        i+=1
                except:
                        if result=='peak.html':
                                t=False
                        print 'error'
                      

```
---
## The 5 level ##

![loading...](/images/pythonchallenge/5.jpg)  
`pronounce it `  

The best way is to see the source code 

```
<html>  
<head>  
  <title>peak hell</title>  
  <link rel="stylesheet" type="text/css" href="../style.css">  
</head>  
<body>  
<center>  
<img src="peakhell.jpg"/>  
<br><font color="#c0c0ff">  
pronounce it  
<br>  
<peakhell src="banner.p"/>  
</body>  
</html>  

<!-- peak hell sounds familiar ? -->  
```
You can notice that [`banner.p`](http://www.pythonchallenge.com/pc/def/banner.p)

It tell us that `Persistent serialized storage` of the python about `pickle`-Create portable serialized representations of Python objects.

The Tools is use like game save in the hard disk can serialize any type of data such as dict list into a string format.

Also has a module `cPickle`  which for a (much) faster implementation becaus it is written by C,but it named pickle in Python3.

pickle functions

>FUNCTIONS
>    dump(obj, file, protocol=None)
    
>    dumps(obj, protocol=None)
    
>    load(file)
    
>    loads(str)

### Answer ###

![loading...](/images/pythonchallenge/5_code.png)

![loading...](/images/pythonchallenge/channel.png)

`channel`

```
[http://www.pythonchallenge.com/pc/def/peak.html](http://www.pythonchallenge.com/pc/def/peak.html)

```
### [source code](https://github.com/YoTro/Python_repository/blob/master/Pygame/5.py)

```python
import urllib2,pprint
import cPickle as pickle
b=urllib2.urlopen('http://www.pythonchallenge.com/pc/def/banner.p')#get该地址文件，b类型为实例（isinstant）
result=pickle.Unpickler(b).load()#创造一个unpickler文件反序列化成list
pprint.pprint(result)#打印result
output=open('c:\\Users\\Administrator\\Desktop\\5text.txt','w')
for line in result:
    print >> output, ' '.join([c[0]*c[1] for c in line])#元组（tuple)元素相乘转成字符串写入5text文件
output.close()#关闭文件
```
---

## The 6 level ##

![loading...](/images/pythonchallenge/6.jpg)

```html
<!-- <-- zip -->

·····

```
change the `http://www.pythonchallenge.com/pc/def/channel.html` to `http://www.pythonchallenge.com/pc/def/channel.zip`, then download a zip file.

A readme.txt tell us 
```
welcome to my zipped list.

hint1: start from 90052
hint2: answer is inside the zip
```
Every files in this zip has a string: "Next nothing is (a int number)", so we need to travel these files match the number then find a final number

### Answer

```python
import re
c = 0
n = ['90052']#if chose other numbers, iteration will decrease
while n[0].isdigit():
    c += 1
    with open("/Users/jin/Downloads/channel/{}.txt".format(n[0]),"r+") as f:
              r = f.readlines()
              n = re.findall(r'(\d+)',r[0])
    print(c, n[0])#c = 908
        

```
it returns `46145` finally which content is `Collect the comments.`

So we need to change our thought. 

According to [zipfile](https://docs.python.org/2.7/library/zipfile.html) module, we know the zipfile has comment, so we should to collect the comments with zipfile module

```
 **************************************************************
****************************************************************
****************************************************************
**                                                            **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE NN      NN  **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE  NN    NN   **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE       NN  NN    **
**   OOOOOOOO XX    XX YY        GGG       EEEEE     NNNN     **
**   OOOOOOOO XX    XX YY        GGG       EEEEE      NN      **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE         NN      **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE     NN      **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE     NN      **
**                                                            **
****************************************************************
 **************************************************************
```
### [source code](https://github.com/YoTro/Python_repository/blob/master/Pygame/6.py)

```python
import re
import zipfile
'''
c = 0
n = ['67824']
while n[0].isdigit():
    c += 1
    with open("/Users/jin/Downloads/channel/{}.txt".format(n[0]),"r+") as f:
              r = f.readlines()
              n = re.findall(r'(\d+)',r[0])
    print(c, n[0])

 **************************************************************
****************************************************************
****************************************************************
**                                                            **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE NN      NN  **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE  NN    NN   **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE       NN  NN    **
**   OOOOOOOO XX    XX YY        GGG       EEEEE     NNNN     **
**   OOOOOOOO XX    XX YY        GGG       EEEEE      NN      **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE         NN      **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE     NN      **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE     NN      **
**                                                            **
****************************************************************
 **************************************************************
'''     
s = '/Users/jin/Downloads/channel.zip'
z = zipfile.ZipFile(s,"r")
n = '90052.txt'
convert = '90052'
comments = []
while convert.isdigit():
    #read the information in the archive
    info = z.read(n)
    #get comment from the txt
    comment0 = z.getinfo(n).comment
    #push to comments
    comments.append(comment0)
    #It need to translate byte to str to match numbers
    info=str(info).encode("utf-8")
    res = re.findall('\d', info)
    convert = ''.join(res)
    n = convert + '.txt'
    end = ''
    for c in comments:
        end += str(c).encode("utf-8")
    print(end)
```
---

## The 7 level ##

```
it's in the air. look at the letters.
```
According to NASA, the gases in Earth's atmosphere include:
Nitrogen — 78 percent.
Oxygen — 21 percent.
Argon — 0.93 percent.

And oxygen is the second largest component of the atmosphere, comprising 20.8% by volume.

### Answer

oxygen

