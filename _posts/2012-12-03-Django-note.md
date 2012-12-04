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
**Simple Example**

{% highlight python %}
from django.conf.urls import patterns, url, include

urlpatterns = patterns('',
    (r'^articles/2003/$','news.specical2003'),
    (r'^articles/(\d{4})/(\d{2})/(\d+)/$','news.details'),
)
{% endhighlight %}

**Notes**

  * 当从URL获取值时，仅需加入()即可,如(\d{4}),(\d+);
  * r 代表正则表达式，符合Python的正则表达式规则;
  * 针对上面的式子，当request是/articles/2012/12/1时，Django将call 

    {% highlight python %}
        news.details(request,'2012','12')
    {% endhighlight %}

  * 当request是/articles/2012时，不会match，因为缺少'/'

**Named Groups**
在Python Regular Expressions中，正则组用(?P<name>pattern) 表示。
  
  * name表示组名
  * pattern表示匹配模式
  * pattern提取的参数是string,需要做类型转化。
重写上面例子

{% highlight python %}
urlpatterns = patterns('',
(r'^articles/(?P<year>\d{4})/$','news.views.year_archive'),
(r'^articles/(?P<year>\d{4})/(?P<month>\d{2})$','news.views.month'),
)
{% endhighlight %}

**Notes**

  * 其中year和month分别表示function中的变量名称
  * 分别对应如下
    
    {% highlight python %}
        news.views.year_archive(request,year='2012')
        news.views.month(request,year='2012',month='12')
    {% endhighlight %}

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
      * patterns() 是一个函数，最大接收255个参数，但可以用python的list相加的方式处理.
        {% highlight python %}
        patterns = patterns('',)
        patterns += patterns('',)
        {% endhighlight %}
  * url(regex,view,kwargs=None, name=None, prefix='')
      * nameing URL patterns 用同一个view function处理不同的URL 是一个常见的需求。
            {% highlight python %}
            urlpatterns = patterns('',
                (r'^archive/(\d{4})/$',archive,name='full-archive'),
                (r'^archive-summary/(\d{4})/$',archive,{'summary':True},name='arch-summary'),
                )
            {% endhighlight %}
      * 当在模板中可以用 {% url arch-summary 1990 %} , {% url full-archive 1099 %} 来实现分别调用

  * include(<module or pattern_list>)
      * 导入其他模块，或URLConf的patterns list变量.

### Error-handling
  * 自己定制错误处理[Customizing error views](https://docs.djangoproject.com/en/1.4/topics/http/views/#customizing-error-views)
  * handler403 Permissions Issue. 通常由CSRF产生. django.views.defaults->permission_denied
  * handler404 Not Found -> django.view.defaults.page_not_found
  * handler500 Server Error ->  django.view.defaults.server_error

### Including other URLconfs
  * 用于提交给其他木块的URLconfs进行进一步处理。
  * 样式如下：
        {% highlight python %}
        urlpatterns = patterns('', (r'^comments/',include('django.contrib,comments.urls')))
        {% endhighlight %}
  * 没有$结束符，并且用include函数，包含其他文件的patterns list变量，这样有利于分层管理。 

### Defining URL namespace
  * 当给一个应用程序部署多个实例时，使用namespace
  * URL namespace 来自两个部分：
      * application namespace 
      * instance namespace
  * (r'^help/',include('apps.help.urls',namespace='foo',app_name='bar')),同时也可以导入django object

### Passing extra options to view function
  * 以Python Dict的形式传递参数
  * 实例

        {% highlight python %}
            urlpatterns = patterns('blog.views',
            (r'^blog/(?P<year>\d{4})/$','year_archive',{'foo':'bar'}),)
        {% endhighlight %}

  * 在函数中就会调用额外的参数。这种技术用在syndication framework中传递metadata和views的options参数。

### Passing extra options to include()
  * 与view function 类似，参见[这里](https://docs.djangoproject.com/en/1.4/topics/http/urls/#the-view-prefix)

### Passing calling objects instead of strings
  * Django可以用更加自然的方法，用Python的object代替字符串实现调用。

-------------------------------------
## View Functions
简单来说是处理web Request请求，返回web response.
  * 404 返回, 会自动调用404错误页面进行响应。
    * Django 提供了404Exception
      {% highlight python %}
        from django.http import Http404
        raise Http404
      {% endhighlight %}
    * 系统自动调用在根目录的404.html, 但可以在URLConf中通过给handler404 = 'var' 指定特殊的View
  * 403,500 错误处理相同。
-------------------------------------
## Shortcuts
django.Shortcuts 集合提供MVC的多种类层次抽象。
  * render: 根据一个给定的模板和相应的变量，进行HttpResponse来rendered text.
    * requirements: request, template_name  
  * render_to_response
    * requirements: template_name
    * return render_to_response（'my_templates.html',my_data_dictionary,context_instance=RequestContext(request)
  * redirect: 返回一个HttpResponseRedirect
    * model: 模型的get_absolute_url()函数会被调用。
    * view name: 会需要额外参数urlresolvers.reverse()。
    * URL 地址。
  * get_object_or_404:
    * 调用给定模型的get()函数，会抛出Http404代替DoesNotExist.
  * get_list_or_404
    * 调用模型的filter(),会抛出Http404代替Empty

-------------------------------------
## Decorators
Django 和 Python中通过@，即decorators实现某些HTTP特性，比如用户验证等。
  * Allowed HTTP methods
    * 控制HTTP的请求方法, 在django.views.decorators.http 中,当发生错误时返回django.http.HttpResponseNotAllowed
    * example:
     {% highlight python  %}
        from django.views.decorators.http import require_http_methods
        @require_http_methods(["GET","POST"])  #需要大写
        def my_view(request):
            pass
     {% endhighlight %}
    * 其他: require_GET(), require_POST(), require_safe()
  * [Conditional View Processing](https://docs.djangoproject.com/en/1.4/topics/http/decorators/)
  * [Gzip Compression](https://docs.djangoproject.com/en/1.4/topics/http/decorators/)
  * [Vary headers](https://docs.djangoproject.com/en/1.4/topics/http/decorators/)

-------------------------------------
# Reference of View Layer
Django 通过Request和Response对象来传递状态。当一个页面被请求，Django将会创建HttpRequest对象，其中包含request的metadata。Django载入相应的View处理函数或类，
并将HttpRequest作为第一个参数，view function返回HttpResponse对象。 HttpRequest 和 HttpResponse 在django.http中定义。
## Request objects (HttpRequest objects)
### Attributes
所有的属性都是只读的。session是一个著名的只读异常。
  * HttpRequest.body: byte string. 当处理binary images 和 xml payload时使用。
  * HttpRequest.path: 返回完整的请求路径，不包括domain。
  * HttpRequest.path_info: 与path功能一致，只是去除prefix。
  * HttpRequest.method: 大写的'GET','POST' 
  * HttpRequest.encoding: None,表示DEFAULT_CHARSET。
  * HttpRequest.GET:返回dict-like, 参见[QueryDict](https://docs.djangoproject.com/en/1.4/ref/request-response/#django.http.QueryDict)
  * HttpRequest.POST: 返回dict-like, 参见[QueryDict](https://docs.djangoproject.com/en/1.4/ref/request-response/#django.http.QueryDict)
  * HttpRequest.COOKIES: 包含所有的cookies.
  * HttpRequest.FILES: 包含所有的uploaded files. 其中FILES中的name从
    {% highlight html %}
      <input type='file',name=''/>
    {% endhighlight %}
   中获取,每个value从[UploadFile](https://docs.djangoproject.com/en/1.4/ref/request-response/#django.http.UploadedFile)。注意FILES只有当请求的POST方法，并且
   form中有enctype="multipart/form-data"时，才有具体的数值，否则FILES为None。
  * HttpRequest.META: 包含了所有的HTTP headers. 包括CONTENT_LENGTH,CONTENT_TYPE, HTTP_ACCEPT_ENCODING, HTTP_ACCEPT_LANGUAGE , HTTP_HOST, HTTP_REFERER, HTTP_USER_AGENT, 
  HTTP_USER_AGENT, REMOTE_ADDR, REMOTE_HOST, REMOTE_USER, REQUEST_METHOD ,SERVER_NAME, SERVER_PORT。
  * HttpRequest.user: 是django.contrib.auth.model.User的object，反映的是当前的logged-in USER。当前用户没有登陆，会被设置成django.contrib.autg.models.AnonymousUser. 
  可以通过request.user.is_authenticated()来判断。user只有当AuthenticationMiddleware中间件被激活是才可用。关于user，参见[User authentication in Django](https://docs.djangoproject.com/en/1.4/topics/auth/)，Django的User是其精髓。
  * HttpRequest.session: readable , writeable, dick-like
  * HttpRequest.urlconf: 不是Django自身定义的，由中间件进行定义和激活。具体参见[How Django processes a request](https://docs.djangoproject.com/en/1.4/topics/http/urls/#how-django-processes-a-request)
### Methods:
  * HttpRequest.get_host()
  * HttpRequest.get_full_path(): 包括an appended query string.
  * HttpRequest.build_absolute_uri(location): 返回绝对地址包括domain.
  * HttpRequest.get_signed_cookie(key, default=RAISE_ERROR, salt='', max_age=None)
  * HttpRequest.is_secure(): HTTPS的判断
  * HttpRequest.is_ajax(): XMLHttpRequest
  * HttpRequest.read(size=None), HttpRequest.readline(), HttpRequest.readlines(), HttpRequest.readlines()
## UploadFile objects
### Attributes
  * UploadedFile.name 
  * UploadedFile.size
### Methods:
  * UploadedFile.chunks(chunk_size=None): 返回一个generator和yields来反映序列化的文件数据。
  * UploadedFile.read(num_bytes=None)： 从文件中读取bytes.

## QueryDict Objects
QueryDict是不可改变的，除非用copy。
### Methods:
QueryDict实现了多有的标准字典的方法，是subclass
  * QueryDict.__getitem__(key): Raises django.utils.datastructures.MultiValueDictKeyError
  * QueryDict.__setitem__(key, value): 只有copy后才能更改。
  * QueryDict.__contains__(key)
  * QueryDict.get(key, default): default为该值不存在时默认返回值。
  * QueryDict.setdefault(key, default)
  * QueryDict.update(other_dict)
  * QueryDict.urlencode([safe])

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

