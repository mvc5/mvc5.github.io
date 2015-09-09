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
    'Route\Builder'   => Route\Builder\Builder::class,
    'Route\Generator' => new Service(
        Route\Generator\Generator::class,
        [new Param('routes'), new Dependency('Route\Builder')]
    )        
];
```

A string configuration is used when the class has no dependencies and when it can not be [autowired](#constructor-autowiring), e.g the service name is a short name or the name of an interface. An array configuration is used when there are required dependencies and its configuration can be further reduced by using array key names for the arguments that can not be [autowired](#constructor-autowiring). An anonymous function is used when the class instantiation requires custom logic and various [plugins](https://github.com/mvc5/framework/blob/master/config/alias.php) are available as [named arguments](#named-arguments-and-plugins).

However, anonymous functions can not be [serialized](http://php.net/manual/en/function.serialize.php) and there are times when additional methods need to be called after the instantiation of the class. In which case, a [service](https://github.com/mvc5/framework/blob/master/src/Service/Config/Service/Service.php) configuration can be used. It has a third parameter that accepts an array of methods and arguments to invoke, see the above [hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php) for an example of the array of calls.
 
When the dependencies of a class require their own dependencies then the depth of the dependency graph increases. In which case a composite [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) set of configurations can be used. Various types of [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configurations are [available](https://github.com/mvc5/framework/tree/master/src/Service/Config) for specific purposes.

[Custom](https://github.com/mvc5/application/blob/master/src/Service/Config/Manager/Manager.php) service configurations and [providers](https://github.com/mvc5/application/blob/master/src/Service/Resolver/Manager/Resolver.php) can also be created. These configurations must be [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) and their respective service provider must be a callable function that has been added to the [service provider](https://github.com/mvc5/framework/blob/master/config/event.php#L45) event; it can optionally implement the [service provider](https://github.com/mvc5/framework/blob/master/src/Service/Provider/ServiceProvider.php) interface. The [service provider](https://github.com/mvc5/framework/blob/master/config/event.php#L45) event is [invoked](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L393) after it has been determined that the configuration is not one of the [default](https://github.com/mvc5/framework/tree/master/src/Service/Config) [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) types.  

A [service](https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php) configuration can have parent configurations. This allows the parent constructor arguments to be used if none are provided by the child configuration. The parent configuration can also specify the setter <a href="https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php#L21">calls</a> to use and it is possible to <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L258">merge</a> them together.

```php
'Manager' => new Hydrator(null, [
    'aliases'       => new Param('alias'),
    'configuration' => new ConfigLink,
    'events'        => new Param('events'),
    'services'      => new Param('services'),
]),
```

The above [hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php) is used as a parent configuration for all <a href="https://github.com/mvc5/framework/blob/master/config/service.php#L57">Managers</a>.

```php
'Route\Manager' => new Manager(Route\Manager\Manager::class)
```

Because constructor arguments can also be real values, it is sometimes desired to provide an array that contains values which are resolved when the class is being instantiated. The [Args](https://github.com/mvc5/framework/blob/master/src/Service/Config/Args/Args.php) configuration can be used in place of an array and accepts both real and [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) type values, e.g 
<pre><code>'Route' => new Service(
    Route\Config::class,
    [
        new Args([
            'hostname' => new Call('request.getHost'),
            'method'   => new Call('request.getMethod'),
            'path'     => new Call('request.getPathInfo'),
            'scheme'   => new Call('request.getScheme')
        ])
    ])</code></pre>

The [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/Container.php) is part of the main application [config](https://github.com/mvc5/application/blob/master/config/config.php) and other configuration values can be retrieved using the [param](https://github.com/mvc5/framework/blob/master/src/Service/Config/Param/Param.php) configuration, e.g
 
```
new Param('templates.home')
```
