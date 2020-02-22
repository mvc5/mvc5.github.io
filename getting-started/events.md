## Events
<p>When there is no <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php">service</a> configuration and the function name is <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L69">not callable</a>, the <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L22">call</a> method will <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L59">create</a> an <a href="https://github.com/mvc5/mvc5/blob/master/src/Event.php">event</a> object and <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L28">invoke</a> it. If an event configuration <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Service.php#L61">does not exist</a>, then an exception will be thrown. Read more about <a href="/overview/#events">events</a> and <a href="/overview/#named-arguments">named arguments</a>.</p>
 
```php
$config = include __DIR__ . '/../config/config.php';

$config['events']['web'] = [
    fn($model, $msg) => $model . ' ' . $msg,
    fn($response, $model) => $response->with('body', $model),
    function($response) {
        echo $response->body();
    }
];

(new Mvc5\App($config))->call('web', ['model' => 'Hello', 'msg' => 'World!']);
```
