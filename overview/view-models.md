## View Models
Controllers can return a [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) that is [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Render.php) using a specified [template](https://github.com/mvc5/mvc5/blob/master/src/Model/Template.php#L17). For convenience, controllers can use a [view model trait](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) that contains methods for returning a [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) with  assigned variables. It has two methods [model](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L31) and [view](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L45). Either can be used depending on if the name of the template is specified. If a [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) is [injected](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L22), a copy of it is returned with the assigned variables; otherwise a default [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) is [created](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L33).

```php
use Mvc5\View\Model;

class Controller
{
    use Model;
    
    public function __invoke()
    {
        return $this->model(['message' => 'Hello World']);
        // or
        return $this->view('home', ['message' => 'Hello World']);
    }
}
```
