## Console Application
A [console application](https://github.com/mvc5/mvc5-application/blob/master/app.php) can be created by passing [command line arguments](http://php.net/manual/en/reserved.variables.argv.php) to the [service](https://github.com/mvc5/mvc5/blob/master/src/Service/Service.php) [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22) method.
```php
./app.php 'Console\Example' Monday January
```
```php
(new App('./config/config.php'))->call($argv[1], array_slice($argv, 2));
```
The first argument is the name of the function to [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22) and the remaining arguments are its parameters, e.g [<code>Console\Example</code>](https://github.com/mvc5/mvc5-application/blob/master/src/Console/Example.php).
```php
namespace Console;

use Home\ViewModel;

class Example
{
    protected $model;

    function __construct(ViewModel $model)
    {
        $this->model = $model;
    }

    function __invoke($day, $month)
    {
        echo $this->model->message . ': ' . $day . ' ' . $month . "\n";
    }
}
```
An [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) can also work without a configuration. 
```php
(new App)->call($argv[1], array_slice($argv, 2));
```
Read more about <a href="#dependency-injection">dependency injection</a> and <a href="#autowiring">autowiring</a>.
