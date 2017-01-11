# Geonode开发入门

## Geonode开发技术介绍
### 概念和背景介绍
* OGC Services
* Web Application Architecture
* AJAX and REST
* OpenGeo Suite
* GeoServer
* PostgreSQL and PostGIS Administration

### 核心开发库与开发工具
* Python
* Django
* Javasript
* jQuery
* Bootstrap
* GeoTools/GeoScript/GeoServer
* geopython
* GDAL/OGR

## Django概览与介绍
* model
* views
* Templates
* Forms
* GET and POST
* 用户认证
* Admin管理
* 国际化

根据用户特征不同可以将网页信息本地化


    from django.utils.translation import ugettext
    from django.utils.translation import ugettext
    def homepage(request):
        """
        Shows the homepage with a welcome message that is translated in the user's language.
        """
        message = ugettext('Welcome to our site!')
        return render(request, 'homepage.html', {'message': message})`

```html{% load i18n %}
<html>
  <head>
    <title>{% trans 'Homepage - Hall of Fame' %}</title>
  </head>
  <body>
    {# Translated in the view: #}
    <h1>{{ message }}</h1>
    <p>
      {% blocktrans count member_count=bands.count %}
      Here is the only band in the hall of fame:
      {% plural %}
      Here are all the {{ member_count }} bands in the hall of fame:
      {% endblocktrans %}
    </p>
    <ul>
    {% for band in bands %}
      <li>
        <h2><a href="{{ band.get_absolute_url }}">{{ band.name }}</a></h2>
        {% if band.can_rock %}<p>{% trans 'This band can rock!' %}</p>{% endif %}
      </li>
    {% endfor %}
    </ul>
  </body>
</html>
```
更多参考文档：
* [Transifex](https://www.transifex.com/)
* [Django国际化和本地化](https://docs.djangoproject.com/en/1.8/topics/i18n/)

### 安全
* 点击劫持
* XSS
* CSRF
* SQL注入攻击
* Host头攻击
* SSL/HTTPS

## Geonode开发必备
### 基本的shell工具
* ssh and sudo
* bash
* apt
### Python开发工具
* 虚拟环境
* pip
* 其他

    * ipython

    * pdb

### Django
* MVT
* HTTP请求响应
* [Django-admin管理命令说明](https://docs.djangoproject.com/en/dev/ref/django-admin/)

* [Django Admin 界面](https://docs.djangoproject.com/en/dev/ref/contrib/admin/)
* [模板标签](https://docs.djangoproject.com/en/dev/ref/templates/builtins/)

## GeoNode核心模块
* [geonode.layers图层模块](https://github.com/GeoNode/geonode/tree/master/geonode/layers)
* [geonode.maps地图模块](https://github.com/GeoNode/geonode/tree/master/geonode/maps)
* geonode.security 安全模块
* geonode.search 搜索模块
* geonode.catalogue 目录模块
* geonode.geoserver 服务器模块
* geonode.people 人模块
* GeoExplorer

GeoNode的核心GIS客户端功能基于geoexplorer执行，而GeoExplorer又基于GeoExt, OpenLayers and ExtJS。

* Static Site 静态网站
* Templates 模板
* CSS
* JavaScript

## Geonode开发环境搭建

参考文档：[Install GeoNode for Development](http://docs.geonode.org/en/latest/tutorials/devel/devel_env/index.html)

步骤如下：
1. 恢复最新的apt-get列表
2. 安装开发工具和开发库
3. 安装环境依赖于支持工具（Python，Postgresql，Java）
4. 为静态开发添加Node.jsPPA和其他需要的工具
5. 创建虚拟环境
6. 从Github上克隆Geonode并安装在虚拟环境上
7. 运行paver安装Geoserver并启动开发环境
8. 编译并开启服务
9. 开启Geonode实例
10. 终止服务
11. 为Geonode创建Django超级用户

## 再次在Geonode上继续工作
1. 激活虚拟环境

`$ workon geonode`

or

`$ source /home/geonode/dev/.venvs/geonode/bin/activate`

*注意路径可能不同*
2. 开启服务(每次重启机器后都要执行这步)

**如果GeoNode服务是可用的，则需要手动关闭之后再开启服务**

`service apache2 stop`

`service tomcat7 stop`

`$ cd geonode`

`$ paver start_geoserver`

`$ paver start_django`

现在，再次打开http://localhost:80000验证是否可用

注：通过以上步骤只是使用GeoNode的默认设置，如果你想个性化设置，例如用Tomcat代替Jetty，或者用Postgresql代替sqlite3，请参阅以下说明文档：

[Custom Installation Guide.](http://docs.geonode.org/en/latest/tutorials/admin/install/custom_install.html#custom-install)

### GeoNode debug技巧
* 浏览器内Debug

    * Net Ta
    * DOM Tab
    * Script Tab
    * HTML Tab
* GeoExplorer Debug
* Python模块Debug
    * logging
    * PDB
* Geoserver Debug
    * Logging
    * 高级Troubleshooting
    * 使用Django Debug

### Geonode API接口

### GeoNode 测试

#### 单元测试

#### 整体测试

#### Javascript测试

### Pavement \.py and Paver介绍

### GeoNode Projects介绍

详见官网[Gallery](http://geonode.org/gallery)部分

* [Harvard Worldmap](https://github.com/cga-harvard/cga-worldmap)

* [MapStory](https://github.com/MapStory/mapstory)

* [MetroBoston DataCommon](http://metroboston.datacommon.org/)

* [WFP GeoNode](http://geonode.wfp.org/)

* [ADRIPLAN](https://github.com/CNR-ISMAR/adriplan)

* [ROGUE GeoNode](https://github.com/ROGUE-JCTD/geonode)

## 发布Geonode

### 创建并导入GPG密钥

### 准备环境

### 发布

