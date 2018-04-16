## Middleware
The Mvc5 [middleware](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php) handler is a [variadic](http://php.net/manual/en/functions.arguments.php#functions.variable-arg-list) function that is not limited to just HTTP requests and responses.
```php
function __invoke(...$args)
{
    return $this->call($this->rewind(), $args);
}
```
It is configured with a list of middleware functions that are chained together by each function calling a [delegate](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L43) function that is [appended](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L78) to the list of arguments passed to each middleware function.
```php
function call($middleware, array $args = [])
{
    return $middleware ? $this->service->call($middleware, $this->params($args)) : $this->end($args);
}
```
Unless the middleware function returns early, the last argument passed to the last [delegate](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L43) function is [returned](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L56) as the result; otherwise null is [returned](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L56). This allows middleware functions to be easily created and to vary the arguments passed to the [next](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L62) function.
```php
function __invoke(Route $route, Request $request, callable $next)
{
    return $next($route, $request);
}
```
[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) configurations must [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) to a [callable](http://php.net/manual/en/language.types.callable.php) type and can include [plugins](/plugins) and [anonymous functions](http://php.net/manual/en/functions.anonymous.php#functions.anonymous).
### HTTP Middleware
The signature of the [HttpMiddleware](https://github.com/mvc5/mvc5/blob/master/src/Http/HttpMiddleware.php) function is for handling [requests](https://github.com/mvc5/mvc5/blob/master/src/Http/Request.php) and [responses](https://github.com/mvc5/mvc5/blob/master/src/Http/Response.php).
```php
function __invoke(Request $request, Response $response)
{
    return $this->call($this->rewind(), [$request, $response]);
}
```
Below is the default middleware configuration for a HTTP middleware handler.
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
The [web\controller](https://github.com/mvc5/mvc5/blob/master/src/Web/Controller.php) calls the controller and if the returned value is not a [Http\Response](https://github.com/mvc5/mvc5/blob/master/src/Http/Response.php) and not null, it will be [set](https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php#L78) as the value of the response body for the remaining components to transform into a value that can be [sent](https://github.com/mvc5/mvc5/blob/master/src/Response/Service/Send.php#L79) to the client.
```php
function __invoke(Request $request, Response $response, callable $next)
{
    return $next($request, $this->send($response));
}
```
The PSR-7 Middleware demo can be enabled by uncommenting the [web configuration](https://github.com/mvc5/mvc5-application/blob/master/config/service.php#L50) in the [web application](#web-application) service config file.
```php
'web' => 'http\middleware'
```
### Pipelines
[Routes](#routes) can be configured with [middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) pipelines and (optionally) a controller. During the [route match](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7) process, the child stack is [appended](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Merge.php#L26) to the parent stack. When the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) is [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L56), the [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L38) is [appended](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L52) to the stack. A <code>controller</code> placeholder can also be [used](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L52) to indicate where to [insert](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L52) the [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L38). The [middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) stack then [becomes](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Middleware.php#L66) the [controller](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php#L34) for the [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php).
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
