## Action Controller
The [action controller](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php) is used to control the [invocation](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php#L24) of the controller specified by the [request](https://github.com/mvc5/mvc5/blob/master/src/Request/Request.php#L34).
```php
function __invoke($controller = null, array $argv = [])
{
    return $controller ? $this->call($controller, $argv) : null;
}
```
A controller is a function, it can also be [middleware](#middleware), or an [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php), or a [plugin](#plugins) that [resolves](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477) to a [callable](http://php.net/manual/en/language.types.callable.php) function. If the value returned from the controller is not null and is not a [Http\Response](https://github.com/mvc5/mvc5/blob/master/src/Http/Response.php), it will be [set](https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php#L78) as the value of the response body for the remaining components to transform into a value that can be sent to the client.  
```php
'web' => [
    'route\dispatch',
    'request\error',
    'request\service',
    'controller\action',
    'view\layout',
    'view\render',
    'response\status',
    'response\version',
    'response\send'
],
```
