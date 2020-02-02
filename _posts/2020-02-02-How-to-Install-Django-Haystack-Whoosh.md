---
title: How to Install Django Haystack Whoosh
date: 2020-02-02 17:35:00
tags: [Django,Haystack,Whoosh]
music-id: 1317100959

---

[EN](#EN)|[CN](#CN)

<span id="EN">  
Django has a simple search function which is the built-in methods. But this search function is not enough for us,  we need a search engine at least it should be able to sort search results and highlight keywords according to users' search keywords. Now let's use Django haystack to implement these features

# [Django haystack](https://django-haystack.readthedocs.io/en/master/tutorial.html)

Django haystack is a Django third-party application that provides search function. It supports [Solr](http://lucene.apache.org/solr/), [Elasticsearch](https://en.wikipedia.org/wiki/Elasticsearch), [Whoosh](https://whoosh.readthedocs.io/en/latest/intro.html), [Xapian](https://www.sphinx-doc.org/en/1.7/intro.html), Simple and other search engines. With the famous Chinese natural language processing library [Jieba](https://github.com/fxsjy/jieba), it can provide a blog article search system with good results.

# Why Choose Whoosh
[Trends of them](https://trends.google.com/trends/explore?geo=US&q=Solr,Elasticsearch,Whoosh,Xapian),you will see the Elasticsearch is most hotest today

| SE | pros | cons |
| :----: | :----: |:----: |
| ![Elasticsearch](http://p3.pstatp.com/large/pgc-image/1536724508881d0710df381) | 1. Horizontal scalability: just add a server, do a little configuration, and start the ES process to merge into the cluster;   2. The fragmentation mechanism provides better distribution: the same index is divided into multiple sharding, which is similar to the block mechanism of HDFS; the way of partition and governance to improve processing efficiency is believed to be familiar to all;  3. High availability: provide recovery Replica mechanism: one partition can set multiple replications, so that when a server goes down, the cluster can still run as usual, and will restore the replication lost due to the server down to other available nodes; this is similar to the HDFS replication mechanism (the default is 3 replications in HDFS) | 1. Consistency of each node  2. There is no detailed authority management mechanism  3. Only JSON file format is supported |
| ![Solr](http://p9.pstatp.com/large/pgc-image/15367244957772b65ad19d1) | 1. Solr is a mature project  2. Solr supports more data formats, such as JSON, XML and CSV  3. Solr performs better than Elasticsearch in traditional search applications | The efficiency of processing real-time search application is significantly lower than Elasticsearch |
| ![Whoosh](https://carlchenet.com/wp-content/uploads/2013/08/whoosh-logo.png?w=150) | 1. Whoosh is written purely in Python, but soon, it only needs Python environment and no compiler;  2. Okapi bm25f sorting algorithm is used by default, and other sorting algorithms are also supported;  3. Compared with other search engines, whoosh will create smaller index files;  4. Index file code in whoosh must be Unicode;  5. Whoosh can store any Python object ||
| ![Xapian](http://p1.pstatp.com/large/pgc-image/15367251838327903291c8e) | 1. 50% faster than Lucene in indexing events  2. Support real-time search|  The index file is twice larger than Lucene |

# Requirements

```python 
pip install whoosh django-haystack jieba

```

jieba-The best python lib for Chinese

# Configure haystack

After installing Django haystack, you need to do some simple configuration in settings.py of the project.  
First, add Django haystack to the **INSTALLED_APPS** and tell the Django where is the haystack and whoosh:  

{% highlight ruby linenos %}

TorD0/settings.py
 
INSTALLED_APPS = [
    'django.contrib.admin',
    # other app...
    'haystack',
    'TorB0',
    'comments',
]

HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'TorB0.whoosh_cn_backends.WhooshEngine',
        'PATH': os.path.join(BASE_DIR, 'whoosh_index'),
    },
}
HAYSTACK_SEARCH_RESULTS_PER_PAGE = 10 #Shows 10 results on one page
HAYSTACK_SIGNAL_PROCESSOR = 'haystack.signals.RealtimeSignalProcessor'


{% endhighlight %}

**HAYSTACK_CONNECTIONS** uses the `TorB0.whoosh_cn_backends.WhooshEngine` for Engine, you need to copy the haystack/backends/whoosh_backend.py to th app which is TorB0. Then change the name to **whoosh_cn_backends.py**, 
change the code here:  
```
schema_fields[field_class.index_fieldname] = TEXT(stored=True, analyzer=StemmingAnalyzer(), field_boost=field_class.boost, sortable=True)
```
change to  

```python
schema_fields[field_class.index_fieldname] = TEXT(stored=True, analyzer=ChineseAnalyzer(), field_boost=field_class.boost, sortable=True)
```
Add this code :

```python
from jieba.analyse import ChineseAnalyzer
```

Although this engine does not exist at present, we will create it next.Because we need it suits for the Chinese stop_words with `jieba` 
`Path` specifies the location where the index file needs to be stored. We set it to the whoosh_index folder under the `BASE_DIR`of the project TorB0(it will be created automatically when the index is established).

`HAYSTACK_SIGNAL_PROCESSOR` When update the index

`haystack.signals.RealtimeSignalProcessor` Automatically handles updates/deletes for you with the index real-time when the article updates

# Handling Data
## Creating SearchIndexes
Creata a file which is name search_indexes.py below TorB0 

{% highlight ruby linenos %}

from haystack import indexes
from .models import Post

class PostIndex(indexes.SearchIndex, indexes.Indexable):
    text = indexes.CharField(document=True, use_template=True)
    publish = indexes.DateTimeField(model_attr='publish')
    
    def get_model(self):
        return Post
        
    def index_queryset(self, using=None):
        return self.get_model().published.all()

{% endhighlight %}

 The Classname must be **XXIndex** the **XX** is the name of Model of retrieved data such as **Post** tell the haystack which data need to be index by whoosh

`SearchIndex` objects are the way Haystack determines what data should be placed in the search index and handles the flow of data in. You can think of them as being similar to Django `Models` or `Forms` in that they are field-based and manipulate/store data.

Now we need to create the dirs and file ins the templates dir

the default path

`templates/search/indexes/youapp/\<model_name>_text.txt`

mysite path is 
`search/indexes/TorB0/post_text.`

place the following inside:  

```
{{ object.title }}
{{ object.tags.all|join:", " }}
{{ object.body }}
```

# Add The SearchView To Your URLconf

Add the code to your project(TorD0)'s urls.py

```python
urlpatterns = [
    # other url...
    url(r'^search/', include('haystack.urls')),
]

```

# Search Template

Your search template (search/search.html for the default case) will likely be very simple. The following is enough to get going (your template/block names will likely differ):

{% highlight ruby linenos %}

templates/search/search.html
 
{% extends 'base.html' %}
{% load highlight %}
 
{% block content %}
    <h2>Search</h2>
     <form method="get" action=".">
        <table>
            {{ form.as_table }}
            <tr>
                <td>&nbsp;</td>
                <td>
                    <input type="submit" value="Search">
                </td>
            </tr>
        </table>
    {% if query %}
        {% for result in page.object_list %}
            <article class="post post-{{ result.object.pk }}">
                <header class="entry-header">
                    <h1 class="entry-title">
                        <a href="{{ result.object.get_absolute_url }}">{% highlight result.object.title with query %}</a>
                    </h1>
                    <div class="entry-meta">
                    <span class="post-category">
                        <a href="{% url 'blog:category' result.object.category.pk %}">
                            {{ result.object.category.name }}</a></span>
                        <span class="post-date"><a href="#">
                            <time class="entry-date" datetime="{{ result.object.created_time }}">
                                {{ result.object.created_time }}</time></a></span>
                        <span class="post-author"><a href="#">{{ result.object.author }}</a></span>
                        <span class="comments-link">
                        <a href="{{ result.object.get_absolute_url }}#comment-area">
                            {{ result.object.comment_set.count }} comments</a></span>
                        <span class="views-count"><a
                                href="{{ result.object.get_absolute_url }}">{{ result.object.views }} views</a></span>
                    </div>
                </header>
                <div class="entry-content clearfix">
                    <p>{% highlight result.object.body with query %}</p>
                    <div class="read-more cl-effect-14">
                        <a href="{{ result.object.get_absolute_url }}" class="more-link">continue <span
                                class="meta-nav">→</span></a>
                    </div>
                </div>
            </article>
        {% empty %}
            <div class="no-post">No results!</div>
        {% endfor %}
        {% if page.has_previous or page.has_next %}
            <div>
                {% if page.has_previous %}
                    <a href="?q={{ query }}&amp;page={{ page.previous_page_number }}">{% endif %}&laquo; Previous
                {% if page.has_previous %}</a>{% endif %}
                |
                {% if page.has_next %}<a href="?q={{ query }}&amp;page={{ page.next_page_number }}">{% endif %}Next
                &raquo;{% if page.has_next %}</a>{% endif %}
            </div>
        {% endif %}
    {% else %}
        Please input a keyword like: django
    {% endif %}
    </form>
{% endblock %}

{% endhighlight %}

Note that the `page.object_list` is actually a list of `SearchResult` objects instead of individual models. These objects have all the data returned from that record within the search index as well as score. They can also directly access the model for the result via `{{ result.object }}`. So the `{{ result.object.title }}` uses the actual `Post` object in the database and accesses its title field.

# [Highlight keywords](https://django-haystack.readthedocs.io/en/master/templatetags.html#highlight)
```python
#Use default
{% highlight result.summary with query %}

#Highlight summary but wrap highlighted words with a div and the following CSS class.
{% highlight result.summary with query html_tag "div" css_class "highlight_me_please" %}

#You can limit the length of the highlighted {result. Summary} by Max [length]
{% highlight result.summary with query max_length 40 %}
```

The principle of highlighting is to add a `span` label to the keyword package in the text and add the highlighted style to it (of course, you can also modify the default behavior, see the usage given above for details). Therefore, we also need to specify a style for the highlighted class, which can be added in base.html:

{% highlight ruby linenos %}
base.html
 
<head>
    <title>Black &amp; White</title>
    ...
    <style>
        span.highlighted {
            color: red;
        }
    </style>
    ...
</head>
{% endhighlight %}

# Reindex

Simply run `./manage.py rebuild_index`. You’ll get some totals of how many models were processed and placed in the index.


<span id="CN">

# Django Haystack 简介
django-haystack 是一个专门提供搜索功能的 django 第三方应用，它支持 Solr、Elasticsearch、Whoosh、Xapian 等多种搜索引擎，配合著名的中文自然语言处理库 jieba 分词，就可以为我们的博客提供一个效s果不错的博客文章搜索系统。

# 安装必要依赖
要使用 django haystack，首先必须安装它，并且安装一些必要的依赖，具体需要安装的依赖有：

- Whoosh。Whoosh 是一个由纯 Python 实现的全文搜索引擎，没有二进制文件等，比较小巧，配置简单方便。
- jieba 中文分词。由于 Whoosh 自带的是英文分词，对中文的分词支持不是太好，所以使用 jieba 替换Whoosh 的分词组件。

直接使用 pip 安装这些包即可（安装到你使用的虚拟环境下）：pip install whoosh django-haystack jieba

# 配置 Haystack
安装好 django haystack 后需要在项目的 settings.py 做一些简单的配置。

首先是把 django haystack 加入到 INSTALLED_APPS 选项里：

{% highlight ruby linenos %}

TorD0/settings.py
 
INSTALLED_APPS = [
    'django.contrib.admin',
    # 其它 app...
    'haystack',
    'blog',
    'comments',
]
 
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'TorB0.whoosh_cn_backends.WhooshEngine',
        'PATH': os.path.join(BASE_DIR, 'whoosh_index'),
    },
}
HAYSTACK_SEARCH_RESULTS_PER_PAGE = 10
HAYSTACK_SIGNAL_PROCESSOR = 'haystack.signals.RealtimeSignalProcessor'

{% endhighlight %}

HAYSTACK_CONNECTIONS 的 ENGINE 指定了 django haystack 使用的搜索引擎，这里我们使用了 TorB0.whoosh_cn_backends.WhooshEngine，虽然目前这个引擎还不存在，但我们接下来会创建它。PATH 指定了索引文件需要存放的位置，我们设置为项目根目录 BASE_DIR 下的 whoosh_index 文件夹（在建立索引是会自动创建）。

HAYSTACK_SEARCH_RESULTS_PER_PAGE 指定如何对搜索结果分页，这里设置为每 10 项结果为一页。

HAYSTACK_SIGNAL_PROCESSOR 指定什么时候更新索引，这里我们使用 haystack.signals.RealtimeSignalProcessor，作用是每当有文章更新时就更新索引。由于博客文章更新不会太频繁，因此实时更新没有问题。

处理数据
接下来就要告诉 django haystack 使用那些数据建立索引以及如何存放索引。如果要对 blog 应用下的数据进行全文检索，做法是在 blog 应用下建立一个 search_indexes.py 文件，写上如下代码：

{% highlight ruby linenos %}

from haystack import indexes
from .models import Post

class PostIndex(indexes.SearchIndex, indexes.Indexable):
    text = indexes.CharField(document=True, use_template=True)
    publish = indexes.DateTimeField(model_attr='publish')
    
    def get_model(self):
        return Post
        
    def index_queryset(self, using=None):
        return self.get_model().published.all()

{% endhighlight %}

这是 django haystack 的规定。要相对某个 app 下的数据进行全文检索，就要在该 app 下创建一个 search_indexes.py 文件，然后创建一个 `XXIndex` 类（XX 为含有被检索数据的模型，如这里的 Post），并且继承 SearchIndex 和 Indexable。

## 为什么要创建索引？  
索引就像是一本书的目录，可以为读者提供更快速的导航与查找。在这里也是同样的道理，当数据量非常大的时候，若要从这些数据里找出所有的满足搜索条件的几乎是不太可能的，将会给服务器带来极大的负担。所以我们需要为指定的数据添加一个索引（目录），在这里是为 Post 创建一个索引，索引的实现细节是我们不需要关心的，我们只关心为哪些字段创建索引，如何指定。

每个索引里面必须有且只能有一个字段为 document=True，这代表 django haystack 和搜索引擎将使用此字段的内容作为索引进行检索(primary field)。注意，如果使用一个字段设置了document=True，则一般约定此字段名为text，这是在 SearchIndex 类里面一贯的命名，以防止后台混乱，当然名字你也可以随便改，不过不建议改。

并且，haystack 提供了use_template=True 在 text 字段中，这样就允许我们使用数据模板去建立搜索引擎索引的文件，说得通俗点就是索引里面需要存放一些什么东西，例如 Post 的 title 字段，这样我们可以通过 title 内容来检索 Post 数据了。举个例子，假如你搜索 Python ，那么就可以检索出 title 中含有 Python 的Post了，怎么样是不是很简单？数据模板的路径为 templates/search/indexes/youapp/\<model_name>_text.txt（例如 templates/search/indexes/TorB0/post_text.txt），其内容为：

`templates/search/indexes/TorB0/post_text.txt`
 
```
{{ object.title }}
{{ object.tags.all|join:", " }}
{{ object.body }}
```
这个数据模板的作用是对 Post.title、Post.body Post.tags这三个字段建立索引，当检索的时候会对这三个字段做全文检索匹配，然后将匹配的结果排序后作为搜索结果返回。

# 配置 URL

接下来就是配置 URL，搜索的视图函数和 URL 模式 django haystack 都已经帮我们写好了，只需要项目的 urls.py 中包含它：

TorD0/urls.py
 
urlpatterns = [
    # 其它...
    url(r'^search/', include('haystack.urls')),
]
另外在此之前我们也为自己写的搜索视图配置了 URL，把那个 URL 删掉，以免冲突：

```
TorB0/urls.py
 
# url(r'^search/$', views.search, name='search'),

```

# 修改搜索表单

修改一下搜索表单，让它提交数据到 django haystack 搜索视图对应的 URL：

<form role="search" method="get" id="searchform" action="{% url 'haystack_search' %}">
  <input type="search" name="q" placeholder="搜索" required>
  <button type="submit"><span class="ion-ios-search-strong"></span></button>
</form>
主要是把表单的 action 属性改为 {% url 'haystack_search' %}

# 创建搜索结果页面

haystack_search 视图函数会将搜索结果传递给模板 search/search.html，因此创建这个模板文件，对搜索结果进行渲染：

{% highlight ruby linenos %}

templates/search/search.html
 
{% extends 'base.html' %}
{% load highlight %}
 
{% block content %}
    <h2>Search</h2>
     <form method="get" action=".">
        <table>
            {{ form.as_table }}
            <tr>
                <td>&nbsp;</td>
                <td>
                    <input type="submit" value="Search">
                </td>
            </tr>
        </table>
    {% if query %}
        {% for result in page.object_list %}
            <article class="post post-{{ result.object.pk }}">
                <header class="entry-header">
                    <h1 class="entry-title">
                        <a href="{{ result.object.get_absolute_url }}">{% highlight result.object.title with query %}</a>
                    </h1>
                    <div class="entry-meta">
                    <span class="post-category">
                        <a href="{% url 'blog:category' result.object.category.pk %}">
                            {{ result.object.category.name }}</a></span>
                        <span class="post-date"><a href="#">
                            <time class="entry-date" datetime="{{ result.object.created_time }}">
                                {{ result.object.created_time }}</time></a></span>
                        <span class="post-author"><a href="#">{{ result.object.author }}</a></span>
                        <span class="comments-link">
                        <a href="{{ result.object.get_absolute_url }}#comment-area">
                            {{ result.object.comment_set.count }} comments</a></span>
                        <span class="views-count"><a
                                href="{{ result.object.get_absolute_url }}">{{ result.object.views }} views</a></span>
                    </div>
                </header>
                <div class="entry-content clearfix">
                    <p>{% highlight result.object.body with query %}</p>
                    <div class="read-more cl-effect-14">
                        <a href="{{ result.object.get_absolute_url }}" class="more-link">continue <span
                                class="meta-nav">→</span></a>
                    </div>
                </div>
            </article>
        {% empty %}
            <div class="no-post">No results!</div>
        {% endfor %}
        {% if page.has_previous or page.has_next %}
            <div>
                {% if page.has_previous %}
                    <a href="?q={{ query }}&amp;page={{ page.previous_page_number }}">{% endif %}&laquo; Previous
                {% if page.has_previous %}</a>{% endif %}
                |
                {% if page.has_next %}<a href="?q={{ query }}&amp;page={{ page.next_page_number }}">{% endif %}Next
                &raquo;{% if page.has_next %}</a>{% endif %}
            </div>
        {% endif %}
    {% else %}
        Please input a keyword like: django
    {% endif %}
    </form>
{% endblock %}

{% endhighlight %}

这个模板基本和 TorB0/index.html 一样，只是由于 haystack 对搜索结果做了分页，传给模板的变量是一个 page 对象，所以我们从 page 中取出这一页对应的搜索结果，然后对其循环显示，即 {% for result in page.object_list %}。另外要取得 Post（文章）以显示文章的数据如标题、正文，需要从 result 的 object 属性中获取。query 变量的值即为用户搜索的关键词。

# [高亮关键词](https://django-haystack.readthedocs.io/en/master/templatetags.html#highlight)
注意到百度的搜索结果页面，含有用户搜索的关键词的地方都是被标红的，在 django haystack 中实现这个效果也非常简单，只需要使用 {% highlight %} 模板标签即可，其用法如下：

```
# 使用默认值  
{% highlight result.summary with query %}  
 
# 这里我们为 {{ result.summary }} 里所有的 {{ query }} 指定了一个<div></div>标签，并且将class设置为highlight_me_please，这样就可以自己通过CSS为{{ query }}添加高亮效果了，怎么样，是不是很科学呢  
{% highlight result.summary with query html_tag "div" css_class "highlight_me_please" %}  
 
# 可以 max_length 限制最终{{ result.summary }} 被高亮处理后的长度
{% highlight result.summary with query max_length 40 %}  
```

在博客文章搜索页中我们对 title、时间和 body 做了高亮处理：{% highlight result.object.title with query %}，{% highlight result.object.body with query %}。高亮处理的原理其实就是给文本中的关键字包上一个 span 标签并且为其添加 highlighted 样式（当然你也可以修改这个默认行为，具体参见上边给出的用法）。因此我们还要给 highlighted 类指定样式，在 base.html 中添加即可：

{% highlight ruby linenos %}

templates/base.html
 
<head>
    <title>Black &amp; White</title>
    ...
    <style>
        span.highlighted {
            color: red;
        }
    </style>
    ...
</head>
{% endhighlight %}


# 修改搜索引擎为中文分词

我们使用 Whoosh 作为搜索引擎，但在 django haystack 中为 Whoosh 指定的分词器是英文分词器，可能会使得搜索结果不理想，我们把这个分词器替换成 jieba 中文分词器。从你安装的 haystack 中把 haystack/backends/whoosh_backend.py 文件拷贝到 blog/ 下，重命名为 whoosh_cn_backends.py（之前我们在 settings.py 中 的 HAYSTACK_CONNECTIONS 指定的就是这个文件），然后找到如下一行代码：

`schema_fields[field_class.index_fieldname] = TEXT(stored=True, analyzer=**StemmingAnalyzer()**, field_boost=field_class.boost, sortable=True)`
将其中的 analyzer 改为 ChineseAnalyzer，当然为了使用它，你需要在文件顶部引入：from jieba.analyse import ChineseAnalyzer。

```python
from jieba.analyse import ChineseAnalyzer
```

```python
#注意先找到这个再修改，而不是直接添加  
schema_fields[field_class.index_fieldname] = TEXT(stored=True, analyzer=ChineseAnalyzer(),field_boost=field_class.boost, sortable=True)  
```
# 建立索引文件

最后一步就是建立索引文件了，运行命令 python manage.py rebuild_index 就可以建立索引文件了。

