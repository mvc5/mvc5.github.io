## Plugins
Various types of plugins are available to use and custom plugins can be created.

### [Args](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Args.php)
```php
'route' => [
    Route\Config::class,
    'config' => new Args([
        'hostname' => new Call('request.getHost'),
        'method'   => new Call('request.getMethod'),
        'path'     => new Call('request.getPathInfo'),
        'scheme'   => new Call('request.getScheme')
    ])
],
```

The [args](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Args.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L372) to return an array of values that are resolved just in time, e.g. when the class is being instantiated. 

### [Call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Call.php)
```php
new Call('Home\Controller', ['response' => new Service('Response')])
```

The [call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Call.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L368) to invoke an object or method and supports [named arguments and plugins](#name-arguments-and-plugins). 

### [Calls](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Calls.php)
```php
'mvc\layout' => new Calls(new Plugin(Mvc5\Mvc\Layout::class), ['service' => new Link])
```

The [calls](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Calls.php) plugin is similar to a [hydrator](#hydratorhttpsgithubcommvc5mvc5blobmastersrcpluginhydratorphp) and is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L348) to resolve a plugin with a set of function calls and supports [named arguments](#name-arguments-and-plugins).   

### [Child](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Child.php)
```php
'manager'         => new Plugin(null),
'service\manager' => new Child(Service\Manager::class, 'manager'),
```

A [child](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Child.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L352) to extend a parent plugin. The first parameter is the name of the class to create and the second is the name of the parent plugin. Custom child configurations can also be created to allow another [plugin](#pluginhttpsgithubcommvc5mvc5blobmastersrcpluginpluginphp) to be used without having to specify the name of its parent. Examples are the [controller](#controllerhttpsgithubcommvc5mvc5blobmastersrcplugincontrollerphp), [factory](#factoryhttpsgithubcommvc5mvc5blobmastersrcpluginfactoryphp), [form](#formhttpsgithubcommvc5mvc5blobmastersrcpluginformphp) and [manager](#managerhttpsgithubcommvc5mvc5blobmastersrcpluginmanagerphp) plugins. 

### [Config](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Config.php)
```php
'my\service' => new Plugin(My\Service::class, ['config' => new Config])
```
 
The [config](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Config.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L376) to provide the main configuration array or object. 
 
### [Controller](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Controller.php)
```php
'controller'      => new Hydrator(null, ['request' => new Plugin('request')]),
'Home\Controller' => new Controller(Home\Controller::class),
```

A [controller](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Controller.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L352) to provide the constructor arguments and call methods for a controller without having to specify the name of its parent _controller_ plugin. This is a convenience plugin for when controllers have a similar method of instantiation.    

### [Dependency](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Dependency.php)
```php
new Dependency('response')
```

A [dependency](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Dependency.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L360) to create a [shared](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Initializer.php#L44) class object.

### [Factory](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Factory.php)
```php
'factory'         => new Service(null),
'Home\Controller' => new Factory(Home\Controller::class),
```

A [factory](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Factory.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L344) to create a class object without having to specify the name of its parent _factory_ plugin.

### [Filter](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Filter.php)
```php
'response' => new Filter(new Plugin(Response::class), ['var_dump']),
```

A [filter](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Filter.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L384) to pass a value from one function to the next and returns the result of the last function called.

### [Form](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Form.php)
```php
'form'    => new Service(null),
'my\form' => new Form(My\Form::class),
```

A [form](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Form.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L352) create a class object without having to specify the name of its parent _form_ plugin.

### [Hydrator](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Hydrator.php)
```php
'mvc\response' => new Hydrator(Mvc5\Mvc\Layout::class, ['service' => new Link])
```

A [hydrator](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Hydrator.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L356) to create an object with a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin name and a set of calls to invoke. Using null for the parameter name is a convenient way for it to be used as a parent plugin. When the array key of its calls configuration is a string, it is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L149) as the name of the method to call on the newly created object and passes its array value as a single [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) argument. However, if the string is prefixed with the $ symbol, the string is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L144) as the name of the object property to set. If a method needs to be called more than once, then an array of methods can be [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L153).

```php
'route'  => new Hydrator(
    Mvc5\Route\Config::class, [['set', 'controller', 'Home\Controller'], ['set', 'name', 'home']]
)
```

Additionally, any [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin can be [called](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L170).

```php
'route'  => new Hydrator(Mvc5\Route\Config::class, [new Call('response.setStatus', [500])]),
```

When an array configuration is used, the current object is passed to the called methods as a named argument called _item_. This can be changed by adding a value to the beginning of the array with the name of the parameter to use.

```php
'service'  => new Hydrator(
    ArrayObject::class, ['$current', new Object, 'index' => 'foo', 'bar' => 'baz']
)
```

```php
class Object
{
    function __invoke($index, $current, $bar)
    {
        return $current[$index] = $bar; //i.e $current['foo'] = 'baz'
    }
}
```


### [Invokable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invokable.php)
```php
$response = $app->call(new Invokable(new Plugin('response')));
```

An [invokable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invokable.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L398) to return an anonymous function. When invoked, it will resolve and return its configured value.

### [Invoke](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invoke.php)
```php
new Invoke('response.setStatusCode', [500]),
new Invoke(new Args([new Plugin('response'), 'setStatusCode']), [500]),
new Invoke(function() { var_dump(func_get_args()); }),
```

An [invoke](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invoke.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L392) to return an anonymous function. When invoked, it will [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L413) and [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L67) its configured value with the optional array of parameters passed to the anonymous function. The parameters are merged with the plugin's args parameters.

```php
$app->call(new Invoke(new Plugin('Home\Controller')), ['request' => new Plugin('Request')])
```

### [Link](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Link.php)
```php
'mvc' => [Mvc5\Mvc::class, new Link]
```

A [link](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Link.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L380) to return the current service object. It can also be used as a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php) object to delay the [creation](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L413) of a particular value until it is required.

### [Manager](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Manager.php)
```php
'manager' => new Hydrator(null, [
    'aliases'       => new Param('alias'),
    'configuration' => new Config,
    'events'        => new Param('events'),
    'services'      => new Param('services'),
]),
'service\manager' => new Manager(Service\Manager::class)
```

A [manager](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Manager.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L352) to create a class object without having to specify the name of its parent _manager_ plugin.

### [Model](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Model.php)
```php
new Model('error/404', ['message' => 'A 404 error occurred'])
```

A [model](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Model.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L356) to create a [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php). Its first parameter is the template name and the second parameter contains its values.

### [Param](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Param.php)
```php
new Param('templates.home')
```

A [param](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Param.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L364) to retrieve a configuration value and [uses](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L226) a dot notation for arrays and objects with [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php).

### [Plug](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plug.php)
```php
new Plug('controller\exception')
```

A [plug](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plug.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L388) to return the value of another plugin configuration without resolving it.

### [Plugin](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugin.php)
```php
'router' => new Plugin(Mvc5\Route\Router:class, [new Param('routes')], ['service' => new Link])
```
A [plugin](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugin.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L356) to instantiate a class object.  It requires the name of a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin and optionally, its constructor arguments and a set of calls to invoke. See the [hydrator](#hydratorhttpsgithubcommvc5mvc5blobmastersrcpluginhydratorphp) plugin for details on how to specify the calls to invoke.  

### [Response](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Response.php)
```php
'web' => new Response('web')
```

A [response](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Response.php) plugin is used to [dispatch](https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php) a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php). It configures the [response dispatch plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php#L46) with the name of the [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) to use and an optional [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object. Each [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) function can return a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object for the the next function to use.  

```php
'web' => [
    'mvc',
    'response\send'
]
```

By default, the [response dispatch plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php#L46) uses a shared [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object.

```php
'response\dispatch' => [Mvc5\Response\Dispatch::class, 'response' => new Dependency('response')]
```

This allows other [response plugins](responsehttpsgithubcommvc5mvc5blobmastersrcpluginresponsephp) to not have to completely dispatch a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php).
 
```php
'route\error' => [
    'error\status',
    'error\route',
    // halt mvc event or new response object
    //'error\controller',
    //'error\view',
    //'error\response'
]
```

For example, the [route error plugin](https://github.com/mvc5/mvc5/blob/master/config/event.php#L32) is part of the [mvc event](https://github.com/mvc5/mvc5/blob/master/config/event.php#L13). When it is called, it [updates the status](https://github.com/mvc5/mvc5/blob/master/config/service.php#L24) of the shared [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object without [stopping](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php#L49) the [mvc event](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php). If it did return a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php), it would [stop](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php#L49) the [mvc event](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php) and the [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) is passed to the [response send plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php#L48), which [sends](https://github.com/mvc5/mvc5/blob/master/src/Response/Send.php) the [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) to the browser as part of the [web event](https://github.com/mvc5/mvc5/blob/master/config/event.php#L66).

### [Service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php)
```php
'router' => new Service(Mvc5\Route\Router:class, [new Param('routes')])
```

A [service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php) plugin is similar to a [plugin](#pluginhttpsgithubcommvc5mvc5blobmastersrcpluginpluginphp) and is used to add a call to a [service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php#L20L77) method to set the current service object.  
