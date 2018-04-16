## Features
* [Action Controller](#action-controller)
* [Autowiring](#autowiring)
* [Configuration](#configuration-and-arrayaccess)
* [Console Applications](#console-application)
* [Dependency Injection](#dependency-injection)
* [Environment Aware](#environment-aware)
* [Events](#events)
* [Maintainability](#maintainability)
* [Named Arguments](#named-arguments)
* [Plugins](/plugins)
* [Routes](#routes)
* [REST API Methods](#rest-api-methods)
* [Middleware](#middleware)
* [View Models](#view-models)

All of the components require [dependency injection](#dependency-injection) and use [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) objects for consistency and ease of use. For example, the [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) uses it to manage its service objects and has a [configuration array](https://github.com/mvc5/mvc5/blob/master/config/service.php) that can contain real values, string names, [callable types](http://php.net/manual/en/language.types.callable.php) and [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) [plugins](https://github.com/mvc5/mvc5/tree/master/src/Plugin). Consequently, they can all be considered as a type of plugin. 
