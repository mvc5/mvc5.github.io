### Service Container
Service configurations and [plugins](/plugins) can be combined and accessed using the [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface or the [arrow](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L106) notation, e.g. <code>dashboard->home</code>.
```php
use Mvc5\App;
use Mvc5\Plugin\Plugins;
use Mvc5\Plugin\Value;

$app = new App([
    'services' => [
        'dashboard' => new Plugins(
            [
                'home' => fn($template) => fn($view, $form) => 
                    $this->call($view, [$template, $form + ['message' => 'Demo Page']]),
                'template' => new Value('dashboard/index')
            ],
            new Link, //reference to parent container
            true //use current container as the scope for anonymous functions
        ),
        'view' => fn() => function($template, $var) {
                include $template;
        },
    ]
]);

//$app['dashboard']['home'];
//$app['dashboard->home'];

$app->call('dashboard->home', ['form' => []]);
```
A [container](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Container.php) can contain any type of value, except for [null](http://php.net/manual/en/language.types.null.php); in which case the [null value](/plugins/#nullvalue) plugin can be used. A parent container can also pass itself to a child container as the service [provider](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L73) to use when the child container cannot retrieve or resolve a particular value. The parent container can also configure the [scope](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L75) of an anonymous function within the child container.
