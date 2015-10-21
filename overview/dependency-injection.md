## Dependency Injection
When a class is [created](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ManageService.php#L29) and it can not be [autowired](#constructor-autowiring), then a service configuration is required. Different types of configurations can be used depending on the requirements of the class. These configurations can be either a string, an array, an anonymous function, a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) type or a real value. An [array](https://github.com/mvc5/framework/blob/master/config/service.php) is used for the configuration of a [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/Container.php).

```php
[
    'Home' => Home::class,
    'Blog' => [Blog\Controller::class, 'template' => 'blog'],
    'Mvc'  => function(Configuration $sm) {
        return new Mvc($sm);
    },
    'Mvc\View' => new Hydrator(
        Mvc\View\Renderer::class,
        ['setViewManager' => new Dependency('View\Manager')]
    ),    
    'Request' => new Request\HttpRequest($_GET, $_POST, [], $_COOKIE, $_FILES, $_SERVER),
    'Route\Definition\Url' => Route\Definition\Url\Url::class,
    'Route\Generator' => new Service(
        Route\Generator\Generator::class,
        [new Param('routes'), new Dependency('Route\Definition\Url')]
    )        
];
```

A string configuration is used to map a short name or an interface to a class name. Because it is a string configuration, the class either has no dependencies or it can be [autowired](#constructor-autowiring). An array configuration is used when there are required dependencies and its configuration can be further reduced by using array key names for the arguments that can not be [autowired](#constructor-autowiring). An anonymous function is used when the class instantiation requires custom logic and various [plugins](https://github.com/mvc5/framework/blob/master/config/alias.php) are available as [named arguments](#named-arguments-and-plugins).

However, when the dependencies of a class require their own dependencies, then the depth of the dependency graph increases. In this case, a class object can also be created using a resolvable service configuration instead of using an anonymous function, factory or service provider method. This provides inversion of control and is a configuration domain specific language. Each configuration must implement the [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) type interface so that the system can distinguish them from other objects and invoke their associated function. These configurations can be used by each other and chained together to form a composite set of [resolvable service configurations](#resolvable-service-configurations).

When the name of a service configuration is a <a href="https://en.wikipedia.org/wiki/Fully_qualified_name">FQCN</a>, it must have a value different to its string name, otherwise a recursion will occur. Service configurations are only required when an explicit configuration is needed.

```php
Home\Model::class => Home\Model::class //not allowed
```
