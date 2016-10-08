## Routes
A [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) object is used to [match](https://github.com/mvc5/mvc5/blob/master/config/event.php#L22) a [request](https://github.com/mvc5/mvc5/blob/master/src/Route/Request.php). Before being [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php), if the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) does not have a [regular expression](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L90), it will be [compiled](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L38) against the [request](https://github.com/mvc5/mvc5/blob/master/src/Http/Request.php)'s uri [path](https://github.com/mvc5/mvc5/blob/master/src/Http/Uri.php#L31). Each aspect of [matching](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php) a [request](https://github.com/mvc5/mvc5/blob/master/src/Route/Request.php) has a dedicated function, e.g. [scheme](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Scheme.php), [hostname](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Hostname.php), [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php), [method](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Method.php), [wildcard](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php), and any other function can be [configured](https://github.com/mvc5/mvc5/blob/master/config/event.php#L47) for the [route match event](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php).

In order to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php) a url using the [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php), the [base route](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) must have a name, which is typically the homepage for <code>/</code>, e.g [<code>home</code>](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L7), or it can specify its own, e.g <code>/application</code>. [Child](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L16) routes, except for the first level, will automatically have their parent name [prepended](https://github.com/mvc5/mvc5/blob/master/src/Route/Dispatch/Router.php#L121) to their name, e.g <code>blog/create</code>. First level routes will not have the [base route](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L7) prepended as this simplifies their name when [specifying](https://github.com/mvc5/mvc5-application/blob/master/view/blog/create.phtml#L2) which [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php#L48), e.g. <code>blog</code> instead of <code>home/blog</code>.

The [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L55) param must be a service configuration value (which includes real values) that must [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L84) to a [callable](http://php.net/manual/en/language.types.callable.php) type. If there is no [plugin configuration](https://github.com/mvc5/mvc5/blob/master/config/service.php) for a controller and its class exists, a new instance will be [created](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L124) and [autowired](#autowiring).

Controller names prefixed with [@](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L18) will be [invoked directly](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L128) as they are either a [function](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L36) or a [static class method](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L32).

Constraints have named keys that match their corresponding [regex](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L209) parameter and optional parameters are enclosed with square brackets `[]`. This implementation is from the [DASPRiD/Dash](https://github.com/DASPRiD/Dash) project.

Custom [routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) can also be configured by adding a [class name](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L45) to the array, or the configuration itself can be a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) object.

```php
return [
    'name'       => 'home', //for the url plugin in view templates
    'route'      => '/',
    'class'      => Route::class, //optional
    'controller' => 'Home\Controller', //callable
    //'controller' => '@Home\Controller.test', //callable
    'children' => [
        'blog' => [
            'route'      => 'blog',
            'controller' => 'blog->controller.test', //specific method
            'children' => [
                'remove' => [
                    'route' => '/remove',
                    'controller' => 'blog:remove'
                ],
                'create' => [
                    'route'      => '/:author[/:category[/:wildcard]]',
                    'defaults'   => [
                        'author'   => 'owner',
                        'category' => 'web'
                    ],
                    'wildcard'   => true,
                    'controller' => 'blog:add', //call event
                    //'controller'  => function($request) { //named args
                        //var_dump($request->getPathInfo());
                    //},
                    'constraints' => [
                        'author'   => '[a-zA-Z0-9_-]+',
                        'category' => '[a-zA-Z0-9_-]+',
                        'wildcard' => '[a-zA-Z0-9/]+[a-zA-Z0-9]$'
                    ]
                ]
            ],
        ]
    ]
];
```

[Route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) names are used by the url [route generator](https://github.com/mvc5/mvc5/blob/master/src/Route/Generator.php) and can create an absolute url, e.g

```php
echo $this->url('blog/create', ['author' => 'owner', 'category' => 'oop'], ['canonical' => true]);
```

An [automatic route configuration](https://github.com/mvc5/mvc5/tree/master/config/route.php) can be used to match the first and second url parameters to a controller class. For example, the controller for <code>/status/view</code> is <code>Status\View\Controller</code>. A namespace prefix and other options can be configured.

```php
return [
    'defaults'    => ['controller' => 'home'],
    'constraints' => ['controller' => '[a-zA-Z][a-zA-Z0-9]+', 'action' => '[a-zA-Z][a-zA-Z0-9/]+'],
    'name'        => 'app',
    'options'     => [
        'action'     => 'action',
        'controller' => 'controller',
        'prefix'     => '',
        'separators' => ['/' => '\\'],
        'split'      => '\\',
        'strict'     => false,
        'suffix'     => '\Controller'
    ],
    'route' => '/[:controller[/:action]]'
]
```

Strict mode does not change the case of the controller or action name. However, most urls are lower case and file names and directories typically begin with an uppercase letter. This means the controllers can not be automatically auto-loaded. This can be resolved by using a service loader and having a service configuration with a matching lower case name. The service configuration will then specify the name of the class to be auto-loaded. E.g <code>'home\controller' => Home\Controller::class</code>.

Urls can then be created by specifying the name of the controller and action. Wild card parameters can also be enabled.

```php
echo $this->url('app', ['controller' => 'status']);
```
