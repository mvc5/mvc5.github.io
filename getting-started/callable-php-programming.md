## Callable PHP Programming
<p>When the application is opened in the web browser the main <a href="https://github.com/mvc5/application/blob/master/public/index.php">public/index.php</a> script is called.</p>

```php
use Mvc5\Application\App;
use Mvc5\Application\Args;

include __DIR__ . '/../init.php';

(new App(include __DIR__ . '/../config/config.php'))->call(Args::WEB);

```

<p>After loading the <a href="https://github.com/mvc5/framework/blob/master/src/Application/App.php">application</a> with its <a href="https://github.com/mvc5/application/blob/master/config/config.php">configuration</a>, its <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63">call</a> method is invoked with the string parameter <a href="https://github.com/mvc5/framework/blob/master/config/alias.php#L19">web</a> as the name of the function to call. The parameter passed to the call method must resolve to a <a href="http://php.net/manual/en/language.types.callable.php">callable</a> type, which means the parameter provided can also be an <a href="http://php.net/manual/en/functions.anonymous.php">anonymous function</a>.</p>
```php
(new App(include __DIR__ . '/../config/config.php'))->call(function($request, $response) {
    var_dump($request->getPathInfo());
});
```
<p>When the url in the web browser is changed from <code>/</code> to <code>/blog</code> the output of the application will be <code>/blog</code>. The parameters <code>$request</code> and <code>$response</code> are automatically resolved, because they are required by the anonymous function, as named arguments using the plugin <a href="https://github.com/mvc5/framework/blob/master/config/alias.php">alias</a> configuration. The above anonymous function is the only function called by the application, unlike the <a href="https://github.com/mvc5/framework/blob/master/config/alias.php#L19">web</a> function in the main <a href="https://github.com/mvc5/application/blob/master/public/index.php">public/index.php</a> script. The plugin alias configuration for the <a href="https://github.com/mvc5/framework/blob/master/config/alias.php#L19">web</a> function is below.</p>

```php
'web' => new Service('Mvc')
```

<p>If there is no alias or service configuration for the name web, then an error would occur as there is no function in PHP with that name. However, one can be added to the main <a href="https://github.com/mvc5/application/blob/master/public/index.php">public/index.php</a> script. To easily test this, the name web2 should be used instead.</p>

```php
function web2($request, $response) {
    var_dump($request->getPathInfo());
}

(new App(include __DIR__ . '/../config/config.php'))->call('web2');
```

<p>Additionally, since the call method argument must resolve to a callable type, the <a href="https://github.com/mvc5/framework/blob/master/config/alias.php#L19">web</a> alias configuration can also be an anonymous function.</p>

```php
'web' => function($request, $response) {
    var_dump($request->getPathInfo());
}
```

<p>However, because its default value is a <a href="https://github.com/mvc5/framework/blob/master/src/Service/Config/Service/Service.php">Service</a> configuration, an actual <a href="https://github.com/mvc5/framework/blob/master/config/service.php#L62">service configuration</a> must exist.</p>

```php
'Mvc' => new Service(Mvc\Mvc::class, [new ServiceManagerLink]),
```

<p>Which means the <a href="https://github.com/mvc5/framework/blob/master/config/service.php#L62">service configuration</a> can also be an anonymous factory function that returns another anonymous function as the one to invoke.</p>

```php
'Mvc' => function() {
    return function($request, $response) {
        var_dump($request->getPathInfo());
    };
},
```

<p>This is the limit in which a single anonymous function can be used by the <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63">call</a> method. There is also a limit in how much functionality can be obtained from a single function. Functions can become large and splitting them into a list of functions is beneficial since the function becomes an extensible list of functions each with their own specific dependencies injected. Consequently, the outcome of the function does not have to depend on the list of functions. By using an event class it is possible to control the outcome of each function and consequently the function itself. Read more in <a href="/overview/#events">events</a>.</p> 
