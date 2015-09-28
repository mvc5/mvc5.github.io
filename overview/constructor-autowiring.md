## Constructor Autowiring
When the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) [creates](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ManageService.php#L29) a class that either

* Does not have a service configuration, or
* No arguments are passed to the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php), or
* If the arguments passed are <a href="#named-arguments-and-plugins">named</a>,

then the [service manager](https://github.com/mvc5/framework/blob/master/src/Service/Manager/ServiceManager.php) will attempt to determine the required dependencies of the class constructor and their type hint. Service configurations can return any positive value.

```php
Home\Model::class => new Service(Home\Model::class,['home'])
```

When the name of a service configuration is a <a href="https://en.wikipedia.org/wiki/Fully_qualified_name">FQCN</a>, it must have a value other than its string name, otherwise a recursion will occur.

```php
Home\Model::class => Home\Model::class //not allowed
```

Service configurations are only required when an explicit configuration is needed, and in some cases, can provide better runtime performance.
