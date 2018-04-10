## Dependency Injection
A service configuration can either be a string, an array, an anonymous function, a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) [plugin](#plugins) or a real value.
The service name can either be a short name or a class or interface name.
```php
[
    'home' => Home\Controller::class,
    'request' => Mvc5\Request\HttpRequest::class,
    'response' => Mvc5\Response\HttpResponse::class,
    'url' => new Shared('url\plugin'),
    'url\generator' => [Mvc5\Url\Generator::class, new Param('routes')],
    'url\plugin' => [Mvc5\Url\Plugin::class, new Shared('request'), new Plugin('url\generator')],
    'web' => new Response('web')
];
```
A string configuration can be a class name, or the name of another configuration. When it is a class name, the class either has no dependencies or it can be [autowired](#autowiring). An array configuration is used when there are required dependencies; and its configuration can be further reduced by using array key names for the arguments that cannot be [autowired](#autowiring). An anonymous function can be used when the class instantiation requires custom logic. However, when the dependencies of a class have their own dependencies, then the depth of the dependency graph increases. In this case, a [plugin](#plugins) can be used instead of an anonymous function (or a factory method). [Plugins](#plugins) are a configuration domain specific language and provide inversion of control. Each [plugin](#plugins) must implement the [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) interface so the system can distinguish them from other objects and invoke their associated function. They can be nested together to form a composite plugin and are only required when an explicit configuration is necessary.

