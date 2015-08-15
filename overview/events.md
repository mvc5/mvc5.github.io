## Events
Events can be strings or classes that can manage the arguments used by the methods that invoked it.

```php
class Event
{
    function args()
    {
        return [
        Args::EVENT      => $this,
        Args::RESPONSE   => $this->response(),
        Args::ROUTE      => $this->route(),
        Args::VIEW_MODEL => $this->viewModel(),
        Args::CONTROLLER => $this->route()->controller()
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

The <a href="http://php.net/manual/en/language.types.callable.php">callable</a> $callback parameter can be used to provide any additional parameters not in the named $args array. It can be a [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php), e.g $this, or any callable type.

```php
$this->trigger([Dispatch::CONTROLLER, $controller], $args, $this);
```

Similar to $args in <a href="#named-arguments-and-plugins">named arguments</a>, adding $event will provide the current event.

The [trigger()](https://github.com/mvc5/framework/blob/master/src/Event/Manager/EventManager.php#L18) method of the [event manager](https://github.com/mvc5/framework/blob/master/src/Event/Manager/EventManager.php) accepts either the string name of the event, the event object itself or an array containing the event class name and its constructor arguments. In the example above, $controller is a constructor argument for the [controller dispatch event](https://github.com/mvc5/framework/blob/master/src/Controller/Dispatch/Dispatch.php).

## Event Configuration
Events and listeners are <a href="https://github.com/mvc5/application/blob/master/config/event.php">configurable</a> and support various types of configuration that must resolve to being a <a href="http://php.net/manual/en/language.types.callable.php">callable</a> type.

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
