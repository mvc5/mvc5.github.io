## Callable Events
<p>When there is no <a href="https://github.com/mvc5/framework/blob/master/config/alias.php">alias</a> or <a href="https://github.com/mvc5/framework/blob/master/config/service.php">service</a> configuration and the string function name is not <a href="http://php.net/manual/en/language.types.callable.php">callable</a>, the <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L63">call</a> method will <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L75">check</a> to see if an <a href="https://github.com/mvc5/framework/blob/master/config/event.php">event</a> configuration exists, if so, it will <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Event/Create.php">create</a> and <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L83">trigger</a> an event for that configuration. Otherwise an exception is <a href="https://github.com/mvc5/framework/blob/master/src/Service/Resolver/Resolver.php#L76">thrown</a> since nothing can be found for the name of that function. Read more about <a href="/overview/#events">events</a> and <a href="/overview/#named-arguments-and-plugins">named arguments</a>.</p>
 
```php
$config = include __DIR__ . '/../config/config.php';

$config['events']['web2'] = [
    function($request, $one) {
        ob_start();

        echo $one.'. One, ';
    },
    function($two, $request, $response) {
        echo $two.'. Two, ';
    },
    function($response) {
        echo '3. Three';

        $response->setContent(ob_get_clean());
    },
    function($response) {
        $response->send();
    }
];

(new App($config))->call('web2', ['one' => 1, 'two' => 2]);
```
