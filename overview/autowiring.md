### Autowiring
Autowiring occurs when a class is constructed and only some or none of its required parameters have been provided. A callback function is used to create the missing required parameters (and any of their required parameters). The callback function will use the type hint (class or interface) name as the dependency to create. If the result is null, the parameter name will be used instead and an exception will be thrown when the required parameter cannot be created. A service manager can be used as the callback function for [dependency injection](#dependency-injection).


