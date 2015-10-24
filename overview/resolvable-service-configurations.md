## Resolvable Service Configurations
Below are the default types of configurations that are available to use.

### [Args](https://github.com/mvc5/framework/blob/master/src/Service/Config/Args/Args.php)
```php
'Route' => [
    Route\Config::class,
    'config' => new Args([
        'hostname' => new Call('request.getHost'),
        'method'   => new Call('request.getMethod'),
        'path'     => new Call('request.getPathInfo'),
        'scheme'   => new Call('request.getScheme')
    ])
],
```

An [args](https://github.com/mvc5/framework/blob/master/src/Service/Config/Args/Args.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L366) to return an array of values that are resolved just in time, e.g. when the class is being instantiated or when a configuration is invoked. 

### [Call](https://github.com/mvc5/framework/blob/master/src/Service/Config/Call/Call.php)
```php
new Call('Home\Controller', ['response' => new Service('Response')])
```

The [call](https://github.com/mvc5/framework/blob/master/src/Service/Config/Call/Call.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L362) to invoke an object method or a callable string, e.g _request.getHost_ and supports [named arguments and plugins](#name-arguments-and-plugins). 

### [Calls](https://github.com/mvc5/framework/blob/master/src/Service/Config/Calls/Calls.php)
```php
'Mvc\Response' => new Calls(
    new Dependency(Mvc\Response\Dispatcher::class),
    ['setResponseManager' => new Dependency('Response\Manager')]
),
```

A [calls](https://github.com/mvc5/framework/blob/master/src/Service/Config/Calls/Calls.php) configuration is similar to a [hydrator](#hydratorhttpsgithubcommvc5frameworkblobmastersrcserviceconfighydratorhydratorphp) and is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L342) to resolve a service configuration that invokes a set of function calls with [named arguments and plugin](#name-arguments-and-plugins) support.   

### [Child](https://github.com/mvc5/framework/blob/master/src/Service/Config/Child/Child.php)
```php
'Controller'      => new Service(null, [new ServiceManagerLink]),
'IndexController' => new Child(IndexController::class, 'Controller'),
```

A [child](https://github.com/mvc5/framework/blob/master/src/Service/Config/Child/Child.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L346) to extend a parent configuration. The first parameter is the name of the class to create and the second parameter is the name of the parent configuration to use.

Custom child configurations can also be created. This allows another [config](#confighttpsgithubcommvc5frameworkblobmastersrcserviceconfigconfigphp) configuration, such as a [service](#servicehttpsgithubcommvc5frameworkblobmastersrcserviceconfigserviceservicephp) configuration, to be used without having to specify the name of its parent configuration. Examples are the [controller](#controllerhttpsgithubcommvc5frameworkblobmastersrcserviceconfigcontrollercontrollerphp), [factory](#factoryhttpsgithubcommvc5frameworkblobmastersrcserviceconfigfactoryfactoryphp), [form](#formhttpsgithubcommvc5frameworkblobmastersrcserviceconfigformformphp) and [manager](#managerhttpsgithubcommvc5frameworkblobmastersrcserviceconfigmanagermanagerphp) configurations. 

### [Config](https://github.com/mvc5/framework/blob/master/src/Service/Config/Config.php)
```php
new Config([
    'name'  => ''
    'args'  => [],
    'calls' => [],
    'merge' => false
])
```

A [config](https://github.com/mvc5/framework/blob/master/src/Service/Config/Config.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L350) to instantiate a class object by using an array to contain the properties of the service configuration. Its name property is a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration or class name, and optionally, can have an array of arguments for the class constructor and have an array of calls to invoke. It can also be used as a parent configuration to provide a set of default properties for child configurations. This allows the parent constructor arguments configuration to be used if none are provided by the child configuration and when both sets are named, they will be [merged](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L268) together. A parent [config](https://github.com/mvc5/framework/blob/master/src/Service/Config/Config.php) can also specify the <a href="https://github.com/mvc5/framework/blob/master/src/Service/Config/Configuration.php#L21">calls</a> to invoke, and if enabled, will be <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L258">merged</a> with the child's calls configuration, otherwise only the child's calls configuration is used.

### [ConfigLink](https://github.com/mvc5/framework/blob/master/src/Service/Config/ConfigLink/ConfigLink.php)
```php
'My\Service' => new Service(My\Service::class, ['config' => new ConfigLink])
```
 
The [config link](https://github.com/mvc5/framework/blob/master/src/Service/Config/ConfigLink/ConfigLink.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L370) to provide the main configuration array or object. 
 
### [Controller](https://github.com/mvc5/framework/blob/master/src/Service/Config/Controller/Controller.php)
```php
'Controller' => new Hydrator(null, [
    'setRequest' => new Service('Request')
]),
'IndexController' => new Controller(IndexController::class),
```

A [controller](https://github.com/mvc5/framework/blob/master/src/Service/Config/Controller/Controller.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L346) to provide the constructor arguments and call methods for a controller without having to specify the name of its parent _Controller_ configuration. This is an optional convenience configuration, it is helpful when controllers require a similar method of instantiation.    

### [ControllerAction](https://github.com/mvc5/framework/blob/master/src/Service/Config/ControllerAction/ControllerAction.php)
```php
'IndexController' => new ControllerAction([
    function(array $args = []) {
        return new Model(null, ['args' => $args]);
    },
    function(Model $model) {
        $model['__CONTROLLER__'] = __FUNCTION__;
        return $model;
    },
    function(Model $model) {
        $model[$model::TEMPLATE] = 'home';
        return $model;
    },
]),
```

A [controller action](https://github.com/mvc5/framework/blob/master/src/Service/Config/ControllerAction/ControllerAction.php) configuration is for an [action controller event](https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php) which accepts an array of functions that are called with [named argument plugin](#named-arguments-and-plugins) support. If the response from the function is a [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php), it will be <a href="https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php#L50">stored</a> and become available to the remaining functions. If the function returns a [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php), then the event is <a href="https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php#L52">stopped</a> and the [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php) is returned. 

### [Dependency](https://github.com/mvc5/framework/tree/master/src/Service/Config/Dependency)
```php
new Dependency('Response\Manager')
```

A [dependency](https://github.com/mvc5/framework/tree/master/src/Service/Config/Dependency) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L354) to create a [shared](https://github.com/mvc5/framework/blob/master/src/Service/Manager/Initializer.php#L45) class object.

### [Factory](https://github.com/mvc5/framework/blob/master/src/Service/Config/Factory/Factory.php)
```php
'Factory'    => new Service(null, [new ServiceManagerLink]),
'My\Service' => new Factory(My\Service::class),
```

A [factory](https://github.com/mvc5/framework/blob/master/src/Service/Config/Factory/Factory.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L338) to create a class object without having to specify the name of its parent _Factory_ configuration.

### [Filter](https://github.com/mvc5/framework/blob/master/src/Service/Config/Filter/Filter.php)
```php
'Response\Manager' => new Filter(new Manager(Response\Manager\Manager::class), ['var_dump']),
```

A [filter](https://github.com/mvc5/framework/blob/master/src/Service/Config/Filter/Filter.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L378) as a callable set of methods that pass the value from the previous function to the next and returns the result of the last function called.

### [Form](https://github.com/mvc5/framework/blob/master/src/Service/Config/Form/Form.php)
```php
'Form'   => new Hydrator(null, ['setView' => new Dependency('View')]),
'MyForm' => new Form(MyForm::class),
```

A [form](https://github.com/mvc5/framework/blob/master/src/Service/Config/Form/Form.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L346) create a class object without having to specify the name of its parent _Form_ configuration.

### [Hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php)
```php
'Mvc\Response' => new Hydrator(
    Mvc\Response\Dispatcher::class,
    ['setResponseManager' => new Dependency('Response\Manager')]
),
```

A [hydrator](https://github.com/mvc5/framework/blob/master/src/Service/Config/Hydrator/Hydrator.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L350) to create a class object. Its constructor requires a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration name and a set of calls to invoke. Using null for the parameter name is a convenient way for it to be used as a parent configuration.

When the array key is a string, it is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L160) as the name of the method to call on the newly created object and passes its array value as a single [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) argument. However, if the string is prefixed with the $ symbol, the string is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L155) instead as the name of the object property to set. If an object method needs to be called multiple times, then an array of methods can be [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L164).

```php
'Route\Exception\Route'  => new Hydrator(
    Route\Exception\Config::class,
    [['set', ['controller', 'Route\Exception\Manager\Controller']], ['set', ['name', 'exception']]]
),
```

Alternatively, any invokable service configuration can be [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L176).

```php
'Route\Exception\Route'  => new Hydrator(
    Route\Exception\Config::class,
    [new Call('Response.setStatusCode', [500])]
),
```

### [Invokable](https://github.com/mvc5/framework/blob/master/src/Service/Config/Invokable/Invokable.php)
```php
$response = $app->call(new Invokable(new Dependency('Response')));
```

An [invokable](https://github.com/mvc5/framework/blob/master/src/Service/Config/Invokable/Invokable.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L392) to return an anonymous function that when invoked will resolve and return its configuration value.

### [Invoke](https://github.com/mvc5/framework/blob/master/src/Service/Config/Invoke/Invoke.php)
```php
new Invoke('Response.setStatusCode', [500]),
new Invoke(new Args([new Dependency('Response'), 'setStatusCode']), [500]),
new Invoke(function() { var_dump(func_get_args()); }),
```

An [invoke](https://github.com/mvc5/framework/blob/master/src/Service/Config/Invoke/Invoke.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L386) to return an anonymous function that when invoked will [resolve](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L332) and [call](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63) its configuration with the optional array of parameters passed to the anonymous function merged with the configuration's args parameter.

```php
$app->call(new Invoke(new Service('Home\Controller')), ['request' => new Service('Request')])
```

### [Manager](https://github.com/mvc5/framework/blob/master/src/Service/Config/Manager/Manager.php)
```php
'Manager' => new Hydrator(null, [
    'aliases'       => new Param('alias'),
    'configuration' => new ConfigLink,
    'events'        => new Param('events'),
    'services'      => new Param('services'),
]),
'Route\Manager' => new Manager(Route\Manager\Manager::class)
```

A [manager](https://github.com/mvc5/framework/blob/master/src/Service/Config/Manager/Manager.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L346) to create a class object without having to specify the name of its parent _Manager_ configuration.

### [Param](https://github.com/mvc5/framework/blob/master/src/Service/Config/Param/Param.php)
```php
new Param('templates.home')
```

A [param](https://github.com/mvc5/framework/blob/master/src/Service/Config/Param/Param.php) is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L358) to retrieve a configuration value and [uses](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L285) a dot notation for arrays and ArrayAccess configurations.

### [Service](https://github.com/mvc5/framework/blob/master/src/Service/Config/Service/Service.php)
```php
'Router' => new Service(
    Route\Router\Router::class,
    [new Param('routes')],
    ['setRouteManager' => new Dependency('Route\Manager')]
),
```
A [service](https://github.com/mvc5/framework/blob/master/src/Service/Config/Service/Service.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L350) to instantiate a class object.  Its constructor requires a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php) configuration name and optionally, the arguments for the class constructor and a set of calls to invoke. See the [hydrator](#hydratorhttpsgithubcommvc5frameworkblobmastersrcserviceconfighydratorhydratorphp) configuration for details on how to specify the calls to invoke.  

### [ServiceConfiguration](https://github.com/mvc5/framework/blob/master/src/Service/Config/ServiceConfig/ServiceConfig.php)
```php
new ServiceConfiguration('Controller\Dispatcher')
```

A [service configuration](https://github.com/mvc5/framework/blob/master/src/Service/Config/ServiceConfig/ServiceConfig.php) is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L382) to return another service configuration value without resolving it.

### [ServiceManagerLink](https://github.com/mvc5/framework/blob/master/src/Service/Config/ServiceManagerLink/ServiceManagerLink.php)
```php
'Mvc' => [Mvc\Mvc::class, new ServiceManagerLink]
```

A [service manager link](https://github.com/mvc5/framework/blob/master/src/Service/Config/ServiceManagerLink/ServiceManagerLink.php) configuration is [used](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L374) to return the current service manager object. It can also be used as a [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Config.php) object to delay the [creation](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ManageService.php#L29), or [resolving](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L332), of a particular value until it is required.

### [ServiceProvider](https://github.com/mvc5/framework/blob/master/src/Service/Config/ServiceProvider/ServiceProvider.php)
```php
'Service\Resolver\Manager' => new ServiceProvider(ManagerResolver::class)
```

A [service provider](https://github.com/mvc5/framework/blob/master/src/Service/Config/ServiceProvider/ServiceProvider.php) configuration is similar to a [service](#servicehttpsgithubcommvc5frameworkblobmastersrcserviceconfigserviceservicephp) configuration object. It adds a call to a method named [provider](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Provider.php#L77) which is used to set the current service manager for the service provider class to use. It's purpose is to return a callable function that when invoked will inspect the configuration passed to it and returns it if it can not or resolves it by returning a new instance of the class object.  
