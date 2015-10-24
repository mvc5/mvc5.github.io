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

The application is instantiated and a [call](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63) is made to the valid method of the controller class with its parameters resolved from either the array of arguments explicitly passed to the [call](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L58) method or by the [call](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L58) method retrieving a plugin with the same name as the parameter. Methods can be chained together and each will have their parameters resolved similarly.

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

The parameter $args can be used as a named argument that provides an array of the named arguments available to that method.

To manage all of the parameters, an optional callback can be added to the call method, e.g

```php
$response = $web->call(
    'Controller.valid.add.response',
    [],
    function($name) { return new $name; }
);
```
