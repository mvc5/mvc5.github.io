## Model View Controller
Controllers can use a [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object as a [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) object that is rendered by the view using its specified template file name and an optional child model that is used by the [layout model](https://github.com/mvc5/framework/blob/master/src/View/Layout/LayoutModel.php). For convenience, controllers can use an existing [view model trait](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php) that has methods for setting the model and returning it. If no model is injected, then a new instance of a standard model will be created and returned. When a controller is invoked and returns a model, it is stored as the content of the response object and will be rendered prior to sending the response. The [view model trait](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php) has two methods

* [model](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php#L28)
* [view](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php#L44)

This allows the controller to choose the [view](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php#L44) method when a specific template is known, or the controller can use the [model](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php#L28) method and pass an array of variables as the data for the [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php).

```php
use Mvc5\View\ViewModel;

class Controller
{
    use ViewModel;
    
    public function __invoke()
    {
        return $this->model(['message' => 'Hello World']);
        // or
        return $this->view('home', ['message' => 'Hello World']);
    }
}
```

## Controller Action
The [controller action](https://github.com/mvc5/framework/blob/master/src/Service/Config/ControllerAction/ControllerAction.php) configuration is for an [action controller event](https://github.com/mvc5/framework/blob/master/src/Controller/Action/Action.php) which accepts an array of functions that are called with [named argument plugin](#named-arguments-and-plugins) support. If the response from the function is a [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php), it will be stored and be available to subsequent functions. If the function returns a [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php), then the event is stopped and the [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php) is returned.

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
