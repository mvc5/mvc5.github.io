## Named Arguments
The [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L21) method can [invoke](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L78) a function with [named arguments](https://en.wikipedia.org/wiki/Named_parameter) when they are [named](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L22) or none are provided. 
```php
$this->call(Arg::VIEW_RENDER, [Arg::MODEL => $model] + $args);
```
This allows a function to be called without having to provide all of its required parameters. Typically an exception would be [thrown](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L69), but before it [occurs](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L72), a callback function can be [used](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L59) to provide the missing arguments. Consequently, a service manager can provide [itself](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L80) as the callback function to use.
```php
$this->signal($config, $args, $callback ?? $this);
```
The [signal](https://github.com/mvc5/mvc5/blob/master/src/Signal.php) method reserves the argument ```$argv``` to [contain](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L45) the remaining named arguments similar to a [variadic](http://php.net/manual/en/functions.arguments.php#functions.variable-arg-list) trailing argument.
```php
function($controller = null, array $argv = [])
```
When argv is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L41) as a variadic trailing argument, the remaining named arguments are stored in a SignalArgs class that the function can use to retrieve the remaining named arguments.
 