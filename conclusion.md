# Conclusion

结尾

总结使用 Zend Framework 2 构建一个简单的，但是全面的，MVC 程序。

This concludes our brief look at building a simple, but fully functional, MVC application using Zend Framework 2.

在这个向导中，我们短暂地接触了框架中的不同部分。

In this tutorial we but briefly touched quite a number of different parts of the framework.

Zend Framework 2 程序最重要的部分是 [modules](http://framework.zend.com/manual/current/en/modules/zend.module-manager.intro.html#zend-module-manager-intro)，用以构建 [MVC ZF2 application](http://framework.zend.com/manual/current/en/modules/zend.mvc.intro.html#zend-mvc-intro) 的每一部分。

The most important part of applications built with Zend Framework 2 are the [modules](http://framework.zend.com/manual/current/en/modules/zend.module-manager.intro.html#zend-module-manager-intro), the building blocks of any [MVC ZF2 application](http://framework.zend.com/manual/current/en/modules/zend.mvc.intro.html#zend-mvc-intro).

我们使用 [service manager](http://framework.zend.com/manual/current/en/modules/zend.service-manager.html#zend-service-manager-intro) 来处理程序中的依赖。

To ease the work with dependencies inside our applications, we use the [service manager](http://framework.zend.com/manual/current/en/modules/zend.service-manager.html#zend-service-manager-intro).

为了将请求和控制器以及动作映射起来，我们使用了 [routes](http://framework.zend.com/manual/current/en/modules/zend.mvc.routing.html#zend-mvc-routing)。

To be able to map a request to controllers and their actions, we use [routes](http://framework.zend.com/manual/current/en/modules/zend.mvc.routing.html#zend-mvc-routing).

最重要的是持久化数据，包括使用 [Zend\Db](http://framework.zend.com/manual/current/en/modules/zend.db.adapter.html#zend-db-adapter) 来联接一个数据库。输入数据通过 [input filters](http://framework.zend.com/manual/current/en/modules/zend.input-filter.intro.html#zend-input-filter-intro) 和 [Zend\Form](http://framework.zend.com/manual/current/en/modules/zend.form.intro.html#zend-form-intro) 进行过滤和验证，他们提供了一个强大的桥梁来链接域模型和视图层之间。

Data persistence, in most cases, includes using [Zend\Db](http://framework.zend.com/manual/current/en/modules/zend.db.adapter.html#zend-db-adapter) to communicate with one of the databases. Input data is filtered and validated with [input filters](http://framework.zend.com/manual/current/en/modules/zend.input-filter.intro.html#zend-input-filter-intro) and together with [Zend\Form](http://framework.zend.com/manual/current/en/modules/zend.form.intro.html#zend-form-intro) they provide a strong bridge between the domain model and the view layer.

[Zend\View](http://framework.zend.com/manual/current/en/modules/zend.view.quick-start.html#zend-view-quick-start) ，加上大量的[view helpers](http://framework.zend.com/manual/current/en/modules/zend.view.helpers.html#zend-view-helpers)，负责MVC中的视图堆栈。

[Zend\View](http://framework.zend.com/manual/current/en/modules/zend.view.quick-start.html#zend-view-quick-start) is responsible for the View in the MVC stack, together with a vast amount of [view helpers](http://framework.zend.com/manual/current/en/modules/zend.view.helpers.html#zend-view-helpers).