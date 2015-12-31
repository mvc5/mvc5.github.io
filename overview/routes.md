## Routes
A [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) [definition](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) is a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) object that is [configured](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) with an array that contains the [definitions](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) required to [match](https://github.com/mvc5/mvc5/blob/master/config/event.php#L47) a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php). Before being [matched](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php), if the [definition](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) does not have a [regex](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php#L74), it will be [compiled](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L38) against the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php)'s request uri [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php#L51). Each aspect of [matching](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php) a [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) has a dedicated function, e.g. [scheme](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Scheme.php), [hostname](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Hostname.php), [path](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php), [method](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Method.php), [wildcard](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php), and any other function can be [configured](https://github.com/mvc5/mvc5/blob/master/config/event.php#L47) for the [route match event](https://github.com/mvc5/mvc5/blob/master/src/Route/Match.php).

In order to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php) a url using the [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php), the [base route](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) must have a name, which is typically the homepage for <code>/</code>, e.g [<code>home</code>](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L7), or it can specify its own, e.g <code>/application</code>. [Child](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L16) routes, except for the first level, will automatically have their parent name [prepended](https://github.com/mvc5/mvc5/blob/master/src/Route/Router/Router.php#L61) to their name, e.g <code>blog/create</code>. First level routes will not have the [base route](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L7) prepended as this simplifies their name when [specifying](https://github.com/mvc5/mvc5-application/blob/master/view/blog/create.phtml#L2) which [route definition](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php#L48), e.g. <code>blog</code> instead of <code>home/blog</code>.

The [controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php#L44) param must be a service configuration value (which includes real values) that must [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L84) to a [callable](http://php.net/manual/en/language.types.callable.php) type. If there is no [plugin configuration](https://github.com/mvc5/mvc5/blob/master/config/service.php) for a controller and its class exists, a new instance will be [created](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L124) and [autowired](#autowiring).

Controller names that are prefixed with an [@](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L18) will be [invoked directly](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L67) as they are either a [function](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L36) or a [static class method](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L32). In the example below, [@Home\Controller.test](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L10) will call the [home controller](https://github.com/mvc5/mvc5-application/blob/master/src/Home/Controller.php) [static test](https://github.com/mvc5/mvc5-application/blob/master/src/Home/Controller.php#L30) method.

Constraints have named keys that match their corresponding [regex](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L209) parameter and optional parameters are enclosed with square brackets `[]`. This implementation is from the [DASPRiD/Dash](https://github.com/DASPRiD/Dash) project.

Custom [definitions](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) can also be configured by adding its [class name](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition/Build.php#L75) to the array, or the configuration itself can be a [definition](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) object.   

```php
return [
    'name'       => 'home', //for the url plugin in view templates
    'route'      => '/',
    'class'      => RouteDefinition::class, //optional
    'controller' => 'Home\Controller', //callable
    'controller' => '@Home\Controller.test', //callable
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
                    'route'      => '/:author[/:category]',
                    'defaults'   => [
                        'author'   => 'owner',
                        'category' => 'web'
                    ],
                    'wildcard'   => false,
                    'controller' => 'blog:add', //call event
                    //'controller'  => function($request) { //named args
                        //var_dump($request->getPathInfo());
                    //},
                    'constraints' => [
                        'author'   => '[a-zA-Z0-9_-]*',
                        'category' => '[a-zA-Z0-9_-]*'
                    ]
                ]
            ],
        ]
    ]
];
```

Route [definition](https://github.com/mvc5/mvc5/blob/master/src/Route/Definition.php) names are used by the url [route generator](https://github.com/mvc5/mvc5/blob/master/src/Route/Generator.php), e.g

```php
echo $this->url('blog/create', ['author' => 'owner', 'category' => 'oop']);
```

If the [router](https://github.com/mvc5/mvc5/blob/master/src/Route/Router/Router.php) does not return a [matched route](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L43), a [route error event](https://github.com/mvc5/mvc5/blob/master/config/event.php#L32) is used to [update the response status](https://github.com/mvc5/mvc5/blob/master/src/Response/Status.php) and to return the [current route](https://github.com/mvc5/mvc5-application/blob/master/config/service.php#L84) with the [name of the error controller](https://github.com/mvc5/mvc5/blob/master/src/Route/Error/Create.php#L39) to [use](https://github.com/mvc5/mvc5/blob/master/config/service.php#L23).

```php
'route\dispatch' => [
    'route\filter',
    'router',
    'route\error'
]
```
 
