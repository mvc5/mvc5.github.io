## Routes
A collection of [routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) are used to [match](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php) a [request](https://github.com/mvc5/mvc5/blob/master/src/Http/Request.php) using [middleware](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7) route components. Each aspect of [matching](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php) a route to a [request](https://github.com/mvc5/mvc5/blob/master/src/Http/Request.php) is a separate function. For example, [scheme](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Scheme.php), [hostname](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Hostname.php), [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php), [method](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Method.php), [wildcard](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php) and any other function can be [configured](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7). If a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) does not contain a [regular expression](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L90), it will be [created](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L70) from the [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L68) configuration before being [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L47). 
```php
return [
    'home' => [
        'path' => '/{$}'
        'regex' => '/$'
        'controller' => 'Home\Controller'
    ],
    'app' => [
        'path' => '/{controller::*$}' //{controller:[a-zA-Z0-9/]+[a-zA-Z0-9]$}
    ],
]
```
Routes can be configured with a regular expression or a path. If the route is only used to [match](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L47) the request path, then only a regular expression is required. If the route is also used to create a url, then a path configuration is required.

The regular expression can use group names that are [assigned](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L54) as request parameters when the route is [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L47). Consequently, the parameter names must be alphanumeric. The path configuration provides a simpler format for specifying regular expressions and group names. For example, if the matched url is <code>/about</code>, the <code>app</code> route configuration [assigns](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L54) the value <code>about</code> as the <code>controller</code> request parameter.

Short names can also be assigned to regular expressions and be used in a path configuration by prefixing them with a single colon or two colons when assigned to a parameter name. Below are the default short-named regular expressions [available](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Tokens.php#L23) to use.
```php
[
    'a' => '[a-zA-Z0-9]++',
    'i' => '[0-9]++',
    'n' => '[a-zA-Z][a-zA-Z0-9]++',
    's' => '[a-zA-Z0-9_-]++',
    '*' => '.++',
    '*$' => '[a-zA-Z0-9/]+[a-zA-Z0-9]$'
]
```
The regular expression, or short-name, for a path configuration can also be specified as a constraint.
```php
return [
    'app' => [
        'path' => '/{controller}',
        'constraints' => [
            'controller' => '[a-zA-Z0-9/]+[a-zA-Z0-9]$'
        ]
    ],
]
```
Optional parameters are enclosed with square brackets <code>[]</code>. The syntax of the path configuration is based on the [FastRoute](https://github.com/nikic/FastRoute) library and the short-name regular expressions are based on [Klien.php](https://github.com/klein/klein.php). However, the route component is mainly based on the [Dash](https://github.com/DASPRiD/Dash) library.

Custom [routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) can also be configured by adding a [class name](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L45) to the array, or the configuration can be a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) object.
