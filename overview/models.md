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
The [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface is then extended to provide a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) interface containing the [set](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php#L21) and [remove](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php#L14) methods.
```php
namespace Mvc5\Config;

interface Configuration
    extends Model
{
    /**
     * @param array|string $name
     */
    function remove($name) : void;
    
    /**
     * @param array|string $name
     * @param mixed $value
     */
    function set($name, $value = null);
}
```
### Set and Remove
Multiple values can be [set](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L71) or [removed](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L59) by using an array.
```php
$config->set('foo', 'bar');
$config->set(['foo' => 'bar', 'baz' => 'bat']);
$config->remove('foo');
$config->remove(['foo', 'baz']);
```
### Immutable
By [protecting](https://github.com/mvc5/mvc5/blob/master/src/Config/ReadOnly.php#L14) access to the mutable [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) methods and object [magic methods](http://php.net/manual/en/language.oop5.magic.php) an [immutable](https://github.com/mvc5/mvc5/blob/master/src/Config/Immutable.php) interface can be implemented.
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

    function offsetUnset($name) : void
    {
        throw new \Exception;
    }

    function __set($name, $value)
    {
        throw new \Exception;
    }

    function __unset($name) : void
    {
        throw new \Exception;
    }
}
```
Implementing the [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) interface allows a component to specify only its immutable methods.
```php
interface Route
    extends Model
{
    function controller();
    function path();
}
```
### With and Without
A copy of the model can be created [with](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L89) and [without](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php#L104) specific values. It also accepts an array of key values so that only one clone operation is performed.
```php
$this->with(['foo' => 'bar', 'baz' => 'bat']);
$this->without(['foo', 'baz']);
```
### ArrayAccess 
[Models](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) support the [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface. This, for example, enables the [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) to [retrieve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L361) composite values. E.g.
```php
new Param('templates.error');
```
Resolves to
```php
$config['templates']['error'];
```
This makes it possible to use an array or a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) class when a [reference](http://php.net/manual/en/language.references.php) is required.
### Polymorphism
Occasionally, a single instance of a [model](https://github.com/mvc5/mvc5/blob/master/src/Config/Model.php) is necessary within an [immutable](#immutable) system. For example, when rendering a view template that modifies a shared [layout](#template-layouts) model in order to set the title of the web page. In this case, a polymorphic model can be used to assign values directly to itself, instead of assigning them to a copy.  
```php
class SharedLayout
    extends Overload
    implements ViewLayout
{
    /**
     * @param array|string $name
     * @param mixed $value
     * @return ViewLayout
     */
    function with($name, $value = null) : ViewLayout
    {
        $this->set($name, $value);
        return $this;
    }
}
```
