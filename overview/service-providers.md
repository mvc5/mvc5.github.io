### Service Providers
Custom plugins can implement the [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) interface and extend an existing [plugin](#plugins) or have their own [service provider](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L480). A [service provider](https://github.com/mvc5/mvc5-application/blob/master/config/service.php#L39) is a [callable](http://php.net/manual/en/language.types.callable.php) function that is [invoked](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L480) when a [plugin](#plugins) can not be [resolved](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L511) by default.

```php
use Mvc5\Plugin\Config;
use Plugin\Controller;
use Service\Provider;

return [
    'Home\Controller'  => new Controller(Home\Controller::class),
    'service\provider' => [Service\Provider::class, new Config],
];
```

For example, the [home controller](https://github.com/mvc5/mvc5-application/blob/master/src/Home/Controller.php) uses a custom [controller plugin](https://github.com/mvc5/mvc5-application/blob/master/src/Plugin/Controller.php) with a [service provider](https://github.com/mvc5/mvc5-application/blob/master/src/Service/Provider.php) for the [service resolver event](https://github.com/mvc5/mvc5-application/blob/master/config/event.php#L45).

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

The [service resolver](https://github.com/mvc5/mvc5/blob/master/config/service.php#L67) event is used to call the [service provider](https://github.com/mvc5/mvc5/blob/master/config/service.php#L63) and [an exception](https://github.com/mvc5/mvc5/blob/master/config/service.php#L41) is thrown if the [plugin](#plugins) can not be resolved.

```php
'service\resolver' => [
    'service\provider',
    'resolver\exception'
],
```
