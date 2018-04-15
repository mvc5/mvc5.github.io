## REST API Methods
Routes can be configured with [actions](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Action.php) for specific HTTP methods. The default action is specified by the <code>controller</code> configuration.
```php
'resource' => [
    'path' => '/resource',
    'method' => ['GET', 'POST'],
    'controller' => 'Resource\Controller'
    'action' => [
        'POST' => function(Url $url) {
            return new Response\RedirectResponse($url(), 201);
        }
    ]
]
```
