---
date: 2012-12-03 16:00:00
layout: post
title: Django 网站开发日志
categories:
    - Tech
tags:
    - Django
    - Web
    - Python
    - dev 
comments: yes
---

记录如何用Django开发一个可用的网站。

# Template 构建

  * Template 只要指定根文件夹，就可以通过相对路径进行索引 
  * {{% url name %}} url标签要在urls中通过name 参数指定。 这样能够实现URL的动态绑定，避免硬编码。
  * Template要使用settings中的变量，需要设置TEMPLATE_CONTEXT_PROCESSORS,返回context字典，这样就能实现template使用settings.py中的变量。
  实例如下,可以看[这里](http://stackoverflow.com/questions/3430451/using-django-settings-in-templates)：
    {% highlight python %}
        # in template file
        <p>{{settings.test}} 
        # in context.py file 
        # all_required is a content list
        def application_settings(request):
            """The context processor function"""""""
                mysettings = {}
                for keyword in all_required:
                    mysettings[keyword] = getattr(settings,keyword)

                context = { 'settings':mysettings, }
            return context
        # in settings.py
        add context.application_settings into TEMPLATE_CONTEXT_PROCESSORS 
    {% endhighlight %}
  * [Template Context Tutorial](http://www.b-list.org/weblog/2006/jun/14/django-tips-template-context-processors/) 
