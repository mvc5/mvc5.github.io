## Dependency Injection
The [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) implements the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface by extending the [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/ServiceContainer.php). The [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface provides access to existing services, and the [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/ServiceContainer.php) provides access to the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object that contains the configuration values for the services that the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) provides.

Typically the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) is the application's main [configuration object](https://github.com/mvc5/framework/blob/master/config/config.php).

```php
return [
    'alias'     => include __DIR__ . '/alias.php',
    'events'    => include __DIR__ . '/event.php',
    'services'  => new Container(include __DIR__ . '/service.php'),
    'routes'    => include __DIR__ . '/route.php',
    'templates' => include __DIR__ . '/templates.php'
];
```

This allows the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) to use the [param()](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php#L40) method to retrieve other configuration values, e.g `new Param('templates.home')`.

When a service is called by the service manager's [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface, it will check that the service exists, and if it does not, it will use its [configuration](https://github.com/mvc5/framework/blob/master/config/service.php) to create a new service. Configuration values can also be actual values e.g 

```php
'Request' => new HttpRequest($_GET, $_POST, $_SERVER ...)
```

They can also be strings that specify the `FQCN` of the class to instantiate and their dependencies can be automatically injected via [Constructor Autowiring](#Constructor Autowiring). E.g 

```php
'Route\Match\Wildcard' => Route\Match\Wildcard\Wildcard::class,
```

Constructor arguments can also be passed as arguments to the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) when calling the service via the [create](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ManageService.php#L29) or [get](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ManageService.php#L57) method, e.g 

```php
$sm->get('HomeController', [new Dependency('HomeManager')])
```

There is no convention on how dependencies should be injected, however arguments passed to [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) will be used as constructor arguments. If no arguments are passed, the service can still be configured with constructor arguments via the [service configuration](https://github.com/mvc5/framework/blob/master/config/service.php).

```php
'Route\Generator' => new Service(
    Route\Generator\Generator::class,
    [new Param('routes'), new Dependency('Route\Builder')]
),
```

In the example above the [route generator](https://github.com/mvc5/framework/blob/master/src/Route/Generator/Generator.php) is created with the [routes](#routes) configuration passed as a constructor argument.

Sometimes only `calls` to the `setter methods` are needed, in which case the [hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php) configuration object can be used.

```php
'Controller\Manager' => new Hydrator(
    Controller\Manager\Manager::class,
    [
        'aliases'       => new Param('alias'),
        'configuration' => new ConfigLink,
        'events'        => new Param('events'),
        'services'      => new Param('services')
    ]
),
```

A [service configuration](https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php) can have parent configurations which allows either the parent constructor arguments to be used, if none are provided, or the parent configuration may specify the `calls` to use. It is also possible for service configurations to merge their `calls` together.

```php
'Manager' => new Hydrator(null, [
    'aliases'       => new Param('alias'),
    'configuration' => new ConfigLink,
    'events'        => new Param('events'),
    'services'      => new Param('services'),
]),
```

The above [hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php) is used as a parent configuration for all `Managers`.

```php
'Route\Manager' => new Manager(Route\Manager\Manager::class)
```

A [dependency configuration object](https://github.com/mvc5/framework/blob/master/src/Service/Config/Dependency/Dependency.php) is used to retrieve a shared service.

## Constructor Autowiring
When a [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) creates a class that either

* Does not have a service configuration, or
* No arguments are passed to the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php), or
* If the arguments passed are `named`

Then the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) will attempt to determine the *required* dependencies of the class constructor by their type hint. Service configurations can return any positive value.

```php
Home\Model::class => new Service(Home\Model::class,['home'])
```

When the name of a service configuration is a `FQCN` it must have a value other than its string name, otherwise a recursion will occur.

```php
Home\Model::class => Home\Model::class //not allowed
```

Service configurations are only required when an explicit configuration is needed, and in some cases, can provide better runtime performance.
