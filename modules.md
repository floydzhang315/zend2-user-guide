# 模块

Zend Framework 2 使用一个模块系统来组织每个模块内主要的、明确的代码。框架提供的运用程序模块习惯用于提供整个程序的引导、错误和 route 配置。应用程序模块通常是用于提供应用水平的控制器，比如说，应用程序的主页，在生成我们自己的模块式时，想要 album 列表成为主页面，但是在此向导中不会提供如何设置默认主页。

我们要把所有的代码放到包含控制器、模型、表单和视图，以及配置的 Album 模块当中。我们也要根据需要调整应用程序模块。

从所需要的目录开始吧。

## 设置 Album 模块

首先在 `module` 里创建一个名为 `Album` 的目录，用下面的子目录下的目录来保存模块的文件:

```
zf2-tutorial/
     /module
         /Album
             /config
             /src
                 /Album
                     /Controller
                     /Form
                     /Model
             /view
                 /album
                     /album
```

正如你所看到的那样这个 `Album` 模块当中有不同类型文件的独立目录。包含 `Album` 名称空间的 PHP 文件在 `src/Album` 目录里面，这样在我们的模块我们可以有多个命名名称空间，这是我们应该需要的。视图目录也有叫做 `album` 的子文件夹保存我们的模块视图脚本。

为了安装和配置模块，Zend Framework 2 提供一个 `ModuleManager`。这将在模的根目录（`module/Album`）寻找Module.php文件，可以找到一个叫（`module/Album`）的类。也就是说，给定的类模块有模块名称的命名空间，也就是这个模块的目录名称。

在专辑模块创建 `Module.php` 文件：在 `zf2-tutorial/module/Album` 创建一个叫  `Module.php` 的文件：

```php
namespace Album;

 use Zend\ModuleManager\Feature\AutoloaderProviderInterface;
 use Zend\ModuleManager\Feature\ConfigProviderInterface;

 class Module implements AutoloaderProviderInterface, ConfigProviderInterface
 {
     public function getAutoloaderConfig()
     {
         return array(
             'Zend\Loader\ClassMapAutoloader' => array(
                 __DIR__ . '/autoload_classmap.php',
             ),
             'Zend\Loader\StandardAutoloader' => array(
                 'namespaces' => array(
                     __NAMESPACE__ => __DIR__ . '/src/' . __NAMESPACE__,
                 ),
             ),
         );
     }

     public function getConfig()
     {
         return include __DIR__ . '/config/module.config.php';
     }
 }
```

`ModuleManager` 将自动调用 `getAutoloaderConfig()` 和 `getConfig()` 函数来帮助我们。

### 半自动文件

我们的 `getAutoloaderConfig()`  函数返回一个数组，兼容 ZF2 的 `AutoloaderFactory`。对它进行配置，添加一个类映射文件给 `ClassMapAutoloader` 和添加此模块的名称给 `StandardAutoloader`。标准自动装卸机需要一个名称空间和在哪里可以找到文件的路径名称空间。与 PSR-0 相兼容，此类按照 [PSR-0 规则](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md) 直接映射到文件。

随着我们开发继续进行，我们不需要通过类映射加载文件,所以我们提供一个空数组给类映射自动装弹机。在 `zf2-tutorial/module/Album` 创建一个叫 `autoload_classmap.php` 的文件：

```php
 return array();
```

因为这是一个空数组,每当自动装卸机寻找 `Album` 名称空间中的一个类,它将为我们回落到 `StandardAutoloader`。

> **注意**

> 如果你使用的是 Composer，你可以只创建一个空的 getAutoloaderConfig(){ } 并添加到 composer.json：

> ```php
     "autoload": {
         "psr-0": { "Album": "module/Album/src/" }
     },
```

> 如果你这样做，那么你需要运行 `php composer.phar update` 去更新 composer 半自动文件。

## 配置

已经注册好自动装卸机，让我们来快速地浏览 `Album\Module` 里面 `getConfig()` 方法。这个方法简单地加载了 `config/module.config.php` 文件。

在 `zf2-tutorial/module/Album/config` 创建一个叫 `module.config.php` 的文件：

```php
return array(
     'controllers' => array(
         'invokables' => array(
             'Album\Controller\Album' => 'Album\Controller\AlbumController',
         ),
     ),
     'view_manager' => array(
         'template_path_stack' => array(
             'album' => __DIR__ . '/../view',
         ),
     ),
 );
```

配置信息通过 `ServiceManager` 传递给相关的组件。我们需要两个初始部分：控制器和 `view_manager`。控制器部分提供了所有由模块提供的控制器列表。我们需要一个控制器 `AlbumController`，引用作为 `Album\Controller\Album`。所有控制器的键必须是唯一的，所以用模块名称作为前缀。

在 `view_manager` 部分，将视图目录添加到 `TemplatePathStack` 配置中。这样它就可以找到存储在 `view/` 目录下的 `Album` 模块的视图脚本。

## 通知关于我们新模块应用程序

我们现在需要告诉 `ModuleManager` 这个新模块的的存在。这个在基本框架应用程序提供的配置 `config/application.config.php` 文件中完成。更新这个文件，以便他的模块部分包含 `Album` 模块。因此，文件现在看起来像这样：

（变化需要使用注释突显出来：）

```php
return array(
     'modules' => array(
         'Application',
         'Album',                  // <-- Add this line
     ),
     'module_listener_options' => array(
         'config_glob_paths'    => array(
             'config/autoload/{{,*.}global,{,*.}local}.php',
         ),
         'module_paths' => array(
             './module',
             './vendor',
         ),
     ),
 );
```

如你所见，在应用程序模块之后，我们已经添加 `Album` 模块到模块列表当中。

现在设置好模块，准备把自定义代码放到里面。
