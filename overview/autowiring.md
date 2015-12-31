## Autowiring
The required arguments of a class constructor are automatically resolved by a [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) when it [instantiates](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L124) a class that

* does not have a service [plugin](#plugins) configuration,
* or no arguments are passed to the [plugin](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L251) method,
* or the arguments are <a href="#named-arguments">named</a>.

The [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) will resolve the required constructor arguments either by their type hint, or parameter name. 

For a function or class method being invoked by the [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L67) method, the missing required arguments are also automatically [resolved](https://github.com/mvc5/mvc5/blob/master/src/Signal.php). However, they are [resolved](https://github.com/mvc5/mvc5/blob/master/src/Signal.php) in the opposite order, first  by their parameter name, then by their type hint. Otherwise, an exception will be [thrown](https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L72) since the required parameter has not been provided.

