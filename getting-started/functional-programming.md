## Functional Programming
<p>When the application is opened in the web browser, the main <a href="https://github.com/mvc5/mvc5-application/blob/master/public/index.php">public/index.php</a> script is called. However, for this example a simpler version is provided below without any exception handling.</p>

```php
<?php

use Mvc5\App;
use Mvc5\Arg;

include __DIR__ . '/../init.php';

(new App(include __DIR__ . '/../config/config.php', null, true))->call(Arg::WEB);

```

<p>After loading the <a href="https://github.com/mvc5/mvc5/blob/master/src/App.php">application</a> with a <a href="https://github.com/mvc5/mvc5-application/blob/master/config/config.php">configuration</a>, the <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22">call</a> method is invoked with the string parameter <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php#L79">web</a> as the name of the function to call. The parameter passed to the <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22">call</a> method must resolve to a <a href="http://php.net/manual/en/language.types.callable.php">callable</a> type, which means the parameter provided can also be an <a href="http://php.net/manual/en/functions.anonymous.php">anonymous function</a>.</p>


```php
(new App(include __DIR__ . '/../config/config.php'))->call(function($request, $response) {
    var_dump($request->path());
});
```
<p>When the url in the web browser is changed from <code>/</code> to <code>/dashboard</code> the output of the application will be <code>/dashboard</code>. The parameters <a href="https://github.com/mvc5/http-message/blob/master/config/service.php#L11">$request</a> and <a href="https://github.com/mvc5/http-message/blob/master/config/service.php#L13">$response</a> are required and instead of an error occurring, they are resolved as <a href="http://mvc5.github.io/overview/#named-arguments">named arguments</a> using the <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php">service</a> plugin configuration. The anonymous function is the only function called by the application, unlike the original <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php#L79">web</a> function in the main <a href="https://github.com/mvc5/mvc5-application/blob/master/public/index.php">public/index.php</a> script. The service plugin configuration for the default <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php#L79">web</a> function is below.</p>

```php
'web' => new Response('web')
```

<p>If no <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php">service</a> or <a href="https://github.com/mvc5/mvc5/blob/master/config/event.php">event</a> configuration exists for the name <a href="https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L133">web</a>, then an exception will <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L61">occur</a> because a plugin for that name does not exist. Instead, a function with that name can be added to the main <a href="https://github.com/mvc5/mvc5-application/blob/master/public/index.php">public/index.php</a> script.</p>

```php
function web($request, $response) {
    var_dump($request->path());
}

(new App(include __DIR__ . '/../config/config.php'))->call('@web');
```

<p>By default, the <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22">call</a> method assumes that a string is the name of an object, or an <a href="https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php">event</a> to invoke. However, since it is a function, its name must be prefixed with the <a href="https://github.com/mvc5/mvc5/blob/master/src/Arg.php#L16">@</a> sign to indicate that it is a <a href="https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L33">function</a> (or a <a href="https://github.com/mvc5/mvc5/blob/master/src/Signal.php#L29">static class method</a>) and should be invoked directly instead of creating an object.</p>

<p>Since the argument to the <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22">call</a> method must <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L477">resolve</a> to a <a href="http://php.net/manual/en/language.types.callable.php">callable</a> type, the <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php#L79">web</a> configuration can also be an <a href="http://php.net/manual/en/language.oop5.anonymous.php">anonymous class</a>.</p>

```php
'web' => new class() {
    function __invoke($request, $response) {
        var_dump($request->path());
    }
}
```

<p>Its <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php#L79">configuration</a> can also be an <a href="http://php.net/manual/en/functions.anonymous.php">anonymous function</a> that returns another <a href="http://php.net/manual/en/functions.anonymous.php">anonymous function</a> as the one to invoke.</p>

```php
'web' => fn() => function($request, $response) {
        var_dump($request->path());
}
```

<p>However, a function can become large and separating it into a list of functions enables it to be extended with each function having their own individual dependencies. Consequently, the outcome of the function does not have to depend on the list of functions and by using an <a href="https://github.com/mvc5/mvc5/blob/master/src/Event/Event.php">event</a> class, the outcome of each function and the function itself can be controlled.</p>
