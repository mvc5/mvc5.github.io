## Dependency Injection
An [array](https://github.com/mvc5/framework/blob/master/config/service.php) is used for the configuration of a [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/Container.php). It can contain string, array, anonymous functions, [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration types and real values.

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

A string configuration is used when the class to instantiate has no dependencies or it can be [autowired](#constructor-autowiring). An array configuration is used when the class does have dependencies, and if desired, only the required constructor arguments that can not be [autowired](#constructor-autowiring) need to be specified. And when the instantiation of a class requires its own logic, an anonymous function can be used; various [plugins](https://github.com/mvc5/framework/blob/master/config/alias.php) are available as [named arguments](#named-arguments-and-plugins).

However, anonymous functions can not be [serialized](http://php.net/manual/en/function.serialize.php) and there are times when additional methods need to be called after instantiation. This is when a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) [Service](https://github.com/mvc5/framework/blob/master/src/Service/Config/Service/Service.php) configuration can be used which has a third parameter that accepts an array of methods and arguments to invoke. For example, in the above, the [Hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php) uses just the class name and an array of calls.
 
When the dependencies of a class require their own dependencies, either an anonymous function or a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration can be used, see for example the route generator configuration.

Custom service configurations and providers can also be created. The service configuration must be [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) and the service provider must be a callable function that has been added to the [service provider](https://github.com/mvc5/framework/blob/master/config/event.php#L45) event; it can also optionally implement the [service provider](https://github.com/mvc5/framework/blob/master/src/Service/Provider/ServiceProvider.php) interface. The [service provider](https://github.com/mvc5/framework/blob/master/config/event.php#L45) event is invoked after it has been determined that the [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration is not one of the [default](https://github.com/mvc5/framework/tree/master/src/Service/Config) configuration types.  

When a [Dependency](https://github.com/mvc5/framework/blob/master/src/Service/Config/Dependency/Dependency.php) is to be shared, the service manager will check to see if the service exists and if it doesn't it will use its [configuration](https://github.com/mvc5/framework/blob/master/config/service.php) value to create the new service. This allows the configuration value to also be a real value, e.g

```php
'Request' => new HttpRequest($_GET, $_POST, $_SERVER ...)
```
  
A [Service](https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php) configuration can have parent configurations which allows either the parent constructor arguments to be used, if none are provided, or the parent configuration may specify the <a href="https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php#L21">calls</a> to use. It is also possible for a <a href="https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php">service configuration</a> to <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L258">merge</a> <a href="https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php#L21">calls</a> together.

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

Because constructor arguments can also be real values it is sometimes desired to provide an array that contains values which only be resolved when the class is being instantiated. The [Args](https://github.com/mvc5/framework/blob/master/src/Service/Config/Args/Args.php) configuration can be used in place of an array and accepts an array of both real values and [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration types. 
<pre><code>new Args([new Service('Mvc'), 'foo', new Dependency('Controller\Manager')])</code></pre>

The [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/Container.php) is part of the main application [config](https://github.com/mvc5/application/blob/master/config/config.php). This allows a [Param](https://github.com/mvc5/framework/blob/master/src/Service/Config/Param/Param.php) configuration to be used to retrieve other configuration values, e.g
 
```
new Param('templates.home')
```
