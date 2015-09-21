## Events
An [event](https://github.com/mvc5/framework/blob/master/src/Event/Event.php) is a function just like any other function. However, instead of having a single function for its implementation, it can be implemented across multiple functions. Which via its [configuration](https://github.com/mvc5/framework/blob/master/config/event.php), allows the function to be easily extended.       

```php
function dispatch(callable $controller, array $args = [])
{
    return $this->trigger([Controller::DISPATCH, Args::CONTROLLER => $controller], $args, $this);
}
```

For example, rather than directly invoking the controller, the [dispatch](https://github.com/mvc5/framework/blob/master/src/Controller/Manager/Manager.php#L47) method triggers the [controller dispatch](https://github.com/mvc5/framework/blob/master/src/Controller/Dispatch/Dispatch.php) event and returns a result. This allows additional functions to be [configured](https://github.com/mvc5/framework/blob/master/config/event.php#L7) so that they can be invoked before and after [calling](https://github.com/mvc5/framework/blob/master/src/Controller/Dispatch/Dispatcher.php) the controller. In the above, the first parameter of the trigger method is a service configuration array containing the event class name and its named constructor arguments. This parameter can be a string, i.e the name of the event, an array (or a [resolvable](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolvable.php)) service configuration, or an event object.   

When the first parameter of the trigger method is a string that does not have a service configuration, then the result of the last function called will be returned as the result for that event. This causes the event to be a list of functions whose iteration cannot be stopped and its result cannot be controlled from outside of the functions being called. In order to allow the event to be [stopped](https://github.com/mvc5/framework/blob/master/src/Event/Event.php#L23) and for better inversion of control, an event class should be used.

```php
class Dispatch
    implements Event
{
    use EventSignal;
    
    function args()
    {
        return [
            Args::EVENT      => $this,
            Args::CONTROLLER => $this->controller
        ];        
    }
    
    function __invoke(callable $listener, array $args = [], callable $callback = null)
    {
        $response = $this->signal($listener, $this->args() + $args, $callback);
        
        if ($response instanceof Response) {
            $this->stop();
        }
        
        return $response;
    }
}
```

The above shows an event class that uses the [signal](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Signal.php) method to provide support for [named arguments](#named-arguments-and-plugins) to the functions for that event. The method signature of these functions can specify any of the parameters available from the args function, or the $args array, or the [$callback](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L423) function which can be a [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) that provides support for [named arguments and plugins](#named-arguments-and-plugins).  

## Event Configuration
Events and listeners are <a href="https://github.com/mvc5/application/blob/master/config/event.php">configurable</a> and support various types of configuration that must [resolve](https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L186) to being a <a href="http://php.net/manual/en/language.types.callable.php">callable</a> type.

```php
'Mvc' => [
    'Mvc\Route',
    'Mvc\Controller',
    'Mvc\Layout',
    'Mvc\View',
    function($event, $vm) { //named args
        var_dump(__FILE__, $event, $vm);
    },
    'Mvc\Response'
]
```

## Event Iterators
An event configuration can be an array or a [traversable](http://php.net/manual/en/class.traversable.php) object.

```php
'Mvc' => new \ArrayIterator([
    'Mvc\Route',
    'Mvc\Controller',
    'Mvc\Layout',
    'Mvc\View',
    function($event, $vm) { //named args
        var_dump(__FILE__, $event, $vm);
    },
    'Mvc\Response'
])
```

The event class itself can also be [traversable](http://php.net/manual/en/class.traversable.php) and contain its own configuration. See the [controller action](#controller-action) class as an example. 
