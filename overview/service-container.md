## Service Container
A [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) can access multiple containers via the [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) interface and allows their values and [plugins](#plugins) to be grouped together. Values and [plugins](#plugins) from these containers can be accessed using the [arrow](https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L279) notation, e.g. [<code>Blog->Home</code>](https://github.com/mvc5/mvc5-application/blob/master/config/route.php#L11). A container can be an array or an [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php) object and can contain any type of value, except for [null](http://php.net/manual/en/language.types.null.php). A [plugin](#plugins) will be [resolved](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L413) by its closest parent [service manager](https://github.com/mvc5/mvc5/blob/master/src/Service/Manager.php) and if the container is a [service container](https://github.com/mvc5/mvc5/blob/master/src/Service/Container.php), then all of its values are [resolved](https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L413). If necessary, the [Plug](#plugins) plugin can be used to retrieve a value without being resolved. See the [service](https://github.com/mvc5/mvc5-application/blob/master/config/service.php) configuration in the [demo application](https://github.com/mvc5/mvc5-application) for examples.