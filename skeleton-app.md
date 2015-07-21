# 开始构建一个框架应用程序

为了构建我们的应用程序，我们先从 [GitHub](https://github.com/) 获取 [ZendSkeletonApplication](https://github.com/zendframework/ZendSkeletonApplication) 源码。使用 Composer ([http://getcomposer.org](http://getcomposer.org/)) 从头开始创建一个依赖 Zend Framework 的新项目：

```
php composer.phar create-project --stability="dev" zendframework/skeleton-application path/to/install

```

**注意**

另外一个下载 ZendSkeletonApplication 的方法是使用 GitHub。进入 [https://github.com/zendframework/ZendSkeletonApplication](https://github.com/zendframework/ZendSkeletonApplication) 然后点击 “Zip” 这个按钮。这将会下载一个文件名类似于 `ZendSkeletonApplication-master.zip` 或其他相似的文件。 

将这个文件解压到你保持所有虚拟主机的地方，并将生成的目录重命名为 `zf2-tutorial`。

ZendSkeletonApplication 设置使用 Composer 来解决它的依赖。这种情况下，这个依赖就是 Zend Framework 2 它本身。

下载 Zend Framework 2 到我们的应用，只要简单的输入：

```
php composer.phar self-update
php composer.phar install
```

在 `zf2-tutorial` 文件夹中，这需要一些时间。你应该可以看到一个像这样的输出：

```
Installing dependencies from lock file
- Installing zendframework/zendframework (dev-master)
Cloning 18c8e223f070deb07c17543ed938b54542aa0ed8

Generating autoload files
 
```

**注意**

如果你看到这样的消息：

```
[RuntimeException]
  The process timed out.
```

then your connection was too slow to download the entire package in time, and composer timed out. To avoid this, instead of running:

这是说明连接太慢导致无法及时将全部包下载下来，然后 Composer 超时。要避免这个问题，替换一下运行命令：

```
php composer.phar install
```

用下面这个代替：

```
COMPOSER_PROCESS_TIMEOUT=5000 php composer.phar install
```

**注意**

使用 Wamp 的 Windows 用户：

1. 给 Windows 安装 Composer。检查 Composer 是否正确安装，运行：

	```
	composer
	``` 
2. 给 Windows 安装 Git。同时需要添加 Git 的路径到 Windows 的环境配置中。检查 Git 是否正确安装，运行：

	```
	git
	```
3. 现在安装 zf2 使用命令：

	```
	composer create-project -s dev zendframework/skeleton-application path/to/install
   ```
   
我们现在可以移步到 Web 服务器设置步骤。


## 使用 Apache Web Server

你现在需要给应用程序建立一个 Apache 虚拟主机并且编辑你的 host 文件，以至于让 `http://zf2-tutorial.localhost ` 指定到 `zf2-tutorial/public ` 目录为 `index.php` 提供服务。

设置虚拟主机通常是在 `httpd.conf` 或 `extra/httpd-vhosts.conf` 中。如果你使用的是 `extra/httpd-vhosts.conf`，需要确保这个文件包含在你的 `httpd.conf` 主文件中。一些 Linux 的发行版本（例如 Ubuntu）会包裹 Apache 文件夹，以至于配置文件会存储在 `/etc/apache2` 中，并且它会在 `/etc/apache2/sites-enabled` 目录下为每个虚拟主机创建一个文件。这种情况下，你应该把虚拟主机（下面那一块）放到 `/etc/apache2/sites-enabled/zf2-tutorial `文件中。

确保已经定义 `NameVirtualHost`，并且设置成类似于 “*:80”，然后按照这些原则定义一个虚拟主机：

```
<VirtualHost *:80>
     ServerName zf2-tutorial.localhost
     DocumentRoot /path/to/zf2-tutorial/public
     SetEnv APPLICATION_ENV "development"
     <Directory /path/to/zf2-tutorial/public>
         DirectoryIndex index.php
         AllowOverride All
         Order allow,deny
         Allow from all
     </Directory>
</VirtualHost>
``` 

确保你已经更新了 `/etc/hosts` 或者 `c:\windows\system32\drivers\etc\hosts` 文件以至于让 `zf2-tutorial.localhost` 映射到 `127.0.0.1 `。这样之后网站就可以通过 `http://zf2-tutorial.localhost` 访问了。

```
127.0.0.1            zf2-tutorial.localhost localhost
```

重启 Apache。

如果你正确的完成以上步骤，你将看到类似于这样的：

![](images/user-guide.skeleton-application.hello-world.png)

测试你的 `.htaccess ` 文件是有效的，导航到 `http://zf2-tutorial.localhost/1234` 然后你应该会看到：

![](images/user-guide.skeleton-application.404.png)

如果你看到一个标准的 Apache 404 错误，继续下一步前你应该修复 `.htaccess` 的用法。如果你使用的是 IIS 的 URL Rewrite Module，导入下面的内容：

```
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [NC,L]
```

现在你拥有一个正常工作的框架应用程序，我们可以开始给我们的应用添加一些细节内容。

## 使用 Built-in PHP CLI Server

或许－如果你使用的是 PHP 5.4 或以上版本－你可以使用 Built-in PHP CLI Server（cli-server）。使用这个，你只需要在根目录开启这个服务：

```
php -S 0.0.0.0:8080 -t public/ public/index.php
```

这将让网站在所有网络接口的 8080 端口有效的，使用 `public/index.php` 处理路由。这就意味着通过 [http://localhost:8080](http://localhost:8080/) 或者 http://<your-local-IP>:8080` 都可以访问到你的网站。

如果你正确完成以上步骤，你可以看到和上面 Apahce 显得的同样的结果。

测试你的路由是有效的，导航到 [http://localhost:8080/1234](http://localhost:8080/1234)，你可以看到和上面 Apahce 显得的同样的错误页面。

**注意**

内置的 CLI server 仅仅只是用于**部署**。

## 错误报告

当你使用 Apache 的时候，你可以随意的使用 `VirtualHost ` 里面的 `APPLICATION_ENV ` 设置让 PHP 输出所有浏览器上的错误。在开发应用的时候使用这个将会有很大用处。


编辑 `zf2-tutorial/public/` 目录下的 `index.php`，用下面的内容更改它：

```
<?php

 /**
  * Display all errors when APPLICATION_ENV is development.
  */
 if ($_SERVER['APPLICATION_ENV'] == 'development') {
     error_reporting(E_ALL);
     ini_set("display_errors", 1);
 }

 /**
  * This makes our life easier when dealing with paths. Everything is relative
  * to the application root now.
  */
 chdir(dirname(__DIR__));

 // Decline static file requests back to the PHP built-in webserver
 if (php_sapi_name() === 'cli-server' && is_file(__DIR__ . parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH))) {
     return false;
 }

 // Setup autoloading
 require 'init_autoloader.php';

 // Run the application!
 Zend\Mvc\Application::init(require 'config/application.config.php')->run();
```
