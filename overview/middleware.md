## Middleware
[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) applications can be created with a configuration that supports anonymous functions, [plugins](#plugins) and [dependency injection](#dependency-injection).

```php
return [
    'web' => [
        'web\route',
        'web\error',
        'web\service',
        'web\dispatch',
        'web\status',
        'web\version',
        'web\send',
    ],
    'web\response' => [
        'web\controller',
        'web\layout',
        'web\render',
    ],
];
```

The [web\dispatch](https://github.com/mvc5/mvc5/blob/master/src/Web/Dispatch.php) component calls the inner middleware <code>web\response</code> component which can be short circuited and returns a response for the remaining <code>web</code> middleware components to use.

```php
function __invoke(Request $request, Response $response, callable $next)
{
    return $next($request, $this->call('web\response'', [$request, $response]));
}
```

If the value returned by a controller is not a [http\response](https://github.com/mvc5/mvc5/blob/master/src/Http/Response.php), it is set as the body of the response. This allows the remaining components to inspect the response body and transform it into a value that can be sent to the client.
