## Custom Service Providers
[Custom](https://github.com/mvc5/application/blob/master/src/Service/Config/Manager/Manager.php) service configurations and [providers](https://github.com/mvc5/application/blob/master/src/Service/Resolver/Manager/Resolver.php), or [resolvers](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L332), can also be created. These configurations must be [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) and their respective service [provider](https://github.com/mvc5/application/blob/master/src/Service/Resolver/Manager/Resolver.php), or [resolver](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L332), must be a [callable](http://php.net/manual/en/language.types.callable.php) function that has been added to the [service provider](https://github.com/mvc5/framework/blob/master/config/event.php#L45) event.

When the system resolves a configuration that is not one of the default [resolvable service configurations](#resolvable-service-configurations), the [service provider](https://github.com/mvc5/framework/blob/master/config/event.php#L45) event is [invoked](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L393) so that the configuration can be resolved by a [custom service provider](https://github.com/mvc5/application/blob/master/src/Service/Resolver/Manager/Resolver.php). If the configuration can not be resolved, the [default resolver](https://github.com/mvc5/framework/blob/master/src/Service/Provider/Resolver.php#L11) will throw an exception.  

For example, in the service configuration below, the view manager uses a custom [manager](https://github.com/mvc5/application/blob/master/src/Service/Config/Manager/Manager.php) configuration 

```php
use Mvc5\Service\Config\ServiceProvider\ServiceProvider;
use Service\Config\Manager\Manager as ServiceManager;

return [
    'Service\Resolver\Manager' => new ServiceProvider(ManagerResolver::class),
    'View\Manager'             => new ServiceManager(ViewManager::class)
];
```

and its corresponding [service provider](https://github.com/mvc5/application/blob/master/src/Service/Resolver/Manager/Resolver.php) function is added to configuration of the [service provider event](https://github.com/mvc5/framework/blob/master/config/event.php#L45).

```php
return [
    'Service\Provider' => [
        'Service\Resolver\Manager',
        'Service\DefaultResolver' //throws exception
    ],
]
```
