## Action Controller
The [mvc action controller](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Controller.php) is used to control the [invocation](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php#L20) of the controller specified by the [matched route](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Event/Model.php#L40). 

```php
function __invoke($controller, array $args = [])
{
    try {

        return $this->action($controller, $args);

    } catch (Throwable $exception) {

        return $this->exception($exception, $controller);

    }
}
```

A controller is a function just like any other function. It can also be an [event](https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php) or a plugin that [resolves](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L84) to a [callable](http://php.net/manual/en/language.types.callable.php) function. If the controller throws an exception, it will be [caught](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Controller.php#L33) and a [controller exception](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php#L39) will be invoked. If the [controller exception](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php#L39) returns a [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php), the [mvc event](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php) will be [stopped](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php#L49) and its [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) will be returned and [sent](https://github.com/mvc5/mvc5/blob/master/src/Response/Send.php) to the browser.

```php
'controller\exception' => [
    'exception\status',
    'exception\controller',
    //'exception\view',
    //'exception\response'
]
```

However, by default, the [controller exception](https://github.com/mvc5/mvc5/blob/master/src/Controller/Action.php#L39) is [configured](https://github.com/mvc5/mvc5/blob/master/config/event.php#L7) to only [set the response exception status](https://github.com/mvc5/mvc5/blob/master/config/service.php#L32) and to [return a view model](https://github.com/mvc5/mvc5/blob/master/src/Controller/Exception.php#L34) so the [mvc event](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php) can continue, without interruption, to [render the view model](https://github.com/mvc5/mvc5/blob/master/src/Mvc/View.php) and to [return a response](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Response.php).

