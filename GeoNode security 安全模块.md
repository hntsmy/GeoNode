# 概述
安全模块主要定义资源的安全

GeoNode权限管理主要针对用户和文件两部分构成，具体如下图所示：

![GeoNode权限控制](http://oh6j8wijn.bkt.clouddn.com/05C98E89-D2AA-4481-AC34-36725ADFC16D.png)

# resource_permissions

描述：资源权限控制

http请求方式：GET/POST

参数说明：

| 参数名称 | 参数说明 | 长度要求 | 是否必须 |
| --- | --- | --- | --- | --- |
| resource_id | 资源id编号 |   | Y |

正常：

如果请求方式为POST：

将请求体转为Python格式，然后调用`set_permissions()`方法，返回状态码：200

如果请求方式为GET：

调用_perms_info_json()方法，`_perms_info_json()`方法用于获取文件所属创作者及群组信息

返回状态码：200

返回内容：json格式

异常：

状态码：401

响应信息：不允许POST与GET之外的请求方式

# set_bulk_permissions

描述：对资源进行权限扩展

http请求方式：POST

正常：

根据资源id获取标题并添加到不允许列表；（此处逻辑没有看懂）

异常：

状态码：400

err_msg：错误的权限说明

# request_permissions

描述：向文件所有者请求下载资源的权限，如果获得uuid，则程序继续执行，否则执行Django的`get_object_or_404()`方法直接跳转到404页面。

http请求方式：POST

正常：调用notification的`send()`方法，向资源所有者发送通知

返回参数：

状态码：200  

异常：返回错误提示

状态码：400

err_msg：通知递送失败
