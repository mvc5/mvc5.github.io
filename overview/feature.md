
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

All of the components require [dependency injection](#dependency-injection) and use [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) objects for consistency and ease of use. For example, the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) is a [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object that manages its services via the standard configuration interface, and has additional [service container](https://github.com/mvc5/framework/blob/master/src/Service/Container/ServiceContainer.php) methods that manage the underlying configurations of the services that it provides. The main [configuration array](https://github.com/mvc5/framework/blob/master/config/service.php) can contain values, string names, callables and configuration objects that are resolvable by the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php).
