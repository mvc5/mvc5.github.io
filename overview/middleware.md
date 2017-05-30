## Middleware
[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) applications can be created with a configuration that supports [anonymous functions](http://php.net/manual/en/functions.anonymous.php#functions.anonymous), [plugins](#plugins) and [dependency injection](#dependency-injection).

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

The PSR-7 Middleware demo can be enabled by uncommenting the [web configuration](https://github.com/mvc5/mvc5-application/blob/master/config/service.php#L67) in the [web application](#web-application) service config file.

```php
//middleware demo
//'web' => 'web\middleware',
```

### Pipelines

[Routes](#routes) can be configured with [Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) pipelines and (optionally) a controller. During the [route match](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7) process the child stack is [appended](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Merge.php#L26) to the parent stack. When the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) is [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L38), the [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L38) is [appended](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L52) to the stack. A <code>controller</code> placeholder can also be [used](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L52) to indicate where to [insert](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L52) the [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L38). The [Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) stack then [becomes](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L66) the [controller](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php#L31) for the [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php).  
```php
'explore' => [
    'path' => '/explore',
    'middleware' => ['web\authenticate'],
    'defaults' => [
        'controller' => 'explore'
    ],
    'children' => [
        'more' => [
            'path' => '/more',
            'middleware' => ['controller', 'web\log'],
            'defaults' => [
                'controller' => 'more'
            ]
        ]
    ]
]
```

[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) configurations must [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L21) to a [callable](http://php.net/manual/en/language.types.callable.php) type and can include [plugins](#plugins) and [anonymous functions](http://php.net/manual/en/functions.anonymous.php#functions.anonymous).
