## Routes
A route can be configured as an `array` or as a [route definition](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php). If the configuration does not have a `regex`, then it will be compiled before matching against the request uri path. Each aspect of matching a route has a dedicated function, e.g. scheme, hostname, path, method, wildcard, and any other function can be configured to be called in the [route match event](https://github.com/mvc5/framework/blob/master/src/Route/Match/Match.php).

In order to create a url using the [route plugin](https://github.com/mvc5/framework/blob/master/src/Route/Plugin/Plugin.php), e.g a view helper plugin, the first route must have a name that can be referred to as the base route, which is typically the homepage for `/`, e.g `home`, or it can specify its own, e.g `/home`. Child routes, except for the first level, will automatically have their parent name prepended to their name e.g `application/dashboard`. First level routes will not have the parent route prepended as it keeps their name simpler to use when specifying which route to create e.g `application` instead of `home/application`.

The [controller](https://github.com/mvc5/framework/blob/master/src/Route/Definition/Definition.php#L26) param must be a service configuration value (which includes real values) that must resolve to a callable type. In the example below, `@Home.test` will call the `test` method on a shared instance of `Home`. If no configuration for `Home` exists, a new instance will be created via [Constructor Autowiring](#Constructor Autowiring).

Controller configurations that are prefixed with an `@` will be called as a plugin, so its `alias` configuration must resolve to a callable type. In the example below, `@blog:create` is an `alias` to a `Blog Create Event` and is triggered as an event instead calling a single method.

Constraints have named keys that match the name of its corresponding `regex` parameter, optional parameters are enclosed with the square brackets `[]`. This implementation is from the [DASPRiD/Dash](https://github.com/DASPRiD/Dash) router project.

Route definitions implement the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface which enables each definition to have their own set of configuration values that can then be used by any function called during the [route match event](https://github.com/mvc5/framework/blob/master/src/Route/Match/Match.php).

```php
return [
    'name'       => 'home', //name required for url generator
    'route'      => '/',
    'controller' => 'Home',
    'children' => [
        'application' => [
            'route'      => '/application',
            'controller' => '@Home.test',
            'children'   => [
                'default' => [
                    'route'      => '/:sort[/:order]',
                    'controller' => '@blog:create', //call event (trigger)
                        //'controller'  => function($model) { //named args
                        //return $model;
                    //},
                    'constraints' => [
                        'sort'  => '[a-zA-Z0-9_-]*',
                        'order' => '[a-zA-Z0-9_-]*'
                    ]
                ]
            ],
        ]
    ]
];
```

The route names are used by the url [route generator](https://github.com/mvc5/framework/blob/master/src/Route/Generator/Generator.php), e.g

```php
echo $this->url('application/default', ['sort' => 'name', 'order' => 'desc']);
```
