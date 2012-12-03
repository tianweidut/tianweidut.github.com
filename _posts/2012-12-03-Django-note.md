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
**example**
{% highlight python %}
from django.conf.urls import patterns, url, include

urlpatterns = patterns('',
    (r'^articles/2003/$','news.specical2003'),
    (r'^articles/(\d{4})/(\d{2})/(\d+)/$','news.details'),
)
{% endhighlight%}
## View Functions
## Shortcuts
## Decorators

# Reference of View Layer
## Request/response objects
## TemplateResponse objects

# File uploads of Django
## File objects
## Storage API
## Managing Files
## Custom Storage

# Generic views
## Built-in generic views

# Advanced of View Layer
## Generating CSV
## Generating PDF

# Middleware of View Layer
## Built-in Middle classes

