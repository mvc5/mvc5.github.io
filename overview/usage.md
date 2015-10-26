## Usage
The <a href="https://github.com/mvc5/application">mvc5/application</a> demonstrates its usage as a [web application](#web-application).

```php
include __DIR__ . '/../vendor/autoload.php';
```

```php
$config = [
    'alias'     => include __DIR__ . '/alias.php',
    'events'    => include __DIR__ . '/event.php',
    'services'  => include __DIR__ . '/service.php',
    'routes'    => include __DIR__ . '/route.php',
    'templates' => include __DIR__ . '/templates.php'
];
```

```php
(new App(include __DIR__ . '/../config/config.php'))->call('web');
```

## Web Application
A default [configuration](https://github.com/mvc5/framework/blob/master/config/config.php) is provided with the minimum [configuration](https://github.com/mvc5/framework/blob/master/config) required to run a web application. Then, all that is required are the [request](https://github.com/mvc5/application/blob/master/config/service.php#L30) and [response](https://github.com/mvc5/application/blob/master/config/service.php#L32) objects, [template](https://github.com/mvc5/application/blob/master/config/templates.php) configuration and the [routes](https://github.com/mvc5/application/blob/master/config/route.php) to use. [Routes](#routes) must have a name, so that they can be used to [build](https://github.com/mvc5/framework/blob/master/src/Route/Generator/Generator.php#L47) urls in the [view templates](https://github.com/mvc5/application/tree/master/view) with the [url plugin](https://github.com/mvc5/framework/blob/master/config/alias.php#L19).

```php
new App(include __DIR__ . '/../vendor/mvc5/framework/config/config.php')->call('web');
```

## Console Application
A simple [console application](https://github.com/mvc5/application/blob/master/src/Console/Example.php) can be created by passing the [command line arguments](http://php.net/manual/en/reserved.variables.argv.php) to the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) [call](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63) method.

```php
./app.php 'Console\Example' Monday January
```

```php
include './init.php';

(new App('./config/config.php'))->call($argv[1], array_slice($argv, 2));
```

The first argument is the name of the function and the remaining arguments are its parameters.

```php
namespace Console;

use Mvc5\View\Model\ViewModel;

class Example
{
    protected $model;
    
    public function __construct(ViewModel $model)
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

Read more about <a href="#dependency-injection">dependency injection</a> and <a href="#constructor-autowiring">constructor autowiring</a> for how the dependencies of a function can be resolved. Note, that it is also possible to create a console application similar to a [web application](#web-application) with [routes](#routes) and controllers.

## Environment Aware Configurations
Development configurations can override production values using [array_merge](http://php.net/manual/en/function.array-merge.php) since each [configuration](https://github.com/mvc5/application/blob/master/config/config.php) file returns an array of values. E.g

```php
return array_merge(
    include __DIR__ . '/../config.php',
    [
        'db_name' => 'dev'
    ]
);
```

In the above example, the development configuration file <mark>config/dev/config.php</mark> includes the main production file <mark>config/config.php</mark> and overrides the name of the database to use in development.
