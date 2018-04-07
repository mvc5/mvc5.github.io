### Url Generator
The [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) can [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php) urls with or without a route configuration and are [RFC 3986](https://tools.ietf.org/html/rfc3986) [encoded](https://github.com/mvc5/mvc5/blob/master/src/Url/Assemble.php).
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
Route configurations must be [named](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php#L74) and [child routes](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Path.php#L41) of the current parent route can [automatically](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php#L127) use their parent route parameters, e.g <code>/dashboard/phpdev/add</code>.
```php
echo $this->url('dashboard/add');
```
[Wild card](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php) parameters can be added by using the <code>{wildcard::*$}</code> route expression and enabling it with <code>'wildcard' => true</code>. The parameters are then [appended](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php#L225) to the path, e.g <code>/dashboard/phpdev/add/type/tasks</code> and are [assigned](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/Wildcard.php#L87) as parameters to the request when the route is matched. 
```php
echo $this->url(['dashboard/add', 'type' => 'tasks']);
```
Urls can also be [generated](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php#L117) without a route configuration by prefixing the path with a forward slash.
```php
echo $this->url('/dashboard/phpdev/list', ['order' => 'desc'], '', ['absolute' => true]);
```
The second parameter of the [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) function is for [query](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php#L148) string arguments, e.g <code>/dashboard/phpdev/list?order=desc</code>. The third parameter is for the [fragment](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php#L148) and the fourth parameter can be used to generate an [absolute](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php#L68) url; the current scheme, host and port will be used if not provided. The [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) class can also be [configured](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php#L52) to always generate an absolute url.
