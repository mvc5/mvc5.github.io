## Controller Action
The [controller action](https://github.com/mvc5/framework/blob/master/src/Service/Config/ControllerAction/ControllerAction.php) service configuration is for an [action controller event](https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php) which accepts an array of functions that are called with [named argument plugin](#named-arguments-and-plugins) support. If the response from the function is a [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php), it will be <a href="https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php#L50">stored</a> and become available to the remaining functions. If the function returns a [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php), then the event is <a href="https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php#L52">stopped</a> and the [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php) is returned.

```php
'controller' => new ControllerAction([
    function(array $args = []) {
        return new Model(null, ['args' => $args]);
    },
    function(Model $model) {
        $model['__CONTROLLER__'] = __FUNCTION__;
        return $model;
    },
    function(Model $model) {
        $model[$model::TEMPLATE] = 'home';
        return $model;
    },
]),
```
