## View Models
Controllers can use a [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) [configuration](https://github.com/mvc5/mvc5/blob/master/src/Config/Configuration.php) object that is [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php) using a specified [template](https://github.com/mvc5/mvc5/blob/master/src/Model/Template.php#L17). For convenience, controllers can use an existing [view model trait](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php) that has methods for setting the [model](https://github.com/mvc5/mvc5/blob/master/src/Model.php) and returning it. If a [model](https://github.com/mvc5/mvc5/blob/master/src/Model.php) has not been [injected](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L22) into the controller, a [default view model](https://github.com/mvc5/mvc5/blob/master/src/Model.php) is [created](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L33).

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

The [view model trait](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php) has two methods, [model](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L31) and [view](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L45) that allows the controller to choose one depending on whether the name of the template is being specified. Both methods accept an array of variables as the data for the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php).
