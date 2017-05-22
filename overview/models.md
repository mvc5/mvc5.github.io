## Models and ArrayAccess
Value objects use a [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface to provide a common set of access methods.
```php
namespace Mvc5\Config;

interface Model
    extends \ArrayAccess, \Countable, \Iterator
{
    function get($name);
    function has($name);
    function with($name, $value = null);
    function without($name);
}
```

The [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface is then extended to provide a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) interface containing the methods [set](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php#L22) and [remove](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php#L15).
```php
namespace Mvc5\Config;

interface Configuration
    extends Model
{
    function remove($name);
    function set($name, $value = null);
}
```

By [protecting](https://github.com/mvc5/mvc5/blob/master/src/Config/ReadOnly.php) access to the mutable [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) methods and object [magic methods](http://php.net/manual/en/language.oop5.magic.php) an [immutable](https://github.com/mvc5/mvc5/blob/master/src/Config/Immutable.php) interface can be implemented.
```php
namespace Mvc5\Config;

trait ReadOnly
{
    use Config {
        remove as protected;
        set as protected;
    }

    function offsetSet($name, $value)
    {
        throw new \Exception('Invalid operation: object cannot be modified');
    }

    function offsetUnset($name)
    {
        throw new \Exception('Invalid operation: object cannot be modified');
    }

    function __set($name, $value)
    {
        throw new \Exception('Invalid operation: object cannot be modified');
    }

    function __unset($name)
    {
        throw new \Exception('Invalid operation: object cannot be modified');
    }
}
```

Where possible, all of the Mvc5 value objects are [immutable](https://github.com/mvc5/mvc5/blob/master/src/Config/Immutable.php). 

The [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface also enables the [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) to [retrieve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L321) composite configuration values. E.g.

```php
new Param('templates.error');
```

Resolves to

```php
$config['templates']['error'];
```

This makes it possible to use an array or a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) object when [references](http://php.net/manual/en/language.references.php) are needed. Implementing the [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface allows a component to only have to specify its immutable methods.

```php
interface Route
    extends Model
{
    function controller();
    function path();
}
```

[Constants](https://github.com/mvc5/mvc5/blob/master/src/Arg.php) can be used to reference specific keys when copying or updating an object.

```php
$request[Arg::NAME] = 'home';      //ArrayAccess
$request->set(Arg::NAME, 'home');  //Configuration
$request->with(Arg::NAME, 'home'); //Immutable
```

Models can also be made mutable by applying their traits to an instance of a configuration object. 

```php
class ViewModel
    extends \Mvc5\Config
    implements \Mvc5\View\ViewModel
{
    /**
     *
     */
    use \Mvc5\View\Config\ViewModel;
}
```
