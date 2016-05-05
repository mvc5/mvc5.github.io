## Middleware
[Middleware](https://github.com/mvc5/mvc5/blob/master/src/Middleware.php) applications can be created with a configuration that supports anonymous functions, [plugins](#plugins) and [dependency injection](#dependency-injection).

```php
return [
    'middleware\router',
    'middleware\controller',
    'middleware\layout',
    'middleware\renderer'
];
```
