## Model View Controller
Controllers can use a [configuration](https://github.com/mvc5/framework/blob/master/src/Config/Configuration.php) object as a [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) object that is [rendered](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php) by the view using a specified [template](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php#L23) file name and an optional [child](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php#L18) model which is used by the [layout model](https://github.com/mvc5/framework/blob/master/src/View/Layout/LayoutModel.php). For convenience, controllers can use an existing [view model trait](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php) that has methods for setting the model and returning it. If no model is injected, then a new instance of a standard model will be [created](https://github.com/mvc5/framework/blob/master/src/View/ViewModel.php#L33) and returned. When a controller is [invoked](https://github.com/mvc5/framework/blob/master/src/Controller/Dispatch/Dispatcher.php) and returns a model, it is stored as the [content](https://github.com/mvc5/framework/blob/master/src/Mvc/Base.php#L73) of the [response](https://github.com/mvc5/application/blob/master/config/service.php#L30) object and will be rendered prior to sending the response.

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

The [view model trait](https://github.com/mvc5/framework/blob/master/src/View/ViewModel.php) has two methods, [model](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php#L28) and [view](https://github.com/mvc5/framework/blob/master/src/View/Model/Service/ViewModel.php#L44) that allows the controller to choose which one to use depending on whether the name of the template is specified. Both methods accept an array of variables as the data for the [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php).
