---
title: How to Send Mails in Linux and Django
date: 2020-02-04 00:35:00
tags: [Django,Linux,mailx]
music-id: 1418820831

---

[EN](##EN)|[CN](##CN)

<span id="EN">

# How to Send Mails by third party (QQ) Linux

We have 2 good tools `Sendmail` and `mailx` to send mails in Linux

## Install mailx

You could to use this command `rpm -qa |egrep 'mailx'` to confirm whether the system has installed the mailx

- **CentOS**

```bash
yum -y install mailx
```
-y is means all choices are yes when downloading and installing

- **Ubuntu & Debian**

```bash
apt-get install heirloom-mailx

```

heirloom-mailx and mailx are same application except the configure file, the heirloom-mailx is `/etc/nail.rc`, and mailx's file is `/etc/mail.rc`.

## Configure

We use the qq mail to send our mails, so you need to login qqmail through **SSL**. The normal ports are 465 and 587, the service is smtp.qq.com, we need to get the `smtp-auth-password` to be our password

### Why we need to use SSL to login and send emails

Because the default connection of mailx is the local MTA, such as Sendmail, Postfix, etc., SSL connection is not required. We need to connect to the external SMTP server, so using SSL / TLS protocol for communication is the best choice. But it requires us to configure the root certificate database ourselves (mailx does not configure the certificate database by default). Mailx obtains firstly the certificate chain of the external SMTP server and then checks whether the certificate chain is really issued by its claimed root certificate through NSS. The detection requires the root certificate in our configured certificate database.

## Get NSS CA of QQ mailbox server

We can see which company provides this service to Tencent

```bash
$ openssl s_client -connect smtp.163.com:465 -showcerts 2>&1 </dev/null | grep depth
```

Input>

```bash
depth=2 C = BE, O = GlobalSign nv-sa, OU = Root CA, CN = GlobalSign Root CA
depth=1 C = BE, O = GlobalSign nv-sa, CN = GlobalSign Organization Validation CA - SHA256 - G2
depth=0 C = CN, ST = guangdong, L = shenzhen, O = Tencent Technology (Shenzhen) Company Limited, CN = *.mail.qq.com
```
*Note*: When I use the port `587`, it can't work, I don't know why

When I browser this website, I found that GlobalSign nv-sa service of SSL is not free, so I still try this command to get the CA, it works!

```bash
mkdir -p /root/.certs/

echo -n | openssl s_client -connect smtp.qq.com:465 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ~/.certs/qq.crt

certutil -A -n "GeoTrust SSL CA" -t "C,," -d ~/.certs -i ~/.certs/qq.crt

certutil -A -n "GeoTrust Global CA" -t "C,," -d ~/.certs -i ~/.certs/qq.crt
certutil -A -n "GeoTrust SSL CA - G3" -t "Pu,Pu,Pu" -d ~/.certs/./ -i qq.crt

certutil -L -d /root/.certs
```

You will see 4 files in this dir `cert8.db`、`key3.db`、`secmod.db`、`qq.crt`

## Configure the mail.rc

`vim mail.rc` then write this code

```bash
#To configure the sender's name, please note that it should have the same name as the sending mailbox

#If you want to use a nickname, use parentheses, as shown below
set from=(MDY)xxx@qq.com

#server's url and port
set smtp=smtps://smtp.qq.com:465

#send from
set smtp-auth-user=xxx@qq.com

#you need to open a smtp service in qqmail and get this password
set smtp-auth-password=xxxxxxxxx

# Specifies How to do if an error occurs while verifying the validity of the server certificate

set ssl-verify=ignore

# Authentication method,other optional values are cram-md5 and plain

set smtp-auth=login

# mailx is uses NSS cryptography tool library, so certificate Library of NSS needs to be specified
set nss-config-dir=/root/.certs
```

Man mailx

### Interactive

```bash
$mail example@qq.com # enter your to_addr email first, then enter

Subject: This is a test email. #Then mailx will ask for your email subject. Enter it and press enter

This is a test

Bye!

. #input . in a new line, then enter to send mail 

EOT
```

### Pipe

Mailx can read content as mail content from standard input (stdin). In this way, it can be well combined with scripts or other commands:

```bash
$ echo "Hello, world!" | mail -s "hello world" example@qq.com
```

`-s` is to given subject

If send mails to multi-user, you could add addresses with `,` like this

```bash
$ echo "Hello, world!" | mail -s "hello world" example0@qq.com,example1@qq.com,
```

### Input redirection

```bash
$ mail -s "this is a test" example@qq.com < /path/to/file
```
### Add attachment

```bash
$ echo "Hello, world!" | mail -s "hello world" -a /path/to/file example@qq.com
```

# How to send emails by commands Django package

## Configure Settings.py

Add this code to settings.py which is in djangoproject
```python
EMAIL_USE_SSL = True
EMAIL_HOST = 'smtp.qq.com'  #if it is 163 change to smtp.163.com
EMAIL_PORT = 465
EMAIL_HOST_USER = 'xxx@qq.com' #from addr
EMAIL_HOST_PASSWORD = 'p@ssw0rd'  #password of auth-third-party 
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
```
```bash
python manager.py shell
```
call the python in Django

then input this code:

```python
from django.core.mail import send_mail
from_addr = "from@example.com"
to_addr = ["to@example.com"]
subject = "hello world, this is subject"
content = "here is the message send from {0}".format(from_addr)
send_mail(subject, content, from_addr, to_addr, fail_silently=False)
```
# How to send many emails by Django

```python
from django.core.mail import send_mas_mail
from_addr = "from@example.com"
to_addr = ["to@example.com","to1@example.com"]
to_addr1 = ["to2@example.com"]
subject0 = "hello world, this is subject"
content0 = "here is the message send from {0}".format(from_addr)
subject1 = "hello world, this is subject1"
content1 = "here is the other message send from {0}".format(from_addr)
message0 = (subject0, content0, from_addr,to_addr)
message1 = (subject1, content1, from_addr,to_addr1)
send_mas_mail((message0,message1),fail_silently = False)
```
Note: The main difference between [`send_mass_mail`](https://docs.djangoproject.com/en/3.0/topics/email/##send-mass-mail) and [`send_mail`](https://docs.djangoproject.com/en/3.0/topics/email/##send-mail) is that send_mail() opens a connection to the mail server each time it’s executed, while send_mass_mail() uses a single connection for all of its messages. This makes send_mass_mail() slightly more efficient.

######## How to add attachment to email

```python
from django.conf import settings
from django.core.mail import EmailMultiAlternatives


from_email = settings.DEFAULT_FROM_EMAIL
from_addr = "from@example.com"
to_addr = ["to@example.com","to1@example.com"]
subject = "hello world, this is subject"
content = "here is the message send from {0}".format(from_addr)
path_file = "/path/of/your/attachment.xxx"
msg = EmailMultiAlternatives(subject, content, from_email, to_addr)
#tell the server the file's format
msg.content_subtype = "html"

# add attachment
msg.attach_file(path_file)

msg.send()
```
# How to build email function to share our blog in website

Firstly, let's see the directory structure of the djangoproject

```
djangoproject/
	sqlite3.py
    manage.py
    djangoproject/
        __init__.py
        settings.py
        urls.py
        wsgi.py
	blog/
		templates/
			pagination.html
		    blog/
		        base.html
		        post/
		        	share.html
		            list.html
		            detail.html
	    __init__.py
	    admin.py
	    migrations/
	        __init__.py
	    models.py
	    tests.py
	    views.py

```

Then we need to do 4 steps:

- Build a form to write user's name,email,to_address_of_mail,comments

- Build view in the `views.py` to send emails

- add urlpattern to blog's `urls.py` 

- create template to show this form

## Configure Settings.py

Add this code to settings.py which is in djangoproject
```python
EMAIL_USE_SSL = True
EMAIL_HOST = 'smtp.qq.com'  ## if it is 163 change to smtp.163.com
EMAIL_PORT = 465
EMAIL_HOST_USER = 'xxx@qq.com' ## from addr
EMAIL_HOST_PASSWORD = 'p@ssw0rd'  ##password of auth-third-party 
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
```

## Create sendMailForm

First, make forms.py in blog:

```python
from django import forms

class EmailPostForm(forms.Form):
    name = forms.CharField(max_length=25)
    email = forms.EmailField()
    to = forms.EmailField()
    comments = forms.CharField(required=False, widget = forms.Textarea)
```
The Class `forms` is used to handle the input data, and data validation
[`CharField()`](https://docs.djangoproject.com/en/3.0/ref/forms/fields/##charfield) usually tell web, it needs user input text, templates will rennder like this `<input type="text">` due to the default widget is **TextInput**. We use `<textarea></textarea>` in Comments due to the `widget=forms.Textarea` 
More fields of Django, please check here [https://docs.djangoproject.com/en/3.0/ref/forms/fields/](https://docs.djangoproject.com/en/3.0/ref/forms/fields/)

## Create View in Views.py

```python
from .forms import EmailPostForm
from django.core.mail import send_mail

def post_share(request, post_id):
    # Retrieve post by id
    post = get_object_or_404(Post, id=post_id, status='published')
    sent = False
    cd = None
    from_addr = 'admin@myblog.com'
    if request.method == 'POST':
        # Form was submitted
        form = EmailPostForm(request.POST)
        if form.is_valid():
            # Form fields passed validation
            cd = form.cleaned_data
            post_url = request.build_absolute_uri(
                                    post.get_absolute_url())
            subject = '{} ({}) recommends you reading "{}"'.format(cd['name'], cd['email'], post.title)
            message = 'Read "{}" at {}\n\n{}\'s comments: {}'.format(post.title, post_url, cd['name'], cd['comments'])
            send_mail(subject, message, from_addr,[cd['to']])
            sent = True
    else:
        form = EmailPostForm()
        
    return render(request, 'blog/post/share.html', {'post': post,
                                                    'form': form,
                                                    'sent': sent,
                                                    'cd':cd})

```

### [`get_object_or_404()`](https://docs.djangoproject.com/en/3.0/topics/http/shortcuts/##get-object-or-404)

Calls get() on a given model manager, but it raises Http404 instead of the model’s DoesNotExist exception.

You could understand the function by these 2 codes:

```python
from django.shortcuts import get_object_or_404

def my_view(request):
    obj = get_object_or_404(MyModel, pk=1)
```
 this code is equivalent to:  
```python
from django.http import Http404

def my_view(request):
    try:
        obj = MyModel.objects.get(pk=1)
    except MyModel.DoesNotExist:
        raise Http404("No MyModel matches the given query.")
```

## Add urlpatterns to urls.py of blog

```python
urlpatterns = [
# ...
url(r'^(?P<post_id>\d+)/share/$', views.post_share,
    name='post_share'),
]
```

## Render forms in templates

######## Add share.html 

add to this path `blog/templates/blog/post/`

```
{ % extends "blog/base.html" % }

{ % block title %}Share a post{% endblock % }

{ % block content % } 
  { % if sent % }
    <h1>E-mail successfully sent</h1>
    <p>
      "{{ post.title }}" was successfully sent to {{ cd.to }}.
    </p>
  { % else % }
    <h1>Share "{{ post.title }}" by e-mail</h1>
    <form action="." method="post">
      {{ form.as_p }}
      { % csrf_token % }
      <input type="submit" value="Send e-mail">
    </form>
  { % endif % }
{ % endblock % }
```

This share.html is a file when you click the share button then show the forms to user

The `{% csrf_token%}` template tag introduces an automatically generated token that can avoid cross Site Request Forgery (CSRF) attacks, which is a hidden field. These attacks consist of malicious sites or programs that can perform malicious actions for users on your site. You can find out more about it by visiting [https://en.wikipedia.org/wiki/Cross-site_request_forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery) request'forge.

it will render like this:

```html
<input type='hidden' name='csrfmiddlewaretoken' value='26JjKo2lcEtYkGoV9z4XmJIEHLXN5LDR' />
```

[`{{ form.as_p }}`](https://docs.djangoproject.com/en/3.0/ref/forms/api/##as-p)

renders the form as a series of <p> tags, with each <p> containing one field. You could use `as_table` or `as_ul`.  

######## Add sharelink to detail.html

add this code to blog/post/detail.html :

```html
<p>
  <a href="{% url "blog:post_share" post.id %}">
    Share this post
  </a>
</p>
```

now run `python manage.py runserver 0.0.0.0:80` to run your website

<span id="CN">

# 如何通过mailx发送邮件

mailx 是一个邮件用户代理(MUA)工具，通俗的说就是邮件客户端。可以通过它来操作邮件传输代理(MTA)，比如 Postfix、Sendmail、QQ 邮箱、163 邮箱等，进行收发邮件。不同于我们常见的 Outlook 或者 Spark 这种具有图形界面的邮件客户端，mailx 在 Linux 上以命令方式运行，可以很方便的与脚本或其它命令结合，将脚本或命令输出的信息通过邮件发送出去。与其类似的还有 mutt。

## 安装

注意：如果系统中预置了 mailx，由于依赖可能有所不同并因此会导致无法使用，所以需要先将系统预置 mailx卸载。然后再安装：

**CentOS**
```bash
$ sudo yum -y install mailx
```
**Ubuntu & Debian**
```bash
$ sudo apt-get install heirloom-mailx
```
heirloom-mailx 和 CentOS 7下的 mailx，除了配置文件不同外，实际是同一个程序。heirloom-mailx 的配置文件为 /etc/nail.rc，mailx 的配置文件为 /etc/mail.rc。在 Ubuntu 和 Debian 系统下，与 heirloom-mailx 功能相似的程序还有 bsd-mailx，但 bsd-mailx 不能使用外部的 MTA。

安装完成后会生成 2 个新命令 mail 和 mailx，可以发现 mail 命令是 mailx 命令的别名，也就意味着这两个命令效果完全一样。

## 配置
假设要使用 163 的邮箱发送邮件，并且通过 SSL 协议进行登录。通过 163 邮箱的设置页面可知，用来发信的服务器地址为 smtp.163.com，SSL 端口为 465。开启 smtp服务并获取授权码(也就是专门用于 MUA 认证用的密码)。

由于 mailx 默认连接的是本地的 MTA，如 Sendmail、Postfix 等，所以不需要使用 SSL 连接。而我们需要连接的是外部的 SMTP 服务器，所以使用 SSL/TLS 协议进行通信是最好的选择。但这就需要我们自行配置根证书数据库(mailx 默认未配置证书数据库)。mailx 先获取到外部 SMTP 服务器的证书链，然后通过 NSS 来检测证书链是否真的是由其声称的根证书签发的，检测所需就是我们配置的证书数据库中的根证书。

## 获取邮箱服务器的根证书

先来查看 163 邮箱服务器证书链的信息：

```
$ openssl s_client -connect smtp.163.com:465 -showcerts 2>&1 </dev/null | grep depth
```

输出内容如下：

depth=2 C = US, O = GeoTrust Inc., CN = GeoTrust Global CA
depth=1 C = US, O = GeoTrust Inc., CN = GeoTrust SSL CA - G3
depth=0 C = CN, ST = ZheJiang, L = HangZhou, O = "NetEase (Hangzhou) Network Co., Ltd", CN = *.163.com
输出结果最上面的 GeoTrust Global CA 即是其根证书名（注意：根证书可能会变化，以你看到的为准），于是到 GeoTrust 官网的根证书下载页面找到对应名称的根证书下载地址进行下载:


```bash
$ wget https://www.geotrust.com/resources/root_certificates/certificates/GeoTrust_Global_CA.pem

```
如果是 QQ 邮箱，则访问 DigiCert 的根证书下载页面。

## 将根证书导入证书数据库
先找个地方创建一个存放证书数据库的目录：

```bash
$ sudo mkdir /path/to/
```
将证书导入数据库：

```bash
$ sudo certutil -A -n "GeoTrust Global CA" -t "C,C,C" -d /path/to/mailx_nssdb -i GeoTrust_Global_CA.pem
```

上面的命令中，-A 选项表示将一个证书导入进证书数据库，如果数据库不存在，则初始化一个数据库。-n 选项指定证书的存放名称，如果名称已存在，会报错。-t 选项指定证书的信任级别（C 代表受信任的根证书，用逗号区分范围，查看这篇文档了解 NSS 对证书信任级别的分类）。-d 选项指定存放证书数据库的目录，这个目录必须提前创建好。-i 选项指定需要导入的证书，不区分证书格式。关于 certutil 命令的更多信息可以查看这篇文档：NSS 工具 certutil。

导入后，可以在 mailx_nssdb 目录下看到 cert8.db、key3.db、secmod.db 这三个数据库文件。

可以用下面的命令查看数据库内的证书情况：

```bash
$ sudo certutil -L -d /path/to/mailx_nssdb/
```
输出：

```
Certificate Nickname                                         Trust Attributes
                                                             SSL,S/MIME,JAR/XPI

GeoTrust Global CA                                           C,C,C
```
可见根证书已被导入进去。把根证书导入到数据库之后就可以删除下载的根证书文件了。

```bash
$ rm ~/GeoTrust_Global_CA.pem
```
## 编辑 mailx 配置文件

打开 mailx 的配置文件

```bash
$ sudo vi /etc/mail.rc
```
在最后添加如下内容：

```python
## 设置证书数据库所在目录
set nss-config-dir=/path/to/mailx_nssdb

# 设置邮件服务器地址
set smtp=smtps://smtp.163.com:465
# 设置用户名
set smtp-auth-user=example@163.com
# 设置密码，也就是授权码
set smtp-auth-password=mypassword
# 设置邮件发信地址
set from=example@163.com
```
#### 参数解析：

`nss-config-dir`: mailx 使用的是 NSS 密码学工具库，而不是 Openssl，所以需要指定 NSS 的证书库。
smtp：这一项设置之后，mailx 将不再连接本地 MTA，转而连接这一项参数配置的 MTA 服务器地址。如果指定以 smtps:// 的方式连接，mailx 会检查 nss-config-dir 这一样是否被正确设置。
smtp-auth-user、smtp-auth-password 和 from 这三个参数可以在 mailx 命令中以参数的形式设置，以避免邮箱账户信息被服务器的其他用户看到。
smtp-auth 参数指定认证方式。在上面的配置文件中并没有添加这一项，是因为当 smtp-auth-user 被设置后，mailx 的默认认证方式就是 login。其它可选值为 cram-md5 及 plain。如果 smtp-auth 和 smtp-auth-user 都没有被设置，则不会进行 STMP 认证。
ssl-verify 参数指定在验证服务器证书有效性时发生了错误该怎么处理。可设置的值为：1、strict，判定为失败时立即关闭连接。2、ask，用交互式方法询问是否继续。3、warn，打印一个警告并继续。4、ignore，不执行验证。默认值是 ask。由于我们在上面已经配置好了证书数据库，所以在调试的时候可以以默认值运行，调试成功后可以配置为 strict。

## 使用

### 交互式

这种方式主要用于测试，更人性化一些：

```bash
$ mail example@qq.com    # 首先输入你的目标邮箱，然后回车
Subject: 这是一封测试邮件   # 然后mailx会问询你的邮件主题，输入后回车
this is a test.          # 然后你就可以输入正文了
Bye!
.                        #正文输入完成后在新的一行输入英文句号表示结束，回车后发送
EOT
```
### 管道

mailx 可以从标准输入(stdin)中读取内容作为邮件内容。使用这种方式可以很好的和脚本或其他命令结合：

```bash
$ echo "你好，世界！" | mail -s "hello world" example@qq.com
```

上面命令中的 -s 选项指定的是邮件主题。

### 输入重定向

利用输入重定向可以将文件内容作为邮件内容发送：

```bash
$ mail -s this is a test" example@qq.com < /path/to/file
```
### 多个收件人

多个收件人之间使用逗号分隔：

```bash
$ echo "你好，世界！" | mail -s "hello world" example1@qq.com,example2@qq.com
```
### 抄送和密送

使用 -c 选项添加 CC 地址，使用 -b 选项添加 BCC 地址：

```bash
$ echo "你好，世界！" | mail -s "hello world" -c ccaddress@qq.com -b bccaddress@qq.com example@qq.com
```
### 指定发件人地址和姓名

使用 -r 选项来指定发件人地址和姓名，发件人名字在前，发件人地址使用尖括号括起来：

```bash
$ echo "你好，世界！" | mail -s "hello world" -r "Foo<bar@qq.com>" example@qq.com
```

### 添加附件

使用 -a 选项来指定要添加的附件：

```bash
$ echo "你好，世界！" | mail -s "hello world" -a /path/to/file example@qq.com
```
### 在命令中设定参数

在前面，我们已经在 /etc/mail.rc 这个配置文件中设定了一些参数，这些参数可以通过 -S (大写)选项进行覆盖，格式是：-S "参数名=参数值"。通过这个选项，我们就可以在命令中动态的设置 STMP 服务器配置信息：

```bash
$ echo "你好，世界！" | mail -s "hello world" \
> -S "smtp=smtps://smtp.163.com:465" \
> -S "smtp-auth-user=example@163.com" \
> -S "smtp-auth-password=mypassword" \
> -S "from=example@163.com" \
> example@qq.com
```
### 查看与 STMP 服务器的通信过程

通过 -v 选项，可以在终端打印出与 STMP 服务器的通信过程。

```bash
$ echo "你好，世界！" | mail -v -s "hello world" example@qq.com
```
### 更多

有关mailx的更多详细信息可以通过 Linux 自带的手册查看：

```bash
$ man 1 mailx
```
参考文章：[https://www.practicemp.com/2018/07/mailx.html](https://www.practicemp.com/2018/07/mailx.html)

# 在Django中创建email分享模块

## 通过email分享帖子

首先，我们会允许用户通过发送邮件来分享他们的帖子。让我们花费一小会时间来想下，根据在上一章中学到的知识，你该如何使用views，urls和templates来创建这个功能。现在，核对一下你需要哪些才能允许你的用户通过邮件来发送帖子。你需要做到以下几点：

给用户创建一个表单来填写他们的姓名，email，收件人以及评论，评论不是必选项。
在views.py文件中创建一个视图（view）来操作发布的数据和发送email
在blog应用的urls.py中为新的视图（view）添加一个URL模式
创建一个模板（template）来展示这个表单
使用Django创建表单
让我们开始创建一个表单来分享帖子。Django有一个内置的表单框架允许你通过简单的方式来创建表单。这个表单框架允许你定义你的表单字段，指定这些字段必须展示的方式，以及指定这些字段如何验证输入的数据。Django表单框架还提供了一种灵活的方式来渲染表单以及操作数据。

Django提供了两个可以创建表单的基本类：

- Form: 允许你创建一个标准表单
- ModelForm: 允许你创建一个可用于创建或者更新model实例的表单

首先，在你blog应用的目录下创建一个forms.py文件，输入以下代码：

```pyhton
from django import forms

class EmailPostForm(forms.Form):
    name = forms.CharField(max_length=25)
    email = forms.EmailField()
    to = forms.EmailField()
    comments = forms.CharField(required=False,
                                         widget=forms.Textarea)
```

这是你的第一个Django表单。看下代码：我们已经创建了一个继承了基础Form类的表单。我们使用不同的字段类型以使Django有依据的来验证字段。

表单可以存在你的Django项目的任何地方，但按照惯例将它们放在每一个应用下面的forms.py文件中

name字段是一个CharField。这种类型的字段被渲染成<input type=“text”>HTML元素。每种字段类型都有默认的控件来确定它在HTML中的展示形式。通过改变控件的属性可以重写默认的控件。在comment字段中，我们使用<textarea></textarea>HTML元素而不是使用默认的<input>元素来显示它。

字段验证取决于字段类型。例如，email和to字段是EmailField,这两个字段都需要一个有效的email地址，否则字段验证将会抛出一个forms.ValidationError异常导致表单验证不通过。在表单验证的时候其他的参数也会被考虑进来：我们将name字段定义为一个最大长度为25的字符串；通过设置required=False让comments的字段可选。所有这些也会被考虑到字段验证中去。目前我们在表单中使用的这些字段类型只是Django支持的表单字段的一部分。要查看更多可利用的表单字段，你可以访问：https://docs.djangoproject.com/en/1.8/ref/forms/fields/

在视图（views）中操作表单
当表单成功提交后你必须创建一个新的视图（views）来操作表单和发送email。编辑blog应用下的views.py文件，添加以下代码：

```python
from .forms import EmailPostForm

def post_share(request, post_id):
    # retrieve post by id
    post = get_object_or_404(Post, id=post_id, status='published')
    cd = None
    if request.method == 'POST':
        # Form was submitted
        form = EmailPostForm(request.POST)
        if form.is_valid():
            # Form fields passed validation
            cd = form.cleaned_data
            # ... send email
    else:
        form = EmailPostform()
    return render(request, 'blog/post/share.html', 
                          {'post': post,
                          'form': form,
                          'cd':cd})
```

该视图（view）完成了以下工作：

我们定义了post_share视图，参数为request对象和post_id。
我们使用get_object_or_404快捷方法通过ID获取对应的帖子，并且确保获取的帖子有一个published状态。
我们使用同一个视图（view）来展示初始表单和处理提交后的数据。我们会区别被提交的表单和不基于这次请求方法的表单。我们将使用POST来提交表单。如果我们得到一个GET请求，一个空的表单必须显示，而如果我们得到一个POST请求，则表单需要提交和处理。因此，我们使用request.method == 'POST'来区分这两种场景。
下面是展示和操作表单的过程：

1. 通过GET请求视图（view）被初始加载后，我们创建一个新的表单实例，用来在模板（template）中显示一个空的表单：

  form = EmailPostForm()
2. 当用户填写好了表单并通过POST提交表单。之后，我们会用保存在request.POST中提交的数据创建一个表单实例。

```
  if request.method == 'POST':
     #Form was submitted
     form = EmailPostForm(request.POST)
```
3. 在以上步骤之后，我们使用表单的is_valid()方法来验证提交的数据。这个方法会验证表单引进的数据，如果所有的字段都是有效数据，将会返回True。一旦有任何一个字段是无效的数据，is_valid()就会返回False。你可以通过访问 form.errors来查看所有验证错误的列表。

4. 如果表单数据验证没有通过，我们会再次使用提交的数据在模板（template）中渲染表单。我们会在模板（template）中显示验证错误的提示。

5. 如果表单数据验证通过，我们通过访问form.cleaned_data获取验证过的数据。这个属性是一个表单字段和值的字典。

如果你的表单数据没有通过验证，cleaned_data只会包含验证通过的字段

现在，你需要学习如何使用Django来发送email,把所有的事情结合起来。

# 使用Django发送email

使用Django发送email非常简单。首先，你需要有一个本地的SMTP服务或者通过在你项目的settings.py文件中添加以下设置去定义一个外部SMTP服务器的配置：

```bash
EMAIL_HOST: SMTP服务地址。默认本地。
EMAIL_POSR: SMATP服务端口，默认25。
EMAIL_HOST_USER: SMTP服务的用户名。
EMAIL_HOST_PASSWORD: SMTP服务的密码。
EMAIL_USE_TLS: 是否使用TLS加密连接。
EMAIL_USE_SSL: 是否使用隐式的SSL加密连接。
```

如果你没有本地SMTP服务，你可以使用你的email服务供应商提供的SMTP服务。下面提供了一个简单的例子展示如何通过使用Google账户的Gmail服务来发送email：

```bash
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER = 'your_account@gmail.com'
EMAIL_HOST_PASSWORD = 'your_password'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
```

运行命令python manage.py shell来打开Python shell，发送一封email如下所示：

```python
>>> from django.core.mail import send_mail
>>> send_mail('Django mail', 'This e-mail was sent with Django.','your_account@gmail.com', ['your_account@gmail.com'], fail_silently=False)

```

send_mail()方法需要这些参数：邮件主题，内容，发送人以及一个收件人的列表。通过设置可选参数fail_silently=False，我们告诉这个方法如果email没有发送成功那么需要抛出一个异常。如果你看到输出是1，证明你的email发送成功了。如果你使用之前的配置用Gmail来发送邮件，你可能需要去 https://www.google.com/settings/security/lesssecureapps 去开通一下低安全级别应用的权限。(译者注：练习时老老实实用QQ邮箱吧）

现在，我们要将以上代码添加到我们的视图（view）中。在blog应用下的views.py文件中编辑post_share视图（view）如下所示：

```python
from django.core.mail import send_mail

def post_share(request, post_id):
    ## Retrieve post by id
    post = get_object_or_404(Post, id=post_id, status='published')
    sent = False
    cd = None
    if request.method == 'POST':
        ## Form was submitted
        form = EmailPostForm(request.POST)
        if form.is_valid():
            ## Form fields passed validation
            cd = form.cleaned_data
            post_url = request.build_absolute_uri(
                                    post.get_absolute_url())
            subject = '{} ({}) recommends you reading "{}"'.format(cd['name'], cd['email'], post.title)
            message = 'Read "{}" at {}\n\n{}\'s comments: {}'.format(post.title, post_url, cd['name'], cd['comments'])
            send_mail(subject, message, 'admin@myblog.com',[cd['to']])
            sent = True
    else:
        form = EmailPostForm()
        
    return render(request, 'blog/post/share.html', {'post': post,
                                                    'form': form,
                                                    'sent': sent,
                                                    'cd':cd})
```

请注意，我们声明了一个sent变量并且当帖子被成功发送时赋予它True。当表单成功提交的时候，我们之后将在模板（template）中使用这个变量显示一条成功提示。由于我们需要在email中包含帖子的超链接，所以我们通过使用post.get_absolute_url()方法来获取到帖子的绝对路径。我们将这个绝对路径作为request.build_absolute_uri()的输入值来构建一个完整的包含了HTTP schema和主机名的url。我们通过使用验证过的表单数据来构建email的主题和消息内容并最终给表单to字段中包含的所有email地址发送email。

现在你的视图（view）已经完成了，别忘记为它去添加一个新的URL模式。打开你的blog应用下的urls.py文件添加post_share的URL模式如下所示：

```python
urlpatterns = [
# ...
url(r'^(?P<post_id>\d+)/share/$', views.post_share,
    name='post_share'),
]
```

在模板（templates）中渲染表单
在通过创建表单，编写视图（view）以及添加URL模式后，我们就只剩下为这个视图（view）添加模板（tempalte）了。在blog/templates/blog/post/目录下创建一个新的文件并命名为share.html。在该文件中添加如下代码：

```
{ % extends "blog/base.html" % }

{ % block title %}Share a post{% endblock % }

{ % block content % } 
  { % if sent % }
    <h1>E-mail successfully sent</h1>
    <p>
      "{{ post.title }}" was successfully sent to {{ cd.to }}.
    </p>
  { % else % }
    <h1>Share "{{ post.title }}" by e-mail</h1>
    <form action="." method="post">
      {{ form.as_p }}
      { % csrf_token % }
      <input type="submit" value="Send e-mail">
    </form>
  { % endif % }
{ % endblock % }
```

这个模板（tempalte）专门用来显示一个表单或一条成功提示信息。如你所见，我们创建的HTML表单元素里面表明了它必须通过POST方法提交：

<form action="." method="post">
接下来我们要包含真实的表单实例。我们告诉Django用as_p方法利用HTML的<p>元素来渲染它的字段。我们也可以使用as_ul利用无序列表来渲染表单或者使用as_table利用HTML表格来渲染。如果我们想要逐一渲染每一个字段，我们可以迭代字段。例如下方的例子：

```
{% for field in form %}
  <div>
    {{ field.errors }}
    {{ field.label_tag }} {{ field }}
  </div>
{% endfor %}
```
{% csrf_token %}模板（tempalte）标签（tag）引进了可以避开Cross-Site request forgery(CSRF)攻击的自动生成的令牌，这是一个隐藏的字段。这些攻击由恶意的站点或者可以在你的站点中为用户执行恶意行为的程序组成。通过访问 https://en.wikipedia.org/wiki/Cross-site_request_forgery你可以找到更多的信息 。

上述的标签（tag）生成的隐藏字段就像下面一样：

```
<input type='hidden' name='csrfmiddlewaretoken' value='26JjKo2lcEtYkGoV9z4XmJIEHLXN5LDR' />
```
默认情况下，Django在所有的POST请求中都会检查CSRF标记（token）。请记住要在所有使用POST方法提交的表单中包含csrf_token标签。（译者注：当然你也可以关闭这个检查，注释掉app_list中的csrf应用即可，我就是这么做的，因为我懒）

编辑你的blog/post/detail.html模板（template），在{{ post.body|linebreaks }}变量后面添加如下的链接来分享帖子的URL:


```
<p>
  <a href="{% url "blog:post_share" post.id %}">
    Share this post
  </a>
</p>
```
请记住，我们通过使用Django提供的{% url %}模板（template）标签（tag）来动态的生成URL。我们以blog为命名空间，以post_share为URL，同时传递帖子ID作为参数来构建绝对的URL。

现在，通过python manage.py runserver命令来启动开发服务器，在浏览器中打开 http://127.0.0.1:8000/blog/ 。点击任意一个帖子标题查看详情页面。在帖子内容的下方，你会看到我们刚刚添加的链接

参考文章：[Django By Example](https://github.com/Django-By-Example-ZH/Django-By-Example-ZH/blob/master/Django%20By%20Example%E7%AC%AC%E4%B8%89%E7%AB%A0%E7%BF%BB%E8%AF%91.md)






