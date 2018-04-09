## View Models
Controllers can return a [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) that is [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Render.php) using a specified [template](https://github.com/mvc5/mvc5/blob/master/src/Template/TemplateModel.php#L14). For convenience, controllers can use a [view model trait](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php) that contains methods for returning a [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) with  assigned variables. It has two methods [model](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L23) and [view](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L40). Either can be used depending on if the name of the template is specified. If a [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) is [injected](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L29), a copy of it is returned with the assigned variables; otherwise a default [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) is [created](https://github.com/mvc5/mvc5/blob/master/src/View/Model.php#L31).
```php
use Mvc5\View\Model;
use Mvc5\View\ViewModel;

class Controller
{
    use Model;
    
    public function __invoke() : ViewModel
    {
        return $this->model(['message' => 'Hello World']);
        // or
        return $this->view('home', ['message' => 'Hello World']);
    }
}
```
