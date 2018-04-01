## Models and ArrayAccess
Value objects use a [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface to provide a common set of access methods.
```php
namespace Mvc5\Config;

interface Model
    extends \ArrayAccess, \Countable, \Iterator
{
    /**
     * @param array|string $name
     */
    function get($name);

    /**
     * @param array|string $name
     */
    function has($name) : bool;

    /**
     * @param array|string $name
     * @param mixed $value
     */
    function with($name, $value = null);

    /**
     * @param array|string $name
     */
    function without($name);
}
```
The [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface is then extended to provide a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) interface containing the methods [set](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php#L22) and [remove](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php#L15).
```php
namespace Mvc5\Config;

interface Configuration
    extends Model
{
    /**
     * @param array|string $name
     */
    function remove($name);
    
    /**
     * @param array|string $name
     * @param mixed $value
     */
    function set($name, $value = null);
}
```
### Set and Remove
Multiple values can be [set](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L64) or [removed](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L52) by using an array of key values or keys.
```php
$config->set(['foo' => 'bar', 'baz' => 'bat']);
$config->remove(['foo', 'baz']);
```
### Immutable
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
        throw new \Exception;
    }

    function offsetUnset($name)
    {
        throw new \Exception;
    }

    function __set($name, $value)
    {
        throw new \Exception;
    }

    function __unset($name)
    {
        throw new \Exception;
    }
}
```
Implementing the [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface allows a component to only have to specify its immutable methods.
```php
interface Route
    extends Model
{
    function controller();
    function path();
}
```
### With and Without
A copy of the model can be created [with](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L82) and [without](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L97) specific values. It also accepts an array of key values so that only one clone operation is performed.
```php
$this->with(['foo' => 'bar', 'baz' => 'bat']);
$this->without(['foo', 'baz']);
```
### ArrayAccess 
[Models](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) support the [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface. This, for example, enables the [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) to [retrieve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L321) composite values. E.g.
```php
new Param('templates.error');
```
Resolves to
```php
$config['templates']['error'];
```
This makes it possible to use an array or a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) class when a [reference](http://php.net/manual/en/language.references.php) is required.
### Polymorphism
[Models](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) can become mutable by applying their traits to an instance of a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config.php) object. 
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
