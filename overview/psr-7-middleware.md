## PSR-7 Middleware
[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) applications can be created and configured to use [PSR-7](http://www.php-fig.org/psr/psr-7/) HTTP message objects. The [demo](https://github.com/mvc5/mvc5-application) application contains an example with a configuration that supports anonymous functions, [plugins](#plugins) and [dependency injection](#dependency-injection).

```php
return [
    'middleware\router',
    'middleware\controller',
    'middleware\layout',
    'middleware\renderer'
];
```
