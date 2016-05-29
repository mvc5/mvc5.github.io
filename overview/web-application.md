## Web Application
The <a href="https://github.com/mvc5/mvc5-application">mvc5-application</a> demonstrates its usage as a [web application](https://github.com/mvc5/mvc5/blob/master/src/Web.php).

```php
include __DIR__ . '/../vendor/autoload.php';
```

```php
$config = [
    'events'     => include __DIR__ . '/event.php',
    'middleware' => include __DIR__ . '/middleware.php',
    'routes'     => include __DIR__ . '/route.php',
    'services'   => include __DIR__ . '/service.php',
    'templates'  => include __DIR__ . '/template.php'
];
```

```php
call_user_func(new Web(include __DIR__ . '/../config/config.php', null, true));
```
A default [configuration](https://github.com/mvc5/mvc5/tree/master/config) is provided with the minimum [configuration](https://github.com/mvc5/mvc5/tree/master/config) required to run a [web application](https://github.com/mvc5/mvc5/blob/master/src/Web.php). All that is required are the [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php) and [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) objects, [template](https://github.com/mvc5/mvc5-application/blob/master/config/template.php) configuration and the [routes](#routes) to use. [Routes](#routes) must have a name so that they can be used to [generate urls](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php) in [view model](#view-models) templates using the [url plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php#L47). The third parameter passed to the web class enables anonymous functions within the service configuration to be [scoped](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L607) with the current application context. 
