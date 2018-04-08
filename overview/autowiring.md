### Autowiring
Autowiring occurs when a class is [constructed](https://github.com/mvc5/mvc5/blob/master/src/Service/Builder.php#L35) and only some or none of its required parameters have been provided. A callback function is used to create the missing required parameters (and their required parameters). The callback function will use the type hint (class or interface) name, or the parameter name, as the dependency to create. If the result is null, an exception will be thrown. A service manager can be used as the callback function for [dependency injection](#dependency-injection).


