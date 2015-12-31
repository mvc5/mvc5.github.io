## Named Arguments
The [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L67) method can [invoke](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L193) a function with [named arguments](https://en.wikipedia.org/wiki/Named_parameter) when they are [named](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L22) or none are provided. 

```php
$this->call(Arg::VIEW_RENDER, [Arg::MODEL => $model] + $args);
```

This allows a function to be called without having to provide all of its required parameters. Typically an exception would be [thrown](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L72), but before it [occurs](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L72), a callback function can be [used](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L62) to provide the missing arguments. Consequently, a service manager can provide [itself](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L195) as the callback function to use.

```php
$this->signal($config, $args, $callback ?? $this);
```

An [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) can also control the parameters that are provided to its functions, as well as the outcome of each function.

```php
function __invoke(callable $callable, array $args = [], callable $callback = null)
{
    $model = $this->signal($callable, $this->args() + $args, $callback);

    null !== $model
        && $this->model = $model;

    return $model;
}
```

For example, the <code>blog:remove</code> event uses three functions to create a model and to return a [layout](https://github.com/mvc5/mvc5/blob/master/config/service.php#L35) object. It does not have its own [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) class, so an instance of the [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php) is used. 

```php
'blog:remove' => [
    function() {
        return $model = '<h1>Validate</h1>';
    },
    function($model) {
        return $model . '<h1>Remove</h1>';
    },
    function($layout, $model = null) {
        $model .= '<h1>Respond</h1>';

        $layout->model($model);

        return $layout;
    }
]
```

The [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php) will store the result of the first function and pass it as the value of the model parameter of the second function. If the first function had required the model parameter, its value would of been null, because no value was given as an argument to the [event](https://github.com/mvc5/mvc5/blob/master/src/Event.php) and no [plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php) exists for the name <code>model</code>. In this example, the model parameter is a string, so the second function appends to it and returns it as the value of the model parameter of the third function. When the third function is [invoked](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L193), the [signal](https://github.com/mvc5/mvc5/blob/master/src/Signal.php) method recognizes that the [layout](https://github.com/mvc5/mvc5/blob/master/config/service.php#L35) parameter is missing and [uses](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L62) the callback function to create it. As the model parameter is an argument of the [default event model](https://github.com/mvc5/mvc5/blob/master/src/Event.php), it can also be used as an optional parameter.
