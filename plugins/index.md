---
layout: sidebar
title:  Plugins
tagline: A next generation dependency injection system
role: page
sidebar: true
---
Plugins can perform a variety of tasks and be nested together to form a composite plugin. Various types of plugins are available and custom plugins can also be created.
##### [App](https://github.com/mvc5/mvc5/blob/master/src/Plugin/App.php)
```php
'dashboard' => new App(new FileInclude(__DIR__ . '/dashboard.php')),
```
The [app](https://github.com/mvc5/mvc5/blob/master/src/Plugin/App.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L201) to provide a scoped instance of an [Mvc5\App](https://github.com/mvc5/mvc5/blob/master/src/App.php) class and uses the current service provider as its [parent](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L60).
##### [Args](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Args.php)
```php
'request' => [
    Request\Config::class,
    'config' => new Args([
        'host' => new Call('request.getHost'),
        'method' => new Call('request.getMethod'),
        'path' => new Call('request.getPathInfo'),
        'scheme' => new Call('request.getScheme')
    ])
],
```
The [args](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Args.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L217) to return an array of values that are resolved just in time, e.g. when the class is being instantiated. 
##### [Call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Call.php)
```php
new Call('Home\Controller', ['response' => new Plugin('Response')])
```
The [call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Call.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L213) to invoke an object or method, and supports [plugins](/plugins) and [named arguments](/overview/#named-arguments).
##### [Callback](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Callback.php)
```php
new Callback(function() {
    $messages = $this->plugin('messages');
})
```
A [callback](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Callback.php) plugin [binds](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L122) the scope of the application to a [closure](http://php.net/manual/en/class.closure.php) which can then access the application's public service methods.
##### [Calls](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Calls.php)
```php
'route\dispatch' => new Calls(new Plugin(Mvc5\Route\Dispatch::class), ['service' => new Link])
```
The [calls](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Calls.php) plugin is similar to a [hydrator](#hydrator) and is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L185) to resolve a plugin with a set of function calls and supports [named arguments](/overview/#named-arguments).   
##### [Child](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Child.php)
```php
'manager' => new Plugin(null),
'service\manager' => new Child(Service\Manager::class, 'manager'),
```
A [child](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Child.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L197) to extend a parent plugin. The first parameter is the name of the class to create and the second is the name of the parent plugin. Custom child configurations can also be created to allow another [plugin](#plugin) to be used without having to specify the name of its parent. Examples are the [controller](#controller), [factory](#factory) and [form](#form) plugins. 
##### [Config](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Config.php)
```php
'Home\Controller' => [Home\Controller::class, 'config' => new Config]
```
The [config](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Config.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L221) to provide the main configuration array or object. 
##### [Controller](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Controller.php)
```php
'controller' => new Hydrator(null, ['request' => new Plugin('request')]),
'Home\Controller' => new Controller(Home\Controller::class),
```
A [controller](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Controller.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L197) to provide the constructor arguments and call methods for a controller without having to specify the name of its parent <code>controller</code> plugin. This is a convenience plugin for when controllers have a similar method of instantiation.
##### [Copy](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Copy.php)
```php
new Copy(new Plug('response'))
```
The [copy](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Copy.php) plugin can be [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L259) to [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) and [clone](http://php.net/manual/en/internals2.opcodes.clone.php) an object.
##### [End](https://github.com/mvc5/mvc5/blob/master/src/Plugin/End.php)
```php
new End(new Call('@session_start'), new Plugin(Session\Config::class));
```
The [end](https://github.com/mvc5/mvc5/blob/master/src/Plugin/End.php) plugin will [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) a list of plugins and return the result of the last plugin.
##### [Expect](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Expect.php)
```php
new Expect(new Call('web'), new Call('exception\response'), true, false);
```
The [expect](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Expect.php) plugin is used to catch an exception when resolving a plugin. The second argument is the plugin to use when an exception is thrown. The third argument indicates whether the exception should be passed to the second plugin as a [named argument](/overview/#named-arguments). The fourth argument indicates whether to [merge](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Expect.php#L82) the exception with the arguments passed to the first plugin.
##### [Factory](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Factory.php)
```php
'factory' => new Service(null),
'Home\Controller' => new Factory(Home\Controller::class),
```
A [factory](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Factory.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L189) to create a class object without having to specify the name of its parent <code>factory</code> plugin.
##### [FileInclude](https://github.com/mvc5/mvc5/blob/master/src/Plugin/FileInclude.php)
```php
new FileInclude('config/templates.php'),
```
A [file include](https://github.com/mvc5/mvc5/blob/master/src/Plugin/FileInclude.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L250) to [include](http://php.net/manual/en/function.include.php) and evaluate a specified file. The name of the file can also be resolved via another plugin.   
##### [Filter](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Filter.php)
```php
'response' => new Filter(
    new Plugin(Response::class), [function($response) { return $response;  }], [], 'response'
)
```
A [filter](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Filter.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L229) to pass a value from one function to the next and returns the result of the last function called. If a function returns null, the iteration is stopped and null is returned. If a function returns false, the iteration is stopped and the previous value, or current object, is returned. The third parameter contains any additional arguments and the fourth parameter specifies the name of the argument for the value that is being filtered.     
##### [Form](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Form.php)
```php
'form' => new Service(null),
'my\form' => new Form(My\Form::class),
```
A [form](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Form.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L189) create a class object without having to specify the name of its parent <code>form</code> plugin.
##### [GlobalVar](https://github.com/mvc5/mvc5/blob/master/src/Plugin/GlobalVar.php)
```php
new GlobalVar('_COOKIE')
```
A [global var](https://github.com/mvc5/mvc5/blob/master/src/Plugin/GlobalVar.php) plugin is a [value](#value) plugin that returns the value assigned to the PHP [<code>$GLOBALS</code>](http://php.net/manual/en/reserved.variables.globals.php) array for the specified parameter name.
##### [Hydrator](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Hydrator.php)
```php
'route\dispatch' => new Hydrator(Mvc5\Route\Dispatch::class, ['service' => new Link])
```
A [hydrator](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Hydrator.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L201) to create an object with a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin name and a set of calls to invoke. Using null for the parameter name is a convenient way for it to be used as a parent plugin. When the array key of its calls configuration is a string, it is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L305) as the name of the method to call on the newly created object and passes its array value as a single [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) argument. However, if the string is prefixed with the $ symbol, the string is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L292) as the name of the object property to set. If a method needs to be called more than once, then an array of methods can be [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L309).
```php
'route' => new Hydrator(
    Mvc5\Route\Config::class, [['set', 'controller', 'Home\Controller'], ['set', 'name', 'home']]
)
```
Additionally, any [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin can also be [called](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L326).
```php
'route' => new Hydrator(Mvc5\Route\Config::class, [new Call('response.setStatus', [500])]),
```
When an array configuration is used, the current object is passed to the called methods as a named argument called <code>item</code>. This can be changed by adding a value to the beginning of the array with the name of the parameter to use.
```php
'service' => new Hydrator(
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
##### [Invokable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invokable.php)
```php
$response = $app->call(new Invokable(new Plugin('response')));
```
An [invokable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invokable.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L244) to return an anonymous function. When invoked, it will [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) and return its configured value.
##### [Invoke](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invoke.php)
```php
new Invoke('response.setStatusCode', [500]),
new Invoke(new Args([new Plugin('response'), 'setStatusCode']), [500]),
new Invoke(function() { var_dump(func_get_args()); }),
```
An [invoke](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Invoke.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L237) to return an anonymous function. When invoked, it will [resolve](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) and [call](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22) its configured value with the optional array of parameters passed to the anonymous function, merged with the plugin's args. The return value is also resolved.
```php
$app->call(new Invoke(new Plugin('Home\Controller')), ['request' => new Plugin('Request')])
```
##### [Link](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Link.php)
```php
'request\service' => [Mvc5\Request\Service::class, new Link]
```
A [link](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Link.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L225) to return the current [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) or service provider.
##### [Maybe](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Maybe.php)
```php
new Maybe($value = null, $default = null)
```
The [maybe](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Maybe.php) plugin returns a value that is not null. The [resolved](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) <code>value</code> will be returned if it is not null, in which case the <code>default</code> value will be returned if it is also not null, otherwise an instance of [Nothing](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Nothing.php) is returned. The value can be [shared](#shared) and then retrieved using the [nullable](#nullable) plugin; which returns null when the value is [Nothing](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Nothing.php).
```php
(new Maybe)(); //Nothing
(new Maybe)('foo'); //foo
(new Maybe)(new Nothing); //Nothing
(new Maybe(null, new Value('bar')))(); //new Value('bar')
(new App)(new Maybe); //Nothing
(new App)(new Maybe(new Value('foo'))); //foo
(new App)(new Maybe(null, new Value('bar'))); //bar
```  
##### [Nullable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Nullable.php)
```php
new Nullable($value = null, $default = null)
```
The [nullable](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Nullable.php) plugin returns a [resolved](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) value. When the value is [Nothing](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Nothing.php), the default value is returned.
```php
(new Nullable)(); //null
(new Nullable)('foo'); //foo
(new Nullable)(new Nothing); //null
(new Nullable(new Nothing, new Value('bar')))(new Nothing); //new Value('bar')
(new App)(new Nullable); //null
(new App)(new Nullable(new Value('foo')); //foo
(new App)(new Nullable(new Nothing, new Value('bar'))); //bar
```  
##### [NullValue](https://github.com/mvc5/mvc5/blob/master/src/Plugin/NullValue.php)
```php
new NullValue
```
A [null value](https://github.com/mvc5/mvc5/blob/master/src/Plugin/NullValue.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L263) to return a null value. It can be [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L535) to [prevent](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L136) a service provider from being [invoked](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L136) and from [creating](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L42) an instance of its service same.
##### [Param](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Param.php)
```php
new Param('templates.home')
```
A [param](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Param.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L209) to retrieve a configuration value and [uses](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L644) a dot notation for arrays and objects with [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php).
##### [Plug](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plug.php)
```php
new Plug('controller\exception')
```
A [plug](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plug.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L233) to return the value of another plugin configuration.
##### [Plugin](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugin.php)
```php
'router' => new Plugin(Mvc5\Route\Router:class, [new Param('routes')], ['service' => new Link])
```
A [plugin](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugin.php) is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L201) to instantiate a class object.  It requires the name of a [resolvable](https://github.com/mvc5/mvc5/blob/master/src/Resolvable.php) plugin and optionally, its constructor arguments and a set of calls to invoke. See the [hydrator](#hydrator) plugin for details on how to specify the calls to invoke.  
##### [Plugins](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugins.php)
```php
new Plugins(__DIR__.'/services.php')
```
A [plugins](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Plugins.php) config is used to instantiate an [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) class. The first parameter is an array of service plugins, the second parameter defaults to using the current application as the provider for any missing services. The third parameter sets the [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) as the [scope](http://php.net/manual/en/closure.bind.php) for any anonymous functions within service array plugin configuration.
##### [Provide](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Provide.php)
```php
new Provide($config, array $args = [])
```
The [provide](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Provide.php) plugin is used to retrieve a value from its parent container.
##### [Register](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Register.php)
```php
'user' => new Register('user', 'session', new Plugin(Mvc5\Config::class))
```
The [register](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Register.php) plugin will create an object if it is not already registered with a specified ([configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php)) object. The first parameter is the registered name, the second parameter is the name of the [service configuration](https://github.com/mvc5/mvc5/blob/master/config/service.php#L51) that should contain the registered object. The third parameter is the [plugin](/plugins) configuration for the object to create and register if it does not already exist.
##### [Response](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Response.php)
```php
'web' => new Response('web')
```
A [response](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Response.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L201) to [dispatch](https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php) a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php). It configures the [response dispatch plugin](https://github.com/mvc5/mvc5/blob/master/config/service.php#L44) with the name of the [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) to use and an optional [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php) and [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object. Each [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) function can return a [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php) or [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) object for the the next function to use.
##### [Scope](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Scope.php)
```php
new Scope(
  Request\Config::class, 
  [new _Plugin(App::class, [[Arg::SERVICES => $plugins], null, true, true]), new Link]
)
```
The [scope](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Scope.php) plugin is used to set the scope of an [application](https://github.com/mvc5/mvc5/blob/master/src/App.php) to the class that is being created. A [closure](http://php.net/manual/en/class.closure.php) within the application's configuration will then have the same scope as the class being created and can access its protected properties.
##### [Scoped](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Scoped.php)
```php
new Scoped($this)
```
The [scoped](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Scoped.php) plugin is used to provide a [closure](http://php.net/manual/en/class.closure.php) with the scope of the application. The first parameter of the plugin is a function that is called when the plugin is being resolved. The function returns a [closure](http://php.net/manual/en/class.closure.php) and its scope is set to the scope of the application. 
##### [ScopedCall](https://github.com/mvc5/mvc5/blob/master/src/Plugin/ScopedCall.php)
```php
new ScopedCall($this)
```
The [scoped call](https://github.com/mvc5/mvc5/blob/master/src/Plugin/ScopedCall.php) plugin extends the [call](#call) plugin and uses the [scoped](#scoped) plugin to set the scope of the closure for it to be [called](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22) with.  
##### [Service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php)
```php
'router' => new Service(Mvc5\Route\Router:class, [new Param('routes')])
```
A [service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php) plugin is similar to a [plugin](#plugin) and is used to add a call to a [service](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Service.php#L20) method to set the current service object.  
##### [Session](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Session.php)
```php
'user' => new Session('user', new Plugin(Mvc5\Config::class))
```
The [session](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Session.php) plugin is a [register](#register) plugin that retrieves a session variable. The first parameter is the name of the session variable, the optional second parameter is the plugin configuration for the object to create if it does not already exist in the session. If a value already exists in the session, it will be returned and a new object will not be created. 
##### [Shared](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Shared.php)
```php
new Shared('response')
```
A [shared](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Shared.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L205) to create a [shared](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Container.php#L170) value. When used as a configuration value, it should specify another configuration in order to prevent a recursion error. Alternatively, a configuration can be passed as a second argument to its constructor.
```php
'response' => new Shared('response', new Plugin(Http\Response::class))
```
##### [Value](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Value.php)
```php
new Value('A demo web page')
```
A [value](https://github.com/mvc5/mvc5/blob/master/src/Plugin/Value.php) plugin is used to return a string value from the services container instead of the main configuration. Otherwise the container will assume that the string is the name of a class to instantiate.
##### [ViewModel](https://github.com/mvc5/mvc5/blob/master/src/Plugin/ViewModel.php)
```php
new Model('error/404', ['message' => 'A 404 error occurred'])
```
A [view model](https://github.com/mvc5/mvc5/blob/master/src/Plugin/ViewModel.php) plugin is [used](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L201) to create a [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php). Its first parameter is the template name and the second parameter contains its values.
