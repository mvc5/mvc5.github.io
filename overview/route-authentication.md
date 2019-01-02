## Route Authentication
Routes that should only be available to logged in users can be protected by setting the `authenticate` route attribute to `true`. Child routes are automatically protected and can override the parent value.
```php
'dashboard' => [
    'path' => '/dashboard',
    'authenticate' => true,
    'children' => [
        'add' => [
            'path' => '/add'
        ]
    ]
]
```
If the user is not logged in, and it is a `GET` request and not a `json` request, the current URL is stored in the session and the user is redirected to the login page. Once the user has logged in, they are redirected back to the URL that is stored in the session. The default login URL is `/login`, and it can be changed by adding the URL to the `route\match\authenticate` service configuration.
```php
'route\match\authenticate' => [Mvc5\Route\Match\Authenticate::class, '/login']
```
