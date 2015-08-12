## Usage
The <a href="https://github.com/mvc5/application">mvc5/application</a> demonstrates its usage as a web application.

```php
include __DIR__ . '/../vendor/autoload.php';
```

```php
use Mvc5\Config\Config;
use Mvc5\Service\Container\Container;

$config = [
    'alias'     => include __DIR__ . '/alias.php',
    'events'    => include __DIR__ . '/event.php',
    'services'  => new Container(include __DIR__ . '/service.php'),
    'routes'    => include __DIR__ . '/route.php',
    'templates' => include __DIR__ . '/templates.php'
];
```

```php
(new App(include __DIR__ . '/../config/config.php'))->call('web');
```

## Web Application
A default [configuration](https://github.com/mvc5/framework/blob/master/config/config.php) is provided with the minimum [configuration](https://github.com/mvc5/framework/blob/master/config) required to run a web application. Then, all that is required are the `Request` and `Response` objects, template configuration and the routes to use. Routes must have a name, so that they can be used to build urls in the view templates when using the [url plugin](https://github.com/mvc5/framework/blob/master/config/alias.php#L18).

```php
new App(include __DIR__ . '/../vendor/mvc5/framework/config/config.php')->call('web');
```

## Console Applications
A simple console application can be created by passing the command line arguments to the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) `call` method. E.g

```php
./app.php 'Console\Example' Monday January
```

```php
include './init.php';

(new App('./config/config.php'))->call($argv[1], array_slice($argv, 2));
```

The first argument is the name of the function and the remaining arguments are its parameters. E.g

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

See the section on <a href="#dependency-injection">Dependency Injection</a> and <a href="#constructor-autowiring">Constructor Autowiring</a> for information on how the dependencies of a function can be resolved.

Note that it is also possible to create a console application similar to a web application with routes and controllers.

## Environment Aware Configurations
Development configurations can override production values using `array_merge` since each configuration file returns an array of values. E.g

```php
return array_merge(
    include __DIR__ . '/../config.php',
    [
        'db_name' => 'dev'
    ]
);
```

In the above example, the development configuration file `config/dev/config.php` includes the main production file `config/config.php` and overrides the name of the database to use in development.
