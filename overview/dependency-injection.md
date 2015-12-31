## Dependency Injection
When a class is [created](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L251) and it can not be [autowired](#autowiring), then a service configuration is required. Different types of configurations can be used depending on the requirements of the class. These configurations can be either a string, an array, an anonymous function, a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) [plugin](#plugins) or a real value.

```php
[
    'home'          => Home\Controller::class,
    'blog'          => [Blog\Controller::class, 'template' => 'blog'],
    'mvc'           => function(Configuration $sm) { return new Mvc5\Mvc('mvc', $sm); },
    'mvc\view'      => new Hydrator(Mvc5\Mvc\View::class, ['service' => new Link]),    
    'request'       => new Request\HttpRequest($_GET, $_POST, [], $_COOKIE, $_FILES, $_SERVER),
    'response'      => Response\HttpResponse::class,
    'url'           => new Dependency('url\plugin'),
    'url\generator' => [Mvc5\Url\Generator::class, new Param('routes')],
    'url\plugin'    => [Mvc5\Url\Plugin::class, new Dependency('route'), new Plugin('url\generator')]
];
```

A string configuration is used to map a class name, or a short name, or an interface name to a fully qualified class name, or another configuration name. Because it is a string configuration, the class either has no dependencies or it can be [autowired](#autowiring). An array configuration is used when there are required dependencies and its configuration can be further reduced by using array key names for the arguments that can not be [autowired](#autowiring). An anonymous function is used when the class instantiation requires custom logic and various [plugins](https://github.com/mvc5/mvc5/blob/master/config/service.php) are available as [named arguments](#named-arguments-and-plugins).

However, when the dependencies of a class require their own dependencies, then the depth of the dependency graph increases. In this case, a class object can also be created using a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) [plugin](#plugins) instead of using an anonymous function or factory method. This provides inversion of control and is a configuration domain specific language. Each [plugin](#plugins) must implement the [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) interface so that the system can distinguish them from other objects and invoke their associated function. Plugins can be used by each other and chained together to form a composite [plugin](#plugins) and are only required when an explicit configuration is needed.
