## Routes
A [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) object is used to [match](https://github.com/mvc5/mvc5/blob/master/config/event.php#L22) a [request](https://github.com/mvc5/mvc5/blob/master/src/Route/Request.php). Each aspect of [matching](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php) a [request](https://github.com/mvc5/mvc5/blob/master/src/Route/Request.php) has a separate function, e.g. [scheme](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Scheme.php), [hostname](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Hostname.php), [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php), [method](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Method.php), [wildcard](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php), and any other function can be [configured](https://github.com/mvc5/mvc5/blob/master/config/event.php#L47) for the [route match event](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php). Before being [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php), if the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) does not have a [regular expression](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L90), it will be [created](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L95).

```php
return [
    'home' => [
        'route' => '/{$}'
        'regex' => '/$'
        'controller' => 'Home\Controller'
    ],
    'app' => [
        'route' => '/{controller::*$}' //{controller:[a-zA-Z0-9/]+[a-zA-Z0-9]$}
    ],
]
```

There are two ways a route can be configured, either by providing a regular expression or a route expression. If the route is only going to be used to match the request path, then a regular expression can be provided directly. This saves from having to create the regular expression from a route expression. If the route is also going to be used to create urls, then a route expression must be provided in order to create the regular expression and the tokens used by the [url generator](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php).

The regular expression can use named groups that are assigned as request parameters when the route is matched. Consequently, the parameter names must be alphanumeric. A route expression provides a simpler format for specifying regular expressions and group names. For example, if the matched url is <code>/about</code>, the app configuration assigns the value <code>about</code> as the controller request parameter.

Short names can also be assigned to regular expressions and be used in a route expression by prefixing them with a colon. If the route expression has a name, then two colons are required. Below are the default short-named regular expressions available to use.
  
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

The regular expression, or short-name, for a named route expression can also be provided separately as constraint. E.g

```php
return [
    'app' => [
        'route' => '/{controller}',
        'constraints' => [
            'controller' => '[a-zA-Z0-9/]+[a-zA-Z0-9]$'
        ]
    ],
]
```

Optional parameters are enclosed with square brackets <code>[]</code>. The syntax of the route expression is based on the [FastRoute](https://github.com/nikic/FastRoute) library and the short-name regular expressions are based on [Klien.php](https://github.com/klein/klein.php). However, the route component is mainly based on the [Dash](https://github.com/DASPRiD/Dash) library.
