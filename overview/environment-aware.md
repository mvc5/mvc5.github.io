## Environment Aware
Development configurations can override production values using [array_merge](http://php.net/manual/en/function.array-merge.php) as each [configuration](https://github.com/mvc5/mvc5-application/blob/master/config/config.php) file returns an array of values.

```php
return array_merge(
    include __DIR__ . '/../config.php',
    [
        'db_name' => 'dev'
    ]
);
```

For example, the development configuration file <code>config/dev/config.php</code> includes the main production file <code>config/config.php</code> and overrides the name of the database to use in development.
