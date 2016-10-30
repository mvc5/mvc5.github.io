## Events
<p>When there is no <a href="https://github.com/mvc5/mvc5/blob/master/config/service.php">service</a> configuration and the function name is <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L182">not callable</a>, the <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L128">call</a> method will <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Resolver.php#L183">create</a> an <a href="https://github.com/mvc5/mvc5/blob/master/src/Event.php">event</a> object and <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L30">invoke</a> it. If an event configuration <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Generator.php#L62">does not exist</a>, then an <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Exception.php">exception will be thrown</a>. Read more about <a href="/overview/#events">events</a> and <a href="/overview/#named-arguments">named arguments</a>.</p>
 
```php
$config = include __DIR__ . '/../config/config.php';

$config['events']['web'] = [
    function($model, $msg) {
        return $model . ' ' . $msg;
    },
    function($response, $model) {
        $response['body'] = $model;
    },
    function($response) {
        echo $response->body();
    }
];

(new Mvc5\App($config))->call('web', ['model' => 'Hello', 'msg' => 'World!']);
```
