---
title: MySQL Learning
date: 2019-06-30 18:00:00
tags: [LearningPlan,MySQL]
music-id: 1336990977
youtube-video-ID: tw1OtUWSXkM

---

## MySQL Learning

[EN](#EN)|[CN](#CN)

<span id="EN">After learning AliExpress, there are some batch processing work on daily, such as price, you can refer to the following flow chart
![loading...](/images/SMT/price.png)

So I decided to put the table into the sql database so that the data will be processed faster, then processed by Python.
**MySQL** which I intend to use is the database management system ("My" is the name - Monty Widenius who is one of Co-founder's daughter's name).

This is also very advantageous if the website I am doing in the future is in the small and medium size.  
You could download by command line or installer

<details>
<table markdown="span">
    <tr>
      <td></td>
      <td>Installer</td>
      <td>Command Line</td>
    </tr>
    <tr>
      <td>Linux or Mac</td>
      <td>[MySQL Workbench download](https://dev.mysql.com/downloads/file/?id=485470)
But when you update the homebrew, it usually takes your long time in China.   
So I recommend you download the [mysql-community-dmg](https://dev.mysql.com/downloads/file/?id=486026) in the official website</td>
     <td>~~~sh
.yum install mysql-community-server
.apt-get install mysql-server
.brew install mysql
~~~</td>
    </tr>
    <tr>
      <td>Windows</td>
      <td>[MySQL Community Server Download](https://dev.mysql.com/downloads/file/?id=485812)
      [MySQL Workbench Download](https://dev.mysql.com/downloads/file/?id=485462)</td>
      <td>Download the [wheel](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient) then install in your local computer
~~~sh
cd C:\Users\administered\Downloads\download  
pip install mysqlclient-1.4.2-cp27-cp27m-win_amd64.whl
~~~</td>
    </tr>
</table>
<details>

#### Python DB-API usage process:

. Call the API module.  
. Get a connection to the database.   
. Execute SQL statements and stored procedures.   
. Close the database connection.   


### Some Q&A :

1. **Q**: When importing a form to MySQL Workbench, the encoding format must be UTF8 or it will not recognize Chinese. If the header is not recognized by the English system, then it would be said the encoding of your excel is not utf-8.  
**A**: You should change the Chinese header to English ( beacause I have not found any better way).  
2. **Q**: The speed of table import wizard which means the speed of write to database is so slow(1.2MB/s),how to increase the speed?  
 **A**: Increase the size of cache,open the my.ini which is the MySQL configuration file.  
3. **Q**: If your excel's header has "("、")",the workbench can't distinguish the column of csv, then you will import nothing records
**A**: Cancel the "(",")"

<span id="CN">学习速卖通后，有一些批量处理工作，可以参考以下流程图
于是我决定把表格放入sql数据库中这样处理数据的速度会更快一些。
其中我打算运用的是数据库管理系统MySQL
这也是非常有优势的，如果以后我做的网站是处于中小型时。

### 遇到的一些问题：

1. **Q**: 导入表格到MySQL Workbench 时，编码格式必须是UTF8否则无法识别中文，表头如果英语系统无法识别标题，那么可以说excel的编码不是utf-8。  
**A**：您应该将中文标题更改为英语（因为我没有找到更好的方法）。  

2. **Q**：表导入向导的速度，这意味着写入数据库的速度是如此之慢（1.2MB / s），如何提高速度？  
**A**：增加缓存大小，打开MySQL配置文件my.ini。设置参数

3. **Q**: 导入表格时，会出现虽然可以导入但是导入了0条记录   
**A**: 这是因为你的表头含有小括号。（至于其它符号，有待测试）  


未完待写...