## Named Arguments and Plugins
This contrived example demonstrates named arguments and plugins.

```php
$web = new App(include __DIR__ . '/../config/config.php');

$response = $web->call(
    'Controller.valid.add.response',
    ['date_created' => time(), 'strict' => true]
);

var_dump($response instanceof Response);
```

The application is instantiated and a call is made to the `valid` method of the `Controller` class with its parameters resolved from either the array of arguments explicitly passed to the [`call`](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L58) method or by the [`call`](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L58) method retrieving a plugin with the same name as the parameter. Methods can be chained together and each will have their parameters resolved similarly.

```php
class Controller
{
    protected $blog;

    function valid(Request $request, $strict)
    {
        var_dump($strict);
        
        return $this;
    }
    
    function add(Response $response, $date_created)
    {
        var_dump($date_created);
        
        $this->blog = new Blog;
        
        return $this;
    }
    
    function response(ViewManager $vm, Response $response, $args)
    {
        var_dump($this->blog, $args);
        return $response;
    }
}
```

The output of the above is ...

```php
boolean true

int 1414690433

object(Blog\Blog)[100]

array (size=2)
'date_created' => int 1414690433
'strict' => boolean true

boolean true
```

The parameter `$args` can be used as a `named argument` that provides an `array of the named arguments` available to that method.

To manage all of the parameters, an optional callback can be added to the call method, e.g

```php
$response = $web->call(
    'Controller.valid.add.response',
    [],
    function($name) { return new $name; }
);
```

## Plugins and Aliases
The parameter names of the additional arguments can be aliases or service names. An alias maps a string of varying characters excluding the [call separator](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Args.php#L23) `.` to any positive value. If the value is a [service configuration](https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php) object, then it will be resolved and its value returned.
Each plugin has a configuration specific to its own use and they are resolved each time they are used. This enables them to be used in various ways for different purposes, e.g to provide a value, or to trigger an event, or to call a particular service method.

```php
return [
    'blog:create'  => new Service('Blog\Create'),
    'blog:valid'   => new Invoke('Blog\Controller\Validate', ['model' => new Model('blog:create')]),
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
//trigger create blog event
$this->call('blog:create');

//call the controller's valid method with supporting arguments
$this->call('blog:valid');

function valid(Config $config, Request $request);
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
$app->call(
    'Blog\Controller.valid',
    ['config' => $config, 'request' => $request]
);
```

To get all of the available arguments that are not plugin arguments, add `$args` to the method signature

```php
public function __invoke(Config $config, ViewManager $vm, array $args = [])
{
    var_dump($args);
}
```
