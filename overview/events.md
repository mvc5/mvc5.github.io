## Events
An [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) is a function. However, instead of being implemented as a single function, it is implemented across multiple functions and can be easily extended via its [configuration](https://github.com/mvc5/mvc5/blob/master/config/event.php). Events can also control the parameters that are provided to its functions and the outcome of each function.       
```php
function __invoke(callable $callable, array $args = [], callable $callback = null)
{
    $model = $this->signal($callable, $this->args() + $args, $callback);

    null !== $model
        && $this->model = $model;

    return $model;
}
```
For example, the <code>dashboard:remove</code> event uses three functions to create a model and to return a [layout](https://github.com/mvc5/mvc5/blob/master/src/ViewLayout.php) object. It does not have its own [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) class, so an instance of the [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php) is used. 
```php
'dashboard:remove' => [
    function() {
        return $model = '<h1>Validate</h1>';
    },
    function($model) {
        return $model . '<h1>Remove</h1>';
    },
    function(TemplateLayout $layout, $model = null) {
        $model .= '<h1>Respond</h1>';

        return $layout->withModel($model);
    }
]
```
The [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php) will store the result of the first function, if it is not null, and pass it as the value of the model parameter of the second function. If the first function had required the model parameter, its value would of been null; because no value was given as an argument to the [event](https://github.com/mvc5/mvc5/blob/master/src/Event.php) and no [plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php) exists for the name <code>model</code>. In this example, the model parameter is a string, so the second function appends to it and returns it as the value of the model parameter of the third function. When the third function is invoked, the [signal](https://github.com/mvc5/mvc5/blob/master/src/Signal.php) method recognizes that the [layout](https://github.com/mvc5/mvc5/blob/master/config/service.php#L32) parameter is missing and [uses](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L68) the callback function to create it. As the model parameter is an argument of the [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php), it can also be used as an optional parameter.
```php
$app->call(new Event('route\match'), [$route, $request]);
```
The [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22) function [can](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L28) also [generate](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L32) an [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php). However, sometimes it may be preferable to pass the event arguments directly to its constructor, in which case the [trigger](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L91) method can be used.
```php
function match(Route $route, Request $request)
{
    return $this->trigger([Arg::ROUTE_MATCH, Arg::ROUTE => $route, Arg::REQUEST => $request]);
}
```
### Event Configuration
Events are <a href="https://github.com/mvc5/mvc5/blob/master/config/event.php">configurable</a> and can be an array or an [iterator](http://php.net/manual/en/class.iterator.php). Each item returned must [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L479) to a [callable](http://php.net/manual/en/language.types.callable.php) type.
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
