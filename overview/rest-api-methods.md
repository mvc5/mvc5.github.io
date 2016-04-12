## REST API Methods
Routes can be configured with actions for specific HTTP Methods. The default action is specified with the controller configuration.

```php
'resource' => [
    'route' => '/resource',
    'method' => ['GET', 'POST'],
    'controller' => 'Resource\Controller'
    'action' => [
        'POST' => function(Url $url) {
            return new Response\RedirectResponse($url(), 201);
        }
    ]
]
```
