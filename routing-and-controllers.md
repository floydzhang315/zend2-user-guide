# Routing and controllers 

路由和控制器

我们将构建一个非常简单的库存系统来展示我们的 album 集合。主页列举了我们的集合以及允许添加，修改和删除 album 。因此需要如下页面：

We will build a very simple inventory system to display our album collection. The home page will list our collection and allow us to add, edit and delete albums. Hence the following pages are required:

| 页面 | 描述 |
|--|--|
| Home | 显示 album 的列表,并提供链接来编辑和删除 album，也可以添加新 album。 |
| Add new album | 这个页面提供表单来添加新 album。 |
| Edit album | 这个页面提供表单来编辑 album。 |
| Delete album | 这个页面将会确认我们是否要删除 album，然后删除。 |

| Page | Description |
|--|--|
| Home | This will display the list of albums and provide links to edit and delete them. Also, a link to enable adding new albums will be provided. |
| Add new album | This page will provide a form for adding a new album. |
| Edit album | This page will provide a form for editing an album. |
| Delete album | This page will confirm that we want to delete an album and then delete it. |

在构建文件之前，最重要的是要理解控件组织预定的页面的。应用程序的每个页面被称为一个动作，动作被分组划分给控制器模块。因此，你通常需要将相关联的动作划分到同一个控制器。例如，一个新闻控制器或许有如下动作：`current`，`archived` 和 `view`。

Before we set up our files, it’s important to understand how the framework expects the pages to be organised. Each page of the application is known as an action and actions are grouped into controllers within modules. Hence, you would generally group related actions into a controller; for instance, a news controller might have actions of `current`, `archived` and `view`.

申请四个页面给 albums，将这些页面分组划分给单个控制器 `AlbumController` ，在 Album 模块中作为动作。这四个动作如下：

As we have four pages that all apply to albums, we will group them in a single controller `AlbumController` within our Album module as four actions. The four actions will be:

| Page | Controller | Action |
|--|--|--|
| Home | `AlbumController` | `index` |
| Add new album | `AlbumController` | `add` |
| Edit album | `AlbumController` | `edit` |
| Delete album | `AlbumController` | `delete` |

| Page | Controller | Action |
|--|--|--|
| Home | `AlbumController` | `index` |
| Add new album | `AlbumController` | `add` |
| Edit album | `AlbumController` | `edit` |
| Delete album | `AlbumController` | `delete` |

URL 指定了一个特定的动作来使用路由，这个路由在 `module.config.php` 模块定义。我们将添加一个路由到我们的 album 动作中去。更新模块的配置文件与新代码会高亮显示。

The mapping of a URL to a particular action is done using routes that are defined in the module’s `module.config.php` file. We will add a route for our album actions. This is the updated module config file with the new code highlighted.

```php
 return array(
     'controllers' => array(
         'invokables' => array(
             'Album\Controller\Album' => 'Album\Controller\AlbumController',
         ),
     ),

     // The following section is new and should be added to your file
     'router' => array(
         'routes' => array(
             'album' => array(
                 'type'    => 'segment',
                 'options' => array(
                     'route'    => '/album[/:action][/:id]',
                     'constraints' => array(
                         'action' => '[a-zA-Z][a-zA-Z0-9_-]*',
                         'id'     => '[0-9]+',
                     ),
                     'defaults' => array(
                         'controller' => 'Album\Controller\Album',
                         'action'     => 'index',
                     ),
                 ),
             ),
         ),
     ),

     'view_manager' => array(
         'template_path_stack' => array(
             'album' => __DIR__ . '/../view',
         ),
     ),
 );
```

route 的名字是 ‘album’，其类型是 ‘segment’。segment route 允许我们指定 URL 模式中的占位符 (route)，将其映射到命名参数匹配的 route。名字为 `/album[/:action][/:id]` 路由，将配备所有由 `/album` 开头的 URL 。下一个 segment 是可选的动作名，最后一个字段将会被映射到可选id。方括号括起来参数的是可选的，这些约束可以让我们明确知道在字段中字符是可预料的，所以动作的第一个单词被限制为字母，随后的字符可以是数字、字母、下划线和连字符。我们规定 id 必须是数字。

The name of the route is ‘album’ and has a type of ‘segment’. The segment route allows us to specify placeholders in the URL pattern (route) that will be mapped to named parameters in the matched route. In this case, the route is ``/album[/:action][/:id]`` which will match any URL that starts with `/album`. The next segment will be an optional action name, and then finally the next segment will be mapped to an optional id. The square brackets indicate that a segment is optional. The constraints section allows us to ensure that the characters within a segment are as expected, so we have limited actions to starting with a letter and then subsequent characters only being alphanumeric, underscore or hyphen. We also limit the id to a number.

route 允许我们可以有如下的 URL 格式：

This route allows us to have the following URLs:

| URL | Page | Action |
|--|--|--|
| `/album` | 主页 (alumn 列表) | `index` |
| `/album/add` | 添加新的 album | `add` |
| `/album/edit/2` | 编辑 id 为 2 的 album | `edit` |
| `/album/delete/4` | 删除 id 为 4 的 album | `delete` |

| URL | Page | Action |
|--|--|--|
| `/album` | Home (list of albums) | `index` |
| `/album/add` | Add new album | `add` |
| `/album/edit/2` | Edit album with an id of 2 | `edit` |
| `/album/delete/4` | Delete album with an id of 4 | `delete` |

## Create the controller

创建控制器

现在已经准备好构建控制器了。在 Zend Framework 2 中，控制器是一个类，所谓的 `{Controller name}Controller`。注意， `{Controller name}` 的命名必须是大写字母开头。控制器的定义必须存放在模块 `Controller` 文件夹下的，名字格式为 `{Controller name}Controller.php` 的文件中。在本例中，模块文件夹是 `module/Album/src/Album/Controller`。每一个动作是在控制器类中一个公有的方法，其命名格式是 `{action name}Action`。在本例中，`{action name}` 应该以小写字母开头。

We are now ready to set up our controller. In Zend Framework 2, the controller is a class that is generally called `{Controller name}Controller`. Note that `{Controller name}` must start with a capital letter. This class lives in a file called `{Controller name}Controller.php` within the `Controller` directory for the module. In our case that is `module/Album/src/Album/Controller`. Each action is a public method within the controller class that is named `{action name}Action`. In this case `{action name}` should start with a lower case letter.

> **注意**

> 这个是习惯用法。 Zend Framework 2 并不提供其他限制性控制器，用户必须实现 `Zend\Stdlib\Dispatchable` 这个接口类。这个框架提供了两个抽象类： `Zend\Mvc\Controller\AbstractActionController` 和 `Zend\Mvc\Controller\AbstractRestfulController`。我们可以使用标准的 `AbstractActionController` 类，如果你打算编写一个 RESTful 的 web 服务器，`AbstractRestfulController` 或许更有用。

> **Note**

> This is by convention. Zend Framework 2 doesn’t provide many restrictions on controllers other than that they must implement the `Zend\Stdlib\Dispatchable` interface. The framework provides two abstract classes that do this for us: `Zend\Mvc\Controller\AbstractActionController` and `Zend\Mvc\Controller\AbstractRestfulController`. We’ll be using the standard `AbstractActionController`, but if you’re intending to write a RESTful web service, `AbstractRestfulController` may be useful.

让我们继续在 `zf2-tutorials/module/Album/src/Album/Controller` 路径下的 `AlbumController.php` 的文件中构建控制器类：

Let’s go ahead and create our controller class `AlbumController.php` at `zf2-tutorials/module/Album/src/Album/Controller` :

```php
namespace Album\Controller;

 use Zend\Mvc\Controller\AbstractActionController;
 use Zend\View\Model\ViewModel;

 class AlbumController extends AbstractActionController
 {
     public function indexAction()
     {
     }

     public function addAction()
     {
     }

     public function editAction()
     {
     }

     public function deleteAction()
     {
     }
 }
```

> **注意**

> 确保 `Album` 模块已经通过 `config/application.config.php` 注册。你也可以为 Album 模块提供一个 [Module Class](http://framework.zend.com/manual/current/en/modules/zend.module-manager.module-class.html#zend-module-manager-module-class)，以便在 MVC 中使用。

> **Note**

> Make sure to register the new `Album` module in the “modules” section of your `config/application.config.php`. You also have to provide a [Module Class](http://framework.zend.com/manual/current/en/modules/zend.module-manager.module-class.html#zend-module-manager-module-class) for the Album module to be recognized by the MVC.

> **注意**
> 我们已经在 `module/Album/config/module.config.php` 中的 ‘controller’ 部分注册了信息。

> **Note**

> We have already informed the module about our controller in the ‘controller’ section of `module/Album/config/module.config.php`.

现在已经构建了四个动作，建立视图就可以运行了。每一个动作的 URLs 如下：

We have now set up the four actions that we want to use. They won’t work yet until we set up the views. The URLs for each action are:

| URL | Method called |
|-----|---------------|
| `http://zf2-tutorial.localhost/album` | `Album\Controller\AlbumController::indexAction` |
| `http://zf2-tutorial.localhost/album/add` | `Album\Controller\AlbumController::addAction` |
| `http://zf2-tutorial.localhost/album/edit` | `Album\Controller\AlbumController::editAction` |
| `http://zf2-tutorial.localhost/album/delete` | `Album\Controller\AlbumController::deleteAction` |

现在我们有一个可以工作的 router，动作已经建立在程序每个页面中。

We now have a working router and the actions are set up for each page of our application.

是时候建立视图和模型层。

It’s time to build the view and the model layer.

## Initialise the view scripts

初始化视图脚本

为了往程序中集成视图，需要创建一些视图脚本文件。这些文件通过 `DefaultViewStrategy` 执行，从控制器动作方法返回传递的变量或视图模型。这些视图脚本存储在模块的视图目录中的控制器。现在创建这四个空文件:

To integrate the view into our application all we need to do is create some view script files. These files will be executed by the `DefaultViewStrategy` and will be passed any variables or view models that are returned from the controller action method. These view scripts are stored in our module’s views directory within a directory named after the controller. Create these four empty files now:

* `module/Album/view/album/album/index.phtml`

* `module/Album/view/album/album/add.phtml`

* `module/Album/view/album/album/edit.phtml`

* `module/Album/view/album/album/delete.phtml`

现在从数据库和模型中提取数据，开始填充。

We can now start filling everything in, starting with our database and models.

