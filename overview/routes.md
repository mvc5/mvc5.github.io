## Routes
A collection of [routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) are used to [match](https://github.com/mvc5/mvc5/blob/master/src/Route/Dispatch/Router.php#L88) a [request](https://github.com/mvc5/mvc5/blob/master/src/Http/Request.php) using [middleware](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7) route components. Each aspect of [matching](https://github.com/mvc5/mvc5/tree/master/src/Route/Match) a route is a separate function. For example, [scheme](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Scheme.php), [host](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Host.php), [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php), [method](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Method.php), [wildcard](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php) or any other function can be [configured](https://github.com/mvc5/mvc5/blob/master/config/middleware.php#L7).
```php
return [
    'home' => [
        'path' => '/{$}'
        'regex' => '/$'
        'controller' => 'Home\Controller'
    ],
    'dashboard' => [
        'path' => '/dashboard/{user}',
        'controller' => 'dashboard->controller.test',
    ]
]
```
Routes can be configured with a regular expression or a path. If the route is only used to [match](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L47) the request path, then only a regular expression is required. If the route is also used to create a url, then a path configuration is required. If a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) does not contain a [regular expression](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L78), it will be [created](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L72) from the [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L68) configuration before being [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L29).

When a route is [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L29), the route's <code>controller</code> configuration is assigned to the request. However, a route does not always require a <code>controller</code> configuration, it can also have a [middleware](#middleware) configuration and an [automatic route](#automatic-routes) configuration.

Custom [routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) can also be configured by adding a [class name](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L39) to the array, or the configuration can be a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) object containing a regular expression (and optionally a path configuration).

### Regular Expressions
The regular expression for a route can use group names that are [assigned](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L40) as parameters to the request when the route is [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L29). Parameter names must be alphanumeric and the path configuration provides a simpler format for specifying a regular expression and group names. Aliases can be assigned to a regular expression and be used in a path configuration by prefixing them with a single colon or two colons when assigned to a parameter name. Below are the default aliases [available](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Tokens.php#L23) to use.
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
An alias or regular expression for a path configuration can also be specified as a constraint.
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
Optional parameters are enclosed with square brackets <code>[]</code>. The syntax of the path configuration is based on the [FastRoute](https://github.com/nikic/FastRoute) library and the aliases are based on [Klien.php](https://github.com/klein/klein.php). However, the route component is mainly based on the [Dash](https://github.com/DASPRiD/Dash) library.
