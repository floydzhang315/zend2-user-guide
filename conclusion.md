# Conclusion

This concludes our brief look at building a simple, but fully functional, MVC application using Zend Framework 2.

In this tutorial we but briefly touched quite a number of different parts of the framework.

The most important part of applications built with Zend Framework 2 are the [modules](http://framework.zend.com/manual/current/en/modules/zend.module-manager.intro.html#zend-module-manager-intro), the building blocks of any [MVC ZF2 application](http://framework.zend.com/manual/current/en/modules/zend.mvc.intro.html#zend-mvc-intro).

To ease the work with dependencies inside our applications, we use the [service manager](http://framework.zend.com/manual/current/en/modules/zend.service-manager.html#zend-service-manager-intro).

To be able to map a request to controllers and their actions, we use [routes](http://framework.zend.com/manual/current/en/modules/zend.mvc.routing.html#zend-mvc-routing).

Data persistence, in most cases, includes using [Zend\Db](http://framework.zend.com/manual/current/en/modules/zend.db.adapter.html#zend-db-adapter) to communicate with one of the databases. Input data is filtered and validated with [input filters](http://framework.zend.com/manual/current/en/modules/zend.input-filter.intro.html#zend-input-filter-intro) and together with [Zend\Form](http://framework.zend.com/manual/current/en/modules/zend.form.intro.html#zend-form-intro) they provide a strong bridge between the domain model and the view layer.

[Zend\View](http://framework.zend.com/manual/current/en/modules/zend.view.quick-start.html#zend-view-quick-start) is responsible for the View in the MVC stack, together with a vast amount of [view helpers](http://framework.zend.com/manual/current/en/modules/zend.view.helpers.html#zend-view-helpers).