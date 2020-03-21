## REST API Methods
Routes can be configured with [actions](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Action.php) for specific HTTP methods. The default action is specified by the <code>controller</code> configuration. HEAD requests are automatically matched to the route GET <code>method</code> and <code>action</code>. 
```php
'resource' => [
    'path' => '/resource',
    'method' => ['GET', 'POST'],
    'controller' => 'Resource\Controller',
    'csrf_token' => false,
    'action' => [
        'POST' => fn(Url $url) => new Response\RedirectResponse($url(), 201)
    ]
]
```
