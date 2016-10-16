## Route Configuration
There are two ways that a route configuration for an application can be provided. Either as a [single](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) route or a [collection](https://github.com/mvc5/mvc5-application/blob/master/config/route.collection.php) of routes.


```php
return [
    'name'       => 'home',
    'route'      => '/',
    'controller' => 'Home\Controller',
    'children' => [
        'blog' => [
            'route'      => 'blog',
            'controller' => 'Blog\Controller'
        ]
    ]
]
```

```php
return [
    'home' => [
        'route' => '/{$}',
        'controller' => 'Home\Controller',
    ],
    'blog' => [
        'route' => '/blog{$}',
        'controller' => 'Blog\Controller'
    ]
    'blog:create' => [
        'route' => '/blog/create',
        'controller' => 'Blog\Create\Controller'
    ]
]
```


The standard configuration uses a [single](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) route for the homepage and specifies the leading forward slash as its route configuration. In which case, the url of the route configuration is the base url and the child routes immediately below the homepage configuration will not begin with a leading forward slash. Alternatively, a collection of routes can be used which do not require a parent route and their route configuration can begin with a leading forward slash.

It may be preferable to use a [single](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) route configuration for the whole site. The default configuration of the [controller match function](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Controller.php#L196) will replace a forward slash with a back slash and try to load a matching controller. The configuration should prepend a namespace and append a suffix to ensure that it is a valid controller.

```php
return [
    'name' => 'app',
    'route' => '/[{controller::*$}]',
    'defaults' => [
        'controller' => 'home'
    ],
    'options' => [
        'prefix' => 'App\\',
        'suffix' => '\Controller',
        'strict' => false,
    ]    
];
```

Strict mode does not change the case sensitivity of the controller name. However, most urls are lower case and file names and directories typically begin with an uppercase. This prevents controllers from being automatically auto-loaded. This can be resolved by using a service loader and having a service configuration with a matching lower case name. The service configuration will then specify the name of the class to use, e.g <code>'home\controller' => Home\Controller::class</code>.

When a [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L55) is specified, it must be a service configuration value (or a real value) that [resolves](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L572) to a [callable](http://php.net/manual/en/language.types.callable.php) type. If it does not have a [plugin configuration](https://github.com/mvc5/mvc5/blob/master/config/service.php) and its class exists, a new instance will be [created](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L124) and [autowired](#autowiring). Controller names prefixed with [@](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L13) will be directly [invoked](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L394) because they are either a [function](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L36) or a [static class method](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L32).

Custom [routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) can also be configured by adding a [class name](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L45) to the array, or the configuration can be a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) object.
