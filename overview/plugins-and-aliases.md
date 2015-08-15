## Plugins and Aliases
The parameter names of the additional arguments can be aliases or service names. An alias maps a string of varying characters excluding the [call separator](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Args.php#L23) `.` to any positive value. If the value is a [service configuration](https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php) object, then it will be resolved and its value returned.
Each plugin has a configuration specific to its own use and they are resolved each time they are used. This enables them to be used in various ways for different purposes, e.g to provide a value, or to trigger an event, or to call a particular service method.

```php
return [
    'blog:create'  => new Service('Blog\Create'),
    'config'       => new Dependency('Config'),
    'layout'       => new Dependency('Layout'),
    'request'      => new Dependency('Request'),
    'response'     => new Dependency('Response'),
    'route:create' => new Dependency('Route\Create'),
    'sm'           => new Dependency('Service\Manager'),
    'url'          => new Dependency('Route\Plugin'),
    'web'          => new Service('Mvc'),
    'vm'           => new Dependency('View\Manager')
];
```

Note that the [plugin](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ManageService.php#L63) method is used when calling an object.

```php
$this->call('blog:create');
```

Which means invoking a web application is no different to calling any other method, e.g

```php
$app = new Application($config);

$app->call('web'); //invoke web application (event)
```

And

```php
$app = new Application($config);

$app->call('request.getHost'); //get string hostname from the request object.
```

And with named arguments

```php
$app->call('Home\Factory', ['config' => $config, 'vm' => $vm]);
```

To get all of the available arguments that are not plugin arguments, add $args to the method signature

```php
public function __invoke(Config $config, ViewManager $vm, array $args = [])
{
    var_dump($args);
}
```
