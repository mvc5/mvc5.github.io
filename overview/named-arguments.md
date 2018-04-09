## Named Arguments
The [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22) method can [invoke](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L82) a function with [named arguments](https://en.wikipedia.org/wiki/Named_parameter) when they are [named](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L19) or none are provided. 
```php
$this->call(Arg::VIEW_RENDER, [Arg::MODEL => $model] + $args);
```
This allows a function to be called without having to provide all of its required parameters. Typically an exception would be [thrown](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L78), but before it occurs, a callback function can be [used](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L68) to provide the missing arguments by using the parameter name or its type hint (class or interface) name. Consequently, a service manager can provide [itself](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L84) as the callback function to use.
```php
$this->signal($config, $args, $callback ?? $this);
```
The [signal](https://github.com/mvc5/mvc5/blob/master/src/Signal.php) method reserves the argument ```$argv``` to [contain](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L46) the remaining named arguments similar to a [variadic](http://php.net/manual/en/functions.arguments.php#functions.variable-arg-list) trailing argument.
```php
function($controller = null, array $argv = [])
```
When argv is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L42) as a variadic trailing argument, the remaining named arguments are stored in a [SignalArgs](https://github.com/mvc5/mvc5/blob/master/src/Plugin/SignalArgs.php) class that the function can use to [retrieve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L564) the remaining named arguments.
 