# 结尾

总结使用 Zend Framework 2 构建一个简单的，但是全面的，MVC 程序。

在这个向导中，我们短暂地接触了框架中的不同部分。

Zend Framework 2 程序最重要的部分是 [modules](http://framework.zend.com/manual/current/en/modules/zend.module-manager.intro.html#zend-module-manager-intro)，用以构建 [MVC ZF2 application](http://framework.zend.com/manual/current/en/modules/zend.mvc.intro.html#zend-mvc-intro) 的每一部分。

我们使用 [service manager](http://framework.zend.com/manual/current/en/modules/zend.service-manager.html#zend-service-manager-intro) 来处理程序中的依赖。

为了将请求和控制器以及动作映射起来，我们使用了 [routes](http://framework.zend.com/manual/current/en/modules/zend.mvc.routing.html#zend-mvc-routing)。

最重要的是持久化数据，包括使用 [Zend\Db](http://framework.zend.com/manual/current/en/modules/zend.db.adapter.html#zend-db-adapter) 来联接一个数据库。输入数据通过 [input filters](http://framework.zend.com/manual/current/en/modules/zend.input-filter.intro.html#zend-input-filter-intro) 和 [Zend\Form](http://framework.zend.com/manual/current/en/modules/zend.form.intro.html#zend-form-intro) 进行过滤和验证，他们提供了一个强大的桥梁来链接域模型和视图层之间。

[Zend\View](http://framework.zend.com/manual/current/en/modules/zend.view.quick-start.html#zend-view-quick-start) ，加上大量的[view helpers](http://framework.zend.com/manual/current/en/modules/zend.view.helpers.html#zend-view-helpers)，负责MVC中的视图堆栈。
