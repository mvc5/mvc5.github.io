## Configuration and ArrayAccess
A standard [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface is used consistently throughout each component in order to provide a standard set of *concrete* configuration methods. Its [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface enables the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) to [retrieve](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L276) nested configuration values, e.g.

```php
new Param('templates.error');
```

Resolves to

```php
$config['templates']['error'];
```

Which makes it possible to use either an array or a [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object when [references](http://php.net/manual/en/language.references.php) are needed, e.g [templates and aliases](https://github.com/mvc5/framework/blob/master/config/config.php#L13).

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

Implementing the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface allows components to only have to specify their immutable methods and allows the component to choose whether or not to extend the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface or to implement it separately. The idea is that most of the time, only the immutable methods are of interest and the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) interface provides a consistent way of instantiating it.

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

Constants can be used by other components to update the [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object via [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php).

```php
$route[Route::PATH] = '/home';
//or
$route->set(Route::PATH, '/home');
```
