## Console Application
A simple [console application](https://github.com/mvc5/mvc5-application/blob/master/src/Console/Example.php) can be created by passing the [command line arguments](https://github.com/mvc5/mvc5-application/blob/master/app.php) to the [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L67) method.

```php
./app.php 'Console\Example' Monday January
```

```php
include './init.php';

(new App('./config/config.php'))->call($argv[1], array_slice($argv, 2));
```

The first argument is the name of the function or object to invoke and the remaining arguments are its parameters.

```php
namespace Console;

use Home\Model;

class Example
{
    protected $model;
    
    public function __construct(Model $model)
    {
        $this->model = $model;
    }
    
    public function __invoke($day, $month)
    {
        var_dump($this->model);
        echo $day . ' ' . $month . "\n";
    }
}
```

An [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) can also work without any configuration.

```php
(new App)->call($argv[1], array_slice($argv, 2));;
```

Read more about <a href="#dependency-injection">dependency injection</a>, <a href="#autowiring">autowiring</a> and <a href="#named-arguments">named arguments</a> for how the required arguments of a function can be resolved. Note that it is possible to create console applications similar to a [web application](#web-application) with [routes](#routes) and controllers.
