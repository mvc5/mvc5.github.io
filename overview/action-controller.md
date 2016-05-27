## Action Controller
The [action controller](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php) is used to control the [invocation](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php#L24) of the controller specified by the [request](https://github.com/mvc5/mvc5/blob/master/src/Controller/Response.php#L50).

```php
function __invoke($controller, array $args = [])
{
    return $this->action($controller, $args);
}
```

A controller is a function just like any other function. It can also be an [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) or a plugin that [resolves](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L84) to a [callable](http://php.net/manual/en/language.types.callable.php) function. If the controller returns a new response, the [controller\response](https://github.com/mvc5/mvc5/blob/master/src/Controller/Response.php) event will be [stopped](https://github.com/mvc5/mvc5/blob/master/src/Controller/Response.php#L74). The returned response is then available for the remaining [web](https://github.com/mvc5/mvc5/blob/master/config/event.php#L33) components to use.

```php
'web' => [
    'route\dispatch',
    'request\error',
    'request\service',
    'controller\dispatch',
    'response\status',
    'response\version',
    'response\send'
],
```

The [controller\dispatch](https://github.com/mvc5/mvc5/blob/master/src/Controller/Dispatch.php) component is used to [trigger](https://github.com/mvc5/mvc5/blob/master/src/Controller/Dispatch.php#L27) the [controller\response](https://github.com/mvc5/mvc5/blob/master/src/Controller/Response.php) event.

```php
'controller\response' => [
    'controller\action',
    'view\layout',
    'view\render',
    'response\model',
]
```
