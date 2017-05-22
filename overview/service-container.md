### Service Container
[Plugins](#plugins) can be grouped together and accessed via the [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface or using the [arrow](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L104) notation, e.g. <code>dashboard->home</code>.

```php
use Mvc5\App;
use Mvc5\Plugin\Plugins;
use Mvc5\Plugin\Value;

$app = new App([
    'services' => [
        'dashboard' => new Plugins(
            [
                'home' => function($template) {
                    return function($view, $form) use($template) {
                        return $this->call($view, [$template, $form + ['message' => 'Demo Page']]);
                    };
                },
                'template' => new Value('dashboard/index')
            ],
            new Link, //reference to parent container
            true //use current container as the scope for anonymous functions
        ),
        'view' => function() {
            return function($template, $var) {
                include $template;
            };
        },
    ]
]);

//$app['dashboard']['home'];
//$app['dashboard->home'];

$app->call('dashboard->home', ['form' => []]);
```

A container can contain any type of value, except for [null](http://php.net/manual/en/language.types.null.php). A parent container can also pass itself to a child container as the service [provider](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L71) to use when the child container can not retrieve or resolve a particular value. The parent container can also specify what object to use as the scope of an anonymous function within the child container.
