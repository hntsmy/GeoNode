## document_detail

接口描述：资源详情展示页

HTTP请求方式：GET
| 参数名称 | 参数说明 | 长度要求 | 是否必须 |
| --- | --- | --- | --- |
| docid | 文件编号 |  | Y |

* 正常：跳转到文件详情页

* 异常:

如果文件为None，返回值：401 msg：发生未知错误

如果文件不为None，查询用户要求的文件类型异常，抛出异常：' '
如果用户不是文件上传者，也不是超级管理员，则修改数据库中id对应文件流行度，即文件热度+1

## views.py第90-100行看不懂

如果获取文件EXIF信息失败，抛出异常，提示“文件EXIF信息抽离失败”

* 错误：

\_resolve_document()：返回404页面
    
验证权限拒绝：
   返回值： 状态码：403 url：401页面 err_mes：没有查看该文件的权限

## document_download

接口描述：文档下载

HTTP请求方式：GET

| 参数名称 | 参数说明 | 长度要求 | 是否必须 |
| --- | --- | --- | --- |
| docid | 文件编号 |  | Y |

正常：跳转到下载页面

错误：

状态码：401  url:401页面  err_mes:没有查看该文件的权限

## 第227行class DocumentUpdateView(updateView)没看明白


## document_metadata没看明白

接口描述：


## document_search_page

接口描述：从站内根据关键字搜索文件信息并返回结果

HTTP请求方式：GET/POST

| 参数名称 | 参数说明 | 长度要求 | 是否必须 |
| --- | --- | --- | --- |
| limit | 每页显示数量（默认100） |  | N |
| offset | 偏移量（查询起始值，默认0） |  | N |
| title__icontains | 每页显示数量 |  | N |

如果请求方法为GET，则返回参数request.get,如果请求方式为POST，则返回参数为request.post，否则返回状态码：405

返回页面：documents/document_search.html


## /search/

描述：通过填写关键字，实现用户站内搜索功能。

HTTP请求方式：GET

链接示例：/search/?title__icontains=&limit=100&offset=0&category__identifier__in=imageryBaseMapsEarthCover&type__in=document&owner__username__in=geonode&date__range=2017-01-11,2017-01-18&regions__name__in=Central%20Africa&.extent=-47.8125,30.44867367928756,44.6484375,79.6240562918881

| 参数名称 | 参数说明 | 长度要求 | 是否必须 |
| --- | --- | --- | --- |
| title__icontains | 查询关键字 |   | N |
| limit | 每页显示数量（默认100） |  | N |
| offset | 偏移量（查询起始值，默认0） |  | N |
| category__identifier__in | 搜索类别 |   | N |
| type__in | 文件类型 |  | N |
| owner__username__in | 所有者 |  | N |
| start\_date | 数据开始日期 |   | N |
| end\_date | 数据终止日期 |   | N |
| regions__name__in | 地区 |  | N |
| extent | 延伸（经纬度） |  | N |

## document_remove

HTTP请求方式：GET/POST

如果请求格式为GET，则返回文件信息

## geonode.contrib.slack.utils 看不明白

如果请求格式为POST，则返回文件信息

build_slack_message_document，返回不能为删除文件创建 slack message

send_slack_messages，返回不能为删除的文件发送slack message

否则，返回状态码：403   err_msg：禁止操作

如果权限拒绝，则返回状态码401 err_msg:不允许删除文件

正常：执行delete() 方法，重定向到list页面。