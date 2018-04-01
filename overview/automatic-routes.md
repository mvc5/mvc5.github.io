### Automatic Routes
Controllers can be automatically matched to a url with the [controller match](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Controller.php#L196) function using a single route configuration.
```php
return [
    'app' => [
        'defaults' => ['controller' => 'home'],
        'options' => [
            'prefix' => 'App',
            'suffix' => '\Controller'
            'strict' => false,
        ],
        'path' => '/[{controller::*$}]'
    ]
];
```
The [controller match](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Controller.php#L196) function will change the first letter of each word separated by a forward slash in the url into an uppercase letter. It then replaces the forward slash with a back slash to create a fully qualified class name and will try to load a matching controller. In order to ensure that it is a valid controller, the configuration should prepend a namespace and append a suffix.

Strict mode does not change the case sensitivity of the controller name. However, most urls are lower case and file names and directories typically begin with an uppercase. This prevents controllers from being auto-loaded. This can be resolved by using a service loader and having a service configuration with a matching lower case name. The service configuration will then specify the name of the class to use, e.g <code>'home\controller' => Home\Controller::class</code>.

When a [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L55) is specified, it must have a service configuration value (or real value) that [resolves](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L572) to a [callable](http://php.net/manual/en/language.types.callable.php) type. If it does not have a [plugin configuration](https://github.com/mvc5/mvc5/blob/master/config/service.php) and its class exists, a new instance will be [created](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L124) and [autowired](#autowiring).

Controller names prefixed with the [<code>@</code>](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L13) symbol will be directly [invoked](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L394) because they are either a [function](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L36) or a [static class method](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L32).
