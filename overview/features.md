## Features
* Configuration
* Maintainability
* Controller Dispatch
* Route Matching
* Response
* View
* Exceptions
* Dependency Injection
* Constructor Autowiring
* Plugins
* Configurable events and custom behaviours
* Calling methods using named arguments and plugin support
* Controller Action - a Middleware like event.
* Console Applications

All of the components require [dependency injection](#dependency-injection) and use [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) objects for consistency and ease of use. For example, the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) uses it to manage service objects, and has additional [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/ServiceContainer.php) methods for the underlying configuration of the services that it provides. The main [service configuration array](https://github.com/mvc5/framework/blob/master/config/service.php) can contain real values, string names, [callable types](http://php.net/manual/en/language.types.callable.php) and [configuration objects](https://github.com/mvc5/framework/tree/master/src/Service/Config) that are [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L323) by the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php).
