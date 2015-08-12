## Configuration and ArrayAccess
The [configuration](/mvc5/framework/blob/master/src/Config/Configuration.php) interface is used consistently throughout each component in order to provide a standard set of *concrete* configuration methods. Its [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface enables the [service manager](/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) to retrieve nested configuration values by making successive calls on the returned values. E.g

```php
new Param('templates.error');
```

Resolves to

```php
$config['templates']['error'];
```

Which makes it possible to use either an `array` or a [configuration](/mvc5/framework/blob/master/src/Config/Configuration.php) object when references are needed, e.g [templates and aliases](https://github.com/mvc5/framework/blob/master/config/config.php#L13).

```php
interface Configuration
    extends ArrayAccess
{
    function get($name);
    function has($name);
    function remove($name);
    function set($name, $config);
}
```

By implementing the [configuration](/mvc5/framework/blob/master/src/Config/Configuration.php) interface it allows components to only have to specify their *immutable* interface methods and allows the component to choose whether or not to extend the [configuration](/mvc5/framework/blob/master/src/Config/Configuration.php) interface or to implement it separately. The idea is that most of the time, only the *immutable* interface methods are of interest and the configuration interface provides a consistent way of instantiating its configuration.

```php
interface Route
    extends Configuration
{
    const CONTROLLER = 'controller';
    const PATH = 'path';
    function controller();
    function path();
}
```

Constants can be used by other components to update the configuration object via [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php).

```php
$route[Route::PATH] = '/home';
//or
$route->set(Route::PATH, '/home');
```
