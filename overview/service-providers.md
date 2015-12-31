## Service Providers
Custom plugins can implement the [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) interface and extend an existing [plugin](#plugins) or have their own [service provider](https://github.com/mvc5/mvc5-application/blob/master/src/Service/ServiceProvider.php). A [service provider](https://github.com/mvc5/mvc5/blob/master/config/service.php#L63) is a [callable](http://php.net/manual/en/language.types.callable.php) function that is [invoked](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L404) when a [plugin](#plugins) can not be [resolved](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L404) by [default](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L338). If the service provider can not resolve the [plugin](#plugins), the [default resolver](https://github.com/mvc5/mvc5/blob/master/config/event.php#L55) will [throw an exception](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Exception.php#L18). 

```php
use Plugin\Controller;
use Service\ServiceProvider;
use Service\ServiceManager;

return [
    'Home\Controller'  => new Controller(Home\Controller::class),
    'service\provider' => new Service(
        ServiceProvider::class, [], [Arg::SERVICE => new Plugin('service\manager')]
    ),
    'service\manager'  => new Manager(ServiceManager::class),
];
```

For example, the [home controller](https://github.com/mvc5/mvc5-application/blob/master/src/Home/Controller.php) uses a custom [controller plugin](https://github.com/mvc5/mvc5-application/blob/master/src/Plugin/Controller.php) with a custom [service provider](https://github.com/mvc5/mvc5-application/blob/master/src/Service/ServiceProvider.php) that has been added to the [service resolver event](https://github.com/mvc5/mvc5-application/blob/master/config/event.php#L28). In this example, the [service provider](https://github.com/mvc5/mvc5-application/blob/master/src/Service/ServiceProvider.php) uses its own [service manager](https://github.com/mvc5/mvc5-application/blob/master/src/Service/ServiceManager.php) to [resolve](https://github.com/mvc5/mvc5-application/blob/master/src/Service/ServiceManager.php#L31) the [controller plugin](https://github.com/mvc5/mvc5-application/blob/master/src/Plugin/Controller.php).

```php
function resolve($config, array $args = [])
{
    return $this->resolvable($config, $args, function($config) {
        if ($config instanceof Controller) {
            return $this->make($config->config());
        }

        return $config;
    });
}
```
