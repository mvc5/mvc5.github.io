## Configuration and ArrayAccess
A standard [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) interface is used consistently throughout each component and provides a concrete set of methods to use. Its [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface enables the [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) to [retrieve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L224) composite configuration values. E.g.

```php
new Param('templates.error');
```

Resolves to

```php
$config['templates']['error'];
```

Which makes it possible to use an array or a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) object when [references](http://php.net/manual/en/language.references.php) are needed, e.g [templates](https://github.com/mvc5/mvc5-application/blob/master/config/config.php).

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

Implementing the [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) interface allows components to only need to specify their immutable methods.

```php
interface Route
    extends Configuration
{
    function controller();
    function path();
}
```

[Constants](https://github.com/mvc5/mvc5/blob/master/src/Arg.php) can be used by other components to update a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) object via its [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface.

```php
$route[Arg::PATH] = '/home';
//or
$route->set(Arg::PATH, '/home');
```
