## Middleware

An alternative to using an [event](https://github.com/mvc5/mvc5/blob/master/src/Event.php) is to call a list of functions using [Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php). A [handler](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php#L33) is passed to the current function which can optionally call it to invoke the [next](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php#L34) function on the [stack](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7). If the last function calls the next [Http Middleware handler](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php#L33) again, the [response](https://github.com/mvc5/mvc5/blob/master/src/Http/Response.php) object will be returned. Read more about <a href="/overview/#middleware">Middleware</a>.

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
