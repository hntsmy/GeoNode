# 概述及定义

## 概述

国际化和本地化的目的就是让一个网站应用能做到根据用户语种和指定格式的不同而提供不同的显示内容。而Django内部对文本翻译,日期、时间和数字的格式化以及时区提供了完善的支持。由于Geonode底层使用Django实现，所以我们通过设置Django本地化即可实现。

实际上，Django做了两件事：

由开发者和模板作者指定应用的哪些部分应该翻译，或是根据本地语种和文化进行相应的格式化。
根据用户的偏好设置，使用钩子将web应用本地化。
很显然，翻译取决于用户所选语言，而格式化通常取决于用户所在国家。 这些信息由浏览器通过Accept-Language 协议头提供。不过确定时区就不是这么简单了。

以下内容摘自官方文档[翻译-Django 如何发现语言偏好](https://docs.djangoproject.com/en/1.10/topics/i18n/translation/#how-django-discovers-language-preference)一节。

>Once you’ve prepared your translations – or, if you just want to use the translations that come with Django – you’ll just need to activate translation for your app.

>Behind the scenes, Django has a very flexible model of deciding which language should be used – installation-wide, for a particular user, or both.

>To set an installation-wide language preference, set LANGUAGE_CODE. Django uses this language as the default translation – the final attempt if no better matching translation is found through one of the methods employed by the locale middleware (see below).

>**If all you want is to run Django with your native language all you need to do is set LANGUAGE_CODE and make sure the corresponding message files and their compiled versions (.mo) exist.**

如果你想让Django使用你的母语运行，你只需要设置相应的`LANGUAGE_CODE`并确认在相应的`message files`中存在编译过的文件（.mo）。 

>If you want to let each individual user specify which language they prefer, then you also need to use the LocaleMiddleware. LocaleMiddleware enables language selection based on data from the request. It customizes content for each user.

## 定义

* 国际化(Internationlization)

让软件支持本地化的准备工作，开发者对要进行翻译的字符串进行标记，由翻译者进行翻译，当用户访问该 Web 时，Django 内部框架根据用户使用偏好进行 Web 呈现。

**设置`USE_I18N=True`开启django的国际化,如果不需要国际化则设置为False。**

国际化需要三步：

1. 在 Python 代码和模板中嵌入待翻译的字符串

2. 把那些字符串翻译成需要支持的语言，并进行相应的编译

3. 在 Django settings 文件中激活本地中间件

* 本地化(Localization)
指使一个国际化的程序为了在某个特定地区使用而由翻译者进行实际翻译的过程。

**格式化系统默认是禁用的，需要在设置文件中设置`USE_L10N = True`启用。**

注意：为了方便，`django-admin startproject` 创建的settings.py 文件中，`USE_L10N = True`。但是要注意，要开启千位分隔符的数字格式化，你需要在你的设置文件中设置`USE_THOUSAND_SEPARATOR = True`。或者，你也可以在你的模板中使用intcomma来格式化数字。


1. 在 Python 代码和模板中嵌入待翻译的字符串;

针对Python代码：

使用ugettext进行标记。标记时需确定自己已经安装了gettext组件。

linux下安装方式为：`apt-get install gettext`

在python文件中可以import gettext然后就可以用gettext对字符串进行标记。

```python
from django.utils.translation import ugettext as _  
def my_view(request):  
    output = _("Welcome to my site.")  
    return HttpResponse(output) 
```
即：对需要翻译的代码内容前加下划线。

### 4个主要函数的用途

其中：

`django.utils.translation.ugettext()`

指定一个翻译字符串，一般都用于 views.py

`django.utils.translation.gettext_noop()`

标记一个不需要立即翻译的字符串。 这个字符串会稍后从变量翻译。使用这种方法的环境是，有字符串必须以原始语言的形式存储（如储存在数据库中的字符串）而在最后需要被 翻译出来（如显示给用户时）。

`django.utils.translation.gettext_lazy()`

ugettext_lazy() 将字符串作为惰性参照存储，而不是实际翻译 , 一般会用于 models.py。 翻译工作将在字符串在字符串上下文中被用到时进行，比如在 Django 管理页面提交模板时。在 Django 模型中总是无一例外的使用惰性翻译。

`django.utils.translation.ungettext()`

函数包括三个参数： 单数形式的翻译字符串，复数形式的翻译字符串，和对象的个数（将以 count 变量传递给需要翻译的语言）。

针对模板：

**注意**：在模板中进行标记，必须在模板开头写：`{%load i18n%}`，否则报错。

用`{%trans%}`来对要翻译的字符串进行标记，当字符串中含有变量时，用`{%blocktrans%}`和`{%endblocktrans%}`来进行标记。

然后这个字符串就会被翻译为我们需要的语言, 翻译结果还是一个普通的字符串,没什么区别。

在这里,_("Welcome to my site.") 已经不是原来的字符串了,函数返回了一个Trans()类对象,详见django.utils.translation模块源代码。

不含变量：

```html
<title>{% trans "This is the title." %}</title>
```

含有变量：

```html
{% blocktrans with value|filter as myvar %}  
This will have {{ myvar }} inside.  
{% endblocktrans %}  
```

2. 把那些字符串翻译成需要支持的语言，并进行相应的编译;

如果之前从未做过国际化的工作，则需要在工程文件的同级目录下创建文件夹conf/locale，语言文件会按照语言分别保存在该文件夹下面。然后执行

```
django-admin.py  makemessages -l zh_CN #zh-CN 指创建文件中的翻译文件
```

命令执行位置：

* Django项目根目录。

* 您Django应用的根目录。

* django 根目录（不是Subversion检出目录，而是通过 $PYTHONPATH 链接或位于该路径的某处）。 这仅和你为Django自己创建一个翻译时有关。

会生成如下的一个文件目录

```
locale/
├── zh_CN
│   └── LC_MESSAGES
│       ├── django.mo
│       └── django.po
```

zh_CN指简体中文,在django中，每个语言都有自己的LANGUAGE_CODE


若要创建所有语言的语言文件，需运行命令（针对某一地区，不需要用该命令）：

```
django-admin.py makemessages -a  #创建所有的翻译文件

```

这时候打开django.po, 内容格式如下：

```
#: views.py:169

msgid "Welcome to my site."

msgstr ""

#: templates/login.html: 15

msgid "This is the title"

msgstr ""

```

翻译人员在msgstr填上我们的翻译结果即可。

翻译完成之后运行：

```
django-admin.py  compilemessages
```

编译好的django.mo是二进制文件,python的标准库gettext可以读取并进行翻译，这样最终出现的页面就是我们要的中文了。

**注意**

如果我们在代码或模板中增加或删除了相关的国际化代码,那么需要重新运行`makemessages` 和 `compilemessages`。

如果只是改了django.po中的翻译,只需重新 compile 就行了.

3. 在 Django settings 文件中激活本地中间件

以上工作准备完毕之后，国际化的翻译文件才算准备好了，在配置文件中指定LANGUAGE_CODE，Django将使用这个语言作为翻译语言。

```python
LANGUAGE_CODE = 'zh-CN' #此处高版本Django官方推荐设置为zh-Hans
 USE_I18N = True 
 MIDDLEWARE_CLASSES = ( 
    'django.middleware.common.CommonMiddleware', 
    'django.contrib.sessions.middleware.SessionMiddleware', 
    'django.middleware.locale.LocaleMiddleware', 
    'django.contrib.auth.middleware.AuthenticationMiddleware', 
 )
```

另外，如果想让用户各自指定语言偏好，就需要使用`LocaleMiddleware`在中间件中添加`django.middleware.locale.LocaleMiddleware`,注意中间件的顺序。请注意 `MIDDLEWARE_CLASSES` 中的`'django.middleware.locale.LocaleMiddleware'`, 需要放在`'django.contrib.sessions.middleware.SessionMiddleware'` 后面。


LocaleMiddleware会按照下面的算法顺序确定用户的语言：

首先，在当前用户的session中查找django_language键；

如未找到，他会找寻一个cookie；

还找不到的话，他会在HTTP请求头部里查找Accept-Language，该头部是浏览器发送的，并且按优先顺序告诉服务器你的语言偏好。Django会尝试头部中的每一个语种直到它发现一个可用的翻译。

以上都失败了的话，就使用全局的LANGUAGE_CODE设定值。

总结，对于静态翻译（无中间件）而言，语言在settings.LANGUAGE_CODE中，而对于动态翻译（中间件），它在request.LANGUAGE_CODE中。

重启服务

完成以上步骤之后,需要重启一下Django服务。

### 其他说明

和国际化有关的setting

1. USE_I18N = True/False

有关的代码是这样的，在django.utils.translation的__init__.py中

```python
class Trans(object);
      def __getattr__(self, real_name):
          from django.conf import settings
          if settings.USE_I18N:
              from django.utils.translation import trans_real as trans
          else:
              from django.utils.translation import trans_null as trans
          setattr(self, real_name, getattr(trans, real_name))
          return getattr(trans, real_name)
```

而trans_null其实就没做什么事 

2,LANGUAGE_CODE

比如中文是
`LANGUAGE_CODE = 'zh_cn'`

.po 文件中的 fuzzy str translation

有时候, 用makemessages生成的.po文件中有些msgid会被标记为 fuzzy

比如

```
#: models.py:35
#, fuzzy
msgid "hdapp_leader"
msgstr "领队"
```

这是因为对于 "hdapp_leader",  msgmerge工具认为这个和之前的一个msgid很相似,这个翻译可能不靠谱,于是标记fuzzy； msgfmt就会把这个msgid给略过,也就是这个翻译不会生效,当然如果我们确认是对的, 就手动删掉那行fuzzy,重新compilemessages就好.

### 参考来源：

* [Django 国际化实例及原理分析-developerWorks 中国](http://www.ibm.com/developerworks/cn/web/1101_jinjh_djangoi18n/)

* [国际化和本地化-Django官方文档](https://docs.djangoproject.com/en/1.8/topics/i18n/)

* [国际化和本地化-Django中文文档](http://python.usyiyi.cn/django/topics/i18n/index.html)

* [Django国际化和本地化](http://blog.csdn.net/lion19930924/article/details/51488398)

* [Django国际化和本地化-真实的活](http://www.cnblogs.com/livingintruth/p/3568557.html)

* [django国际化和本地化-简书](http://www.jianshu.com/p/dd9fea652018)

* [深入学习Django源码基础8 - Django中系统级国际化本地化](http://blog.csdn.net/watsy/article/details/10994589)

* [针对Mac用户的gettext操作](http://blog.csdn.net/watsy/article/details/10990633)
