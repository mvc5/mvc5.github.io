## Middleware
[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) applications can be created with a configuration that supports anonymous functions, [plugins](#plugins) and [dependency injection](#dependency-injection).

```php
'web' => [
    'web\route',
    'web\error',
    'web\service',
    'web\controller',
    'web\layout',
    'web\render',
    'web\status',
    'web\version',
    'web\send',
],
```

The [web\controller](https://github.com/mvc5/mvc5/blob/master/src/Web/Controller.php) calls the controller and if the returned value is not a [Http\Response](https://github.com/mvc5/mvc5/blob/master/src/Http/Response.php) and it is not null, it will be [set](https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php#L79) as the value of the response body for the remaining components to transform into a value that can be [sent](https://github.com/mvc5/mvc5/blob/master/src/Response/Send/Send.php#L35) to the client.

```php
function __invoke(Request $request, Response $response, callable $next)
{
    return $next($request, $this->send($response));
}
```

The Middleware demo can be enabled by uncommenting the [web configuration](https://github.com/mvc5/mvc5-application/blob/master/config/service.php#L67) in the [web application](#web-application) service configuration file.

```php
//middleware demo
//'web' => 'web\middleware',
```

