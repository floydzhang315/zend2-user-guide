# 开始使用 Zend Framework 2

该教程的目的是介绍如何使用 Zend Framework 2，将会通过创建一个简单的数据库驱动且使用 MVC 模型的应用来说明。最终你将获得一个可运作的 ZF2 应用，你可以查看代码并去发掘它是如何工作和适配的。

## 一些假设

该教程假设你在 Apache Web 服务器上运行 PHP 5.3.23 以上版本和 MySQL 数据库，且能够通过 PDO 扩展访问。你的 Apache 必须安装且配置了 mod_rewrite 扩展。

必须确保你的 Apache 配置支持 `.htaccess` 文件。 通常改变如下设置来支持该功能：

```
AllowOverride None
```

改为：

```
AllowOverride FileInfo
```

在你的 `httpd.conf` 文件中，查看分布文档以获取精确的细节信息。如果你没有正确配置 mod_rewrite 和 .htaccess，在这个教程中你将不能导航到除主页外的任何页面。

注意：如果您使用的是 PHP 5.4+ 版本，你可以使用内置的 Apache Web 服务器来代替 Apache 部署。


## 教程中的应用

这个应用程序中，我们将构建一个简单的库存系统来显示我们自己的专辑。
主页将列出我们收集的并允许我们添加、编辑和删除 CDs。我们网站需要有4个页面：

| 页面 |	描述 |
|-----|------|
| 专辑列表 | 这个页面将展示专辑列表，并且提供链接用来编辑和修改它。同时，还需要提供一个添加新专辑的链接 |
| 添加新专辑	| 这个页面提供一个表单来添加新专辑 |
| 编辑专辑	| 这个页面提供一个表单来编辑专辑 |
| 删除专辑	| 这个页面将确认我们是否删除一个专辑然后才能删除 |

我们还需要将数据存储到数据库。这只需要一个表，字段如下：

|字段名	|类型|	空?	|注释|
|-----|------|------|------|
|id	|integer	|No|	Primary key, auto-increment|
|artist	|varchar(100)	|No	 |
|title	|varchar(100)|	No	 |
