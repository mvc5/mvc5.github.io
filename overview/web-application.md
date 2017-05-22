## Web Application
The <a href="https://github.com/mvc5/mvc5-application">mvc5-application</a> demonstrates its usage as a [web application](https://github.com/mvc5/mvc5/blob/master/src/Web.php).

```php
include __DIR__ . '/../vendor/autoload.php';
```

```php
return [
    'cache'      => __DIR__ . '/../tmp',
    'cookie'     => include __DIR__ . '/cookie.php',
    'events'     => include __DIR__ . '/event.php',
    'middleware' => include __DIR__ . '/middleware.php',
    'routes'     => include __DIR__ . '/route.php',
    'services'   => include __DIR__ . '/service.php',
    'session'    => include __DIR__ . '/session.php',
    'templates'  => include __DIR__ . '/template.php',
    'view'       => __DIR__ . '/../view',
    'web'        => include __DIR__ . '/web.php',
];
```

```php
(new Web(include __DIR__ . '/../config/config.php', null, true))();
```
A default [configuration](https://github.com/mvc5/mvc5-application/blob/master/config/config.php) is provided with the minimum [configuration](https://github.com/mvc5/mvc5/tree/master/config) required to run a [web](https://github.com/mvc5/mvc5-application/blob/master/config/web.php) [application](https://github.com/mvc5/mvc5/blob/master/src/App.php). It contains configurations for PSR 7 compatible [request](https://github.com/mvc5/http-message/blob/master/config/service.php#L11) and [response](https://github.com/mvc5/http-message/blob/master/config/service.php#L13) objects, [templates](https://github.com/mvc5/mvc5-application/blob/master/config/template.php) and [routes](#routes). The third parameter passed to the [web](https://github.com/mvc5/mvc5/blob/master/src/Web.php) class binds the scope of the [anonymous functions](http://php.net/manual/en/functions.anonymous.php#functions.anonymous), within the [service configuration](https://github.com/mvc5/mvc5-application/blob/master/config/service.php), to the [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) class. 
