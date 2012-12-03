---
date: 2012-12-03 16:00:00
layout: post
title: Django 教程之View Layer
categories:
    - Tech
tags:
    - Django
    - Web
    - Python
comments: yes
---

# Basics of View Layer

-------------------------------------
## URLConf
一个简洁，优美的URL对于网站来说至关重要，Django能让您随心所欲的设计URL，不必遵循种种不良的约定。[Cool URIS](http://www.w3.org/Provider/Style/URI)的重要性。

### Overview
Django使用URLConf模块实现URL的映射，由于URLConf是Pure Python，符合Dynamical 特性。

### How Django Processes a request
1. Django检测URLConf配置，通常在根目录下，由[Root_URLCONF](https://docs.djangoproject.com/en/1.4/ref/settings/#std:setting-ROOT_URLCONF)来设置. 
当**HttpRequest**请求中携带**urlconf**属性（**通常由middleware层进行设置，生成，处理**），urlconf属性会成为默认URL配置。
2. Django载入模块，开始寻找**urlpatterns**的值。urlpatterns是一个list,格式化成[django.conf.urls.patterns](https://docs.djangoproject.com/en/1.4/topics/http/urls/#django.conf.urls.patterns)。
3. Django匹配第一个遇到的URL.
4. 当成功匹配时，Django import 并且 call相应的view fuction(视图函数是Python Function 或者 [Class based view](https://docs.djangoproject.com/en/1.4/topics/class-based-views/))。
view Function 将HttpRequest作为第一个参数，将其余能在正则表达式中获取的参数作为剩余参数。
5. 如果没有匹配，或者在该view function处理中发生异常，Django将触发[Error-handling View](https://docs.djangoproject.com/en/1.4/topics/http/urls/#error-handling)机制，比如抛出404或500错误。

### example
1. Simple Example
{% highlight python %}
from django.conf.urls import patterns, url, include

urlpatterns = patterns('',
    (r'^articles/2003/$','news.specical2003'),
    (r'^articles/(\d{4})/(\d{2})/(\d+)/$','news.details'),
)
{% endhighlight%}
**Notes**:
    * 当从URL获取值时，仅需加入()即可,如(\d{4}),(\d+);
    * r 代表正则表达式，符合Python的正则表达式规则;
    * 针对上面的式子，当request是/articles/2012/12/1时，Django将call 
    {% highlight python %}
        news.details(request,'2012','12')
    {% endhighlight%}
    * 当request是/articles/2012时，不会match，因为缺少'/'
2. Named Groups
在Python Regular Expressions中，正则组用(?P<name>pattern) 表示。
    * name表示组名
    * pattern表示匹配模式
重写上面例子，
{% highlight python %}
urlpatterns = patterns('',
(r'^articles/(?P<year>\d{4})/$','news.views.year_archive'),
(r'^articles/(?P<year>\d{4})/(?P<month>\d{2})$','news.views.month'),
)
{% endhighlight%}
**note**
    * 其中year和month分别表示function中的变量名称
    * 分别对应如下
    {% highlight python %}
        news.views.year_archive(request,year='2012')
        news.views.month(request,year='2012',month='12')
    {% endhighlight%}
    * 这样做的优点显而易见，变量摆脱了顺序问题。

### URLConf Search 
    * 优先匹配
    * 忽略GET,POST,PUT参数，Domain名字. 各种参数对与URLConf来说都是一致的。
        * 在http://localhost/myapp/这个请求中，URLConf只寻找myapp/
        * 在http://localhost/myapp/?page=3这个请求中，GET参数被忽略，只处理myapp/

### Patterns 类解析
    * patterns(prefix,pattern_desc,...)
        * prefix前缀，一组模式，最后返回list
        * [Prefix](https://docs.djangoproject.com/en/1.4/topics/http/urls/#the-view-prefix)
-------------------------------------
## View Functions

-------------------------------------
## Shortcuts

-------------------------------------
## Decorators

-------------------------------------
# Reference of View Layer
## Request/response objects
## TemplateResponse objects

-------------------------------------
# File uploads of Django
## File objects
## Storage API
## Managing Files
## Custom Storage

-------------------------------------
# Generic views
## Built-in generic views

-------------------------------------
# Advanced of View Layer
## Generating CSV
## Generating PDF

-------------------------------------
# Middleware of View Layer
## Built-in Middle classes
