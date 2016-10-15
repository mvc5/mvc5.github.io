## Url Generator
The [url plugin](https://github.com/mvc5/mvc5/blob/master/src/Url/Plugin.php) can be used to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php) a url. 

```php
echo $this->url('blog/create', ['author' => 'owner', 'category' => 'oop'], ['canonical' => true]);
```

The [base route](https://github.com/mvc5/mvc5-application/blob/master/config/route.php) must have a name, which is typically the homepage for <code>/</code>, e.g [<code>home</code>](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L7), or it can specify its own, e.g <code>/application</code>. For a standard configuration, [child](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L16) routes, except for the first level, will automatically have their parent name [prepended](https://github.com/mvc5/mvc5/blob/master/src/Route/Dispatch/Router.php#L121); as this simplifies their name when [specifying](https://github.com/mvc5/mvc5-application/blob/master/view/blog/create.phtml#L2) the [route](https://github.com/mvc5/mvc5/blob/master/src/Route/Route.php) to [generate](https://github.com/mvc5/mvc5/blob/master/src/Url/Generator.php#L48), e.g. <code>blog</code> instead of <code>home/blog</code>. Wild card parameters can also be used by using the wildcard route expression <code>{wildcard::*$}</code> and enabling it with <code>'wildcard' => true</code>.
