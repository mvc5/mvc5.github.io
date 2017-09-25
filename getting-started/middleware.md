## Middleware

An alternative to using an [event](https://github.com/mvc5/mvc5/blob/master/src/Event.php) is to call a list of functions using a [middleware](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php) handler. A [delegate](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L43) function is passed to the [current](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L37) function which can optionally call it to invoke the [next](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L62) function on the [stack](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7). If the last function calls the next [delegate](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L43) function again, the last argument passed to the [delegate](https://github.com/mvc5/mvc5/blob/master/src/Service/Middleware.php#L43) function is returned. Read more about <a href="/overview/#middleware">Middleware</a>.

```php
$config = include __DIR__ . '/../config/config.php';

$config['middleware']['web'] = [
    function($request, $response, $next) {
        return $next($request->with('controller', function() { return 'Hello!'; }), $response);
    },
    function($request, $response, $next) {
        return $next($request, $response->with('body', $request['controller']()));
    },
    function($request, $response, $next) {
        echo $response->body();
    }
];

(new Mvc5\App($config))->call('web\middleware');
```
