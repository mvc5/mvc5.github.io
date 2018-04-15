## Environment Aware
Each [configuration](https://github.com/mvc5/mvc5-application/blob/master/config/config.php) file returns an array of values that can be merged together. For example, the development configuration file <code>config/dev/config.php</code> can include the production configuration file <code>config/config.php</code> and override the name of the database to use in the development environment.
```php
return array_merge(include __DIR__ . '/../config.php', ['db_name' => 'dev']);
```

