## Url Generator
The [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) can be used to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php) a url. 

```php
echo $this->url('dashboard/add', ['author' => 'owner', 'category' => 'oop'], ['canonical' => true]);
```

The [base route](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) must have a name, which is typically the homepage, e.g [<code>home</code>](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L7). For a standard configuration, [child](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L18) routes, except for the first level, will automatically have their parent name [prepended](https://github.com/mvc5/mvc5/blob/master/src/Route/Dispatch/Router.php#L94). This simplifies their name when [specifying](https://github.com/mvc5/mvc5-application/blob/master/view/dashboard/add.phtml#L2) the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Route/Generator.php#L77), e.g. <code>dashboard</code> instead of <code>home/dashboard</code>. Wild card parameters can also be used with the route expression <code>{wildcard::*$}</code> and enabling it with <code>'wildcard' => true</code>.
