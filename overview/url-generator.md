### Url Generator
The [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) can [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php) urls with or without a route configuration and are [RFC 3986]() encoded.
 
```php
'dashboard' => [
    'path'      => '/dashboard/{user}',
    'controller' => 'dashboard',
    'children' => [
        'add' => [
            'path' => '/add[/{wildcard::*$}]',
            'controller' => 'dashboard\add',
            'wildcard'   => true,
        ]
    ]
]
```

```php
echo $this->url(['dashboard', 'user' => 'phpdev']);
```

Route configurations must be named and child routes of the current parent route can automatically use their parent route parameters, e.g <code>/dashboard/phpdev/add</code>.

```php
echo $this->url('dashboard/add');
```

Wild card parameters can added by using the <code>{wildcard::*$}</code> route expression and enabling it with <code>'wildcard' => true</code>. The parameters are appended to the url and will be added to the parameters for that request, e.g <code>/dashboard/phpdev/add/type/tasks</code>.

```php
echo $this->url(['dashboard/add', 'type' => 'tasks']);
```

Urls can also be generated without having or using a route configuration by prefixin the path with a forward slash.

```php
echo $this->url('/dashboard/phpdev/list', ['order' => 'desc'], ['absolute' => true]);
```

The second parameter of the [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) function is for query string arguments, e.g <code>/dashboard/phpdev/list?order=desc</code> and the third parameter can be used to generate an absolute url; the current scheme, host and port will be used if not provided. The [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) can also be configured to always generate an absolute url.
