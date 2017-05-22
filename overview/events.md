## Events
An [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) is a function. However, instead of being implemented as a single function, it is implemented across multiple functions; which can be easily extended via its [configuration](https://github.com/mvc5/mvc5/blob/master/config/event.php).       

```php
function action($controller, array $args = [])
{
    return $this->call($controller, $args);
}
```

The [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L21) function [can](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L28) also [generate](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L31) an [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php). However, sometimes it may be preferable to pass the event arguments directly to its constructor, in which case the [trigger](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L87) method can be used.


```php
function match($route, $request)
{
    return $this->trigger([Arg::ROUTE_MATCH, Arg::ROUTE => $route, Arg::REQUEST => $request]);
}
```

When the [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L21) method is used to [generate](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L31) an [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) that does not have a plugin configuration, an instance of the [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php) will be [created](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L59). This allows a common model parameter to be used by the functions of the [event](https://github.com/mvc5/mvc5/blob/master/src/Event.php) to contain a value that is not null.

```php
function __invoke(callable $callable, array $args = [], callable $callback = null)
{
    $model = $this->signal(
        $callable, 
        !$args ? $this->args() : (!is_string(key($args)) ? $args : $this->args() + $args), $callback
    );

    null !== $model
        && $this->model = $model;

    return $model;
}
```

### Event Configuration
Events are <a href="https://github.com/mvc5/mvc5/blob/master/config/event.php">configurable</a> and can be an array or an [iterator](http://php.net/manual/en/class.iterator.php). Each item returned must [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L468) to a [callable](http://php.net/manual/en/language.types.callable.php) type.

```php
'web' => [
    'route\dispatch',
    'request\error',
    'request\service',
    'controller\action',
    function($response) { //named args
        var_dump(__FILE__, $response);
    },
    'view\layout',
    'view\render',
    'response\status',
    'response\version',
    'response\send'
],
```
