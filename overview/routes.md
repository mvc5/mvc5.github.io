## Routes
A [route](https://github.com/mvc5/framework/blob/master/src/Route/Route.php) [definiton](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php) is a [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object that can also be [configured](https://github.com/mvc5/application/blob/master/config/route.php) as an array, and contains the properties required to [match](https://github.com/mvc5/framework/blob/master/config/event.php#L30) a [route](https://github.com/mvc5/framework/blob/master/src/Route/Route.php). Before [matching](https://github.com/mvc5/framework/blob/master/src/Route/Match/Match.php), if the [definiton](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php) does not have a [regex](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php#L56), it will be [compiled](https://github.com/mvc5/framework/blob/master/src/Route/Builder/Base.php#L87) against the [route](https://github.com/mvc5/framework/blob/master/src/Route/Route.php)'s request uri [path](https://github.com/mvc5/framework/blob/master/src/Route/Route.php#L51). Each aspect of [matching](https://github.com/mvc5/framework/blob/master/src/Route/Match/Match.php) a [route](https://github.com/mvc5/framework/blob/master/src/Route/Route.php) has a dedicated function, e.g. [scheme](https://github.com/mvc5/framework/blob/master/src/Route/Match/Scheme/Scheme.php), [hostname](https://github.com/mvc5/framework/blob/master/src/Route/Match/Hostname/Hostname.php), [path](https://github.com/mvc5/framework/blob/master/src/Route/Match/Path/Path.php), [method](https://github.com/mvc5/framework/blob/master/src/Route/Match/Method/Method.php), [wildcard](https://github.com/mvc5/framework/blob/master/src/Route/Match/Wildcard/Wildcard.php), and any other function can be [configured](https://github.com/mvc5/application/blob/master/config/route.php) for the [route match event](https://github.com/mvc5/framework/blob/master/src/Route/Match/Match.php).

In order to [build](https://github.com/mvc5/framework/blob/master/src/Route/Generator/Generator.php#L47) a url using the [route plugin](https://github.com/mvc5/framework/blob/master/src/Route/Plugin/Plugin.php), e.g. as a [view helper plugin](https://github.com/mvc5/framework/blob/master/src/View/ViewPlugin.php), the [base route](https://github.com/mvc5/application/blob/master/config/route.php#L7) must have a name, which is typically the homepage for /, e.g [home](https://github.com/mvc5/application/blob/master/config/route.php#L7), or it can specify its own, e.g /application. [Child](https://github.com/mvc5/application/blob/master/config/route.php#L10) routes except for the first level, will automatically have their parent name [prepended](https://github.com/mvc5/framework/blob/master/src/Route/Router/Router.php#L61) to their name, e.g blog/create. First level routes will not have the [base route](https://github.com/mvc5/application/blob/master/config/route.php#L7) prepended as it keeps their name simpler when [specifying](https://github.com/mvc5/application/blob/master/view/blog/create.phtml#L2) which [route definition](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php) to [build](https://github.com/mvc5/framework/blob/master/src/Route/Generator/Generator.php#L47), e.g. blog instead of home/blog.

The [controller](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php#L26) param must be a service configuration value (which includes real values) that must [resolve](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L182) to a [callable](http://php.net/manual/en/language.types.callable.php) type. If no [service configuration](https://github.com/mvc5/application/blob/master/config/service.php) for the controller exists but its class does, a new instance will be created with [constructor autowiring](#constructor-autowiring).

Controller configurations that are prefixed with an [@](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Args.php#L18) will be [called](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63), so they must be resolve to callable type or be an event. In the example below, [@Blog.test](https://github.com/mvc5/application/blob/master/config/route.php#L13) will call the [test](https://github.com/mvc5/application/tree/master/src/Blog/Controller.php) method on a shared instance of the [Blog](https://github.com/mvc5/application/tree/master/src/Blog/Controller.php) controller. [@blog.remove](https://github.com/mvc5/application/blob/master/config/route.php#L15) will call the [blog:remove](https://github.com/mvc5/application/blob/master/config/event.php#L13) event. And [@blog:create](https://github.com/mvc5/application/blob/master/config/alias.php#L9) is an [alias](https://github.com/mvc5/application/blob/master/config/alias.php#L9) to a [blog create](https://github.com/mvc5/application/blob/master/src/Blog/Create.php) event.

Constraints have named keys that match their corresponding [regex](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php#L56) parameter and optional parameters are enclosed with square brackets `[]`. This implementation is from the [DASPRiD/Dash](https://github.com/DASPRiD/Dash) router project.

```php
return [
    'name'       => 'home', //for the url plugin in view templates
    'route'      => '/',
    'controller' => 'Home\Controller', //callable
    'children' => [
        'blog' => [
            'route'      => 'blog',
            'controller' => '@Blog.test', //specific method
            'children' => [
                'remove' => [
                    'route' => '/remove',
                    'controller' => '@blog:remove'
                ],
                'create' => [
                    'route'      => '/:author[/:category]',
                    'defaults'   => [
                        'author'   => 'owner',
                        'category' => 'web'
                    ],
                    'wildcard'   => false,
                    'controller' => '@blog:create', //call event
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

Route [definition](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php) names are used by the url [route generator](https://github.com/mvc5/framework/blob/master/src/Route/Generator/Generator.php), e.g

```php
echo $this->url('blog/create', ['author' => 'owner', 'category' => 'oop']);
```
