## Plugins
Various types of plugins are available to use and custom plugins can be created.

### [App](https://github.com/mvc5/mvc5/blob/master/src/Plugin/App.php)
```php
'blog' => new App(new FileInclude(__DIR__ . '/blog.php')),
```
The [app](https://github.com/mvc5/mvc5/blob/master/src/Plugin/App.php) plugin is used to provide a scoped instance of the [Mvc5\App](https://github.com/mvc5/mvc5/blob/master/src/App.php) and uses the current application as its fallback service provider.   

### [Args](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Args.php)
```php
'request' => [
    Request\Config::class,
    'config' => new Args([
        'hostname' => new Call('request.getHost'),
        'method'   => new Call('request.getMethod'),
        'path'     => new Call('request.getPathInfo'),
        'scheme'   => new Call('request.getScheme')
    ])
],
```

The [args](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Args.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L205) to return an array of values that are resolved just in time, e.g. when the class is being instantiated. 

### [Call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Call.php)
```php
new Call('Home\Controller', ['response' => new Plugin('Response')])
```

The [call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Call.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L199) to invoke an object or method and supports [named arguments and plugins](#named-arguments). 

### [Calls](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Calls.php)
```php
'route\dispatch' => new Calls(new Plugin(Mvc5\Route\Dispatch::class), ['service' => new Link])
```

The [calls](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Calls.php) plugin is similar to a [hydrator](#hydratorhttpsgithubcommvc5mvc5blobmastersrcpluginhydratorphp) and is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L179) to resolve a plugin with a set of function calls and supports [named arguments](#named-arguments).   

### [Child](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Child.php)
```php
'manager'         => new Plugin(null),
'service\manager' => new Child(Service\Manager::class, 'manager'),
```

A [child](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Child.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L183) to extend a parent plugin. The first parameter is the name of the class to create and the second is the name of the parent plugin. Custom child configurations can also be created to allow another [plugin](#pluginhttpsgithubcommvc5mvc5blobmastersrcpluginpluginphp) to be used without having to specify the name of its parent. Examples are the [controller](#controllerhttpsgithubcommvc5mvc5blobmastersrcplugincontrollerphp), [factory](#factoryhttpsgithubcommvc5mvc5blobmastersrcpluginfactoryphp), [form](#formhttpsgithubcommvc5mvc5blobmastersrcpluginformphp) and [manager](#managerhttpsgithubcommvc5mvc5blobmastersrcpluginmanagerphp) plugins. 

### [Config](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Config.php)
```php
'Home\Controller' => [Home\Controller::class, 'config' => new Config]
```
 
The [config](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Config.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L209) to provide the main configuration array or object. 
 
### [Controller](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Controller.php)
```php
'controller'      => new Hydrator(null, ['request' => new Plugin('request')]),
'Home\Controller' => new Controller(Home\Controller::class),
```

A [controller](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Controller.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L183) to provide the constructor arguments and call methods for a controller without having to specify the name of its parent _controller_ plugin. This is a convenience plugin for when controllers have a similar method of instantiation.    

### [Copy](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Copy.php)
```php
new Copy(new Plug('response'))
```

The [copy](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Copy.php) plugin can be [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L252) to [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L500) and [clone](http://php.net/manual/en/internals2.opcodes.clone.php) an object.

### [Dependency](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Dependency.php)
```php
new Dependency('response')
```

A [dependency](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Dependency.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L191) to create a [shared](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Initializer.php#L44) value. When used as a configuration value, it should specify another configuration in order to prevent a recursion error. Alternatively, a configuration can be passed as a second argument to its constructor.

```php
'response' => new Dependency('response', new Plugin(Http\Response::class))
```

### [End](https://github.com/mvc5/mvc5/blob/master/src/Plugin/End.php)
```php
new End(new Call('@session_start'), new Plugin(Session\Config::class));
```

The [end](https://github.com/mvc5/mvc5/blob/master/src/Plugin/End.php) plugin will [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L566) a list of plugins and return the result of the last plugin.

### [Factory](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Factory.php)
```php
'factory'         => new Service(null),
'Home\Controller' => new Factory(Home\Controller::class),
```

A [factory](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Factory.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L175) to create a class object without having to specify the name of its parent _factory_ plugin.

### [FileInclude](https://github.com/mvc5/mvc5/blob/master/src/Plugin/FileInclude.php)
```php
new FileInclude('config/templates.php'),
```

A [file include](https://github.com/mvc5/mvc5/blob/master/src/Plugin/FileInclude.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L242) to [include](http://php.net/manual/en/function.include.php) and evaluate a specified file. The name of the file can also be resolved via another plugin.   

### [Filter](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Filter.php)
```php
'response' => new Filter(
    new Plugin(Response::class), [function($response) { return $response;  }], [], 'response'
)
```

A [filter](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Filter.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L217) to pass a value from one function to the next and returns the result of the last function called. If a function returns null, the iteration is stopped and null is returned. If a function returns false, the iteration is stopped and the previous value, or current object, is returned. The third parameter contains any additional arguments and the fourth parameter specifies the name of the argument for the value that is being filtered.     

### [Form](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Form.php)
```php
'form'    => new Service(null),
'my\form' => new Form(My\Form::class),
```

A [form](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Form.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L183) create a class object without having to specify the name of its parent _form_ plugin.

### [Hydrator](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Hydrator.php)
```php
'route\dispatch' => new Hydrator(Mvc5\Route\Dispatch::class, ['service' => new Link])
```

A [hydrator](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Hydrator.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L187) to create an object with a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin name and a set of calls to invoke. Using null for the parameter name is a convenient way for it to be used as a parent plugin. When the array key of its calls configuration is a string, it is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L149) as the name of the method to call on the newly created object and passes its array value as a single [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) argument. However, if the string is prefixed with the $ symbol, the string is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L144) as the name of the object property to set. If a method needs to be called more than once, then an array of methods can be [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L153).

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

An [invokable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invokable.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L234) to return an anonymous function. When invoked, it will resolve and return its configured value.

### [Invoke](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invoke.php)
```php
new Invoke('response.setStatusCode', [500]),
new Invoke(new Args([new Plugin('response'), 'setStatusCode']), [500]),
new Invoke(function() { var_dump(func_get_args()); }),
```

An [invoke](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invoke.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L225) to return an anonymous function. When invoked, it will [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L566) and [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L128) its configured value with the optional array of parameters passed to the anonymous function. The parameters are merged with the plugin's args parameters.

```php
$app->call(new Invoke(new Plugin('Home\Controller')), ['request' => new Plugin('Request')])
```

### [Link](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Link.php)
```php
'request\service' => [Mvc5\Request\Service::class, new Link]
```

A [link](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Link.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L213) to return the current service object. It can also be used as a [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Config.php) object to delay the [creation](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L566) of a particular value until it is required.

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

A [manager](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Manager.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L183) to create a class object without having to specify the name of its parent _manager_ plugin.

### [Model](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Model.php)
```php
new Model('error/404', ['message' => 'A 404 error occurred'])
```

A [model](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Model.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L187) to create a [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php). Its first parameter is the template name and the second parameter contains its values.

### [Param](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Param.php)
```php
new Param('templates.home')
```

A [param](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Param.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L195) to retrieve a configuration value and [uses](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L226) a dot notation for arrays and objects with [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php).

### [Plug](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plug.php)
```php
new Plug('controller\exception')
```

A [plug](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plug.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L221) to return the value of another plugin configuration.

### [Plugin](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugin.php)
```php
'router' => new Plugin(Mvc5\Route\Router:class, [new Param('routes')], ['service' => new Link])
```
A [plugin](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugin.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L187) to instantiate a class object.  It requires the name of a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin and optionally, its constructor arguments and a set of calls to invoke. See the [hydrator](#hydratorhttpsgithubcommvc5mvc5blobmastersrcpluginhydratorphp) plugin for details on how to specify the calls to invoke.  

### [Plugins](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugins.php)
```php
new Plugins(new FileInclude(__DIR__.'/plugins.php'), new Link, true)
```
A [plugins](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugins.php) config is used to instantiate an instance of the [Plugins](https://github.com/mvc5/mvc5/blob/master/src/Plugins.php) class.  It requires an array of service plugins and optionally a service provider for when a configuration can not be resolved, this allows nested containers to be created and a reference to the parent container can be provided using the [Link](#linkhttpsgithubcommvc5mvc5blobmastersrcpluginlinkphp) plugin. The third parameter indicates the [scope](http://php.net/manual/en/closure.bind.php) of the anonymous functions within the configuration, setting it to true will use the [Plugins](https://github.com/mvc5/mvc5/blob/master/src/Plugins.php) object.

### [Provide](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Provide.php)
```php
new Provide($config, array $args = [])
```
The [provide](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Provide.php) plugin is used to retrieve a value from its parent container.

### [Register](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Register.php)
```php
'user' => new Register('user', 'session', new Plugin(Mvc5\Config::class))
```

The [register](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Register.php) plugin will create an object if it is not already registered with a specified ([configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php)) object. The first parameter is the registered name, the second parameter is the name of the [service configuration](https://github.com/mvc5/mvc5/blob/master/config/service.php#L51) that should contain the registered object. The third parameter is the [plugin](#plugins) configuration for the object to create and register if it does not already exist. 

### [Response](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Response.php)
```php
'web' => new Response('web')
```

A [response](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Response.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L240) to [dispatch](https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php) a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php). It configures the [response dispatch plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php#L33) with the name of the [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) to use and an optional [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php) and [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object. Each [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) function can return a [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php) or [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object for the the next function to use.

### [Scope](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Scope.php)
```php
new Scope(Server\Config::class, new Plugins(new FileInclude(__DIR__.'/server.php'), new Link))
```
The [scope](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Provider.php) plugin is used to set the scope of the [Plugins](#pluginshttpsgithubcommvc5mvc5blobmastersrcpluginpluginsphp) container to the object that is being created.

### [Service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php)
```php
'router' => new Service(Mvc5\Route\Router:class, [new Param('routes')])
```

A [service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php) plugin is similar to a [plugin](#pluginhttpsgithubcommvc5mvc5blobmastersrcpluginpluginphp) and is used to add a call to a [service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php#L20L77) method to set the current service object.  

### [Session](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Session.php)
```php
'user' => new Session('user', new Plugin(Mvc5\Config::class))

'user' => new Session('user')
```

The [session](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Session.php) plugin is a [register](#registerhttpsgithubcommvc5mvc5blobmastersrcpluginregisterphp) plugin that retrieves a session variable. The first parameter is the name of the session variable, the optional second parameter is the plugin configuration for the object to create if it does not already exist in the session. If a value already exists in the session, it will be returned and a new object will not be created. 

### [Value](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Value.php)
```php
new Value('A demo web page')
```

A [value](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Value.php) plugin is used to return a string value from the services container instead of the main configuration. Otherwise the container will assume that the string is the name of a class to instantiate.
