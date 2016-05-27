## Rendering View Models
If a [layout](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewLayout.php) is required, the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) will be [attached](https://github.com/mvc5/mvc5/blob/master/src/View/Layout/Model.php#L24) to it.

```php
function __invoke(LayoutModel $layout, $model = null)
{
    if (!$model instanceof ViewModel || $model instanceof LayoutModel) {
        return $model;
    }

    $layout->model($model);

    return $layout;
}
```

A [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) is [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php) as part of the [view render event](https://github.com/mvc5/mvc5/blob/master/src/View/Render.php). This allows other renderers to be [configured](https://github.com/mvc5/mvc5/blob/master/config/event.php#L63) and used instead. The [default view renderer](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php) will [bind](http://php.net/manual/en/closure.bind.php) the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) to a [closure](http://php.net/manual/en/class.closure.php) and [extract](http://php.net/manual/en/function.extract.php) its variables before [including](http://php.net/manual/en/function.include.php) the template. The scope of the template is the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) itself.

```php
$render = Closure::bind(function() {
        extract($this->vars());
        
        ob_start();
        
        try {
        
            include $this->template();
        
            return ob_get_clean();
        
        } catch (Throwable $exception) {
        
            ob_get_clean();
        
            throw $exception;
        }
        
    },
    $model
);

return $render();
```

## View Model Plugins
The default [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) supports [plugins](https://github.com/mvc5/mvc5/blob/master/config/service.php) and requires a [service object](https://github.com/mvc5/mvc5/blob/master/src/Service/Service.php) to be injected prior to being [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php). However, because a [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) can be created by a controller, this may not of happened. To overcome this, the current [service object](https://github.com/mvc5/mvc5/blob/master/src/Service/Service.php) will be [injected](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php#L53) into the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) if it does not already have one.

```php
<?php

/** @var Mvc5\Model\ViewModel $this */

echo $this->url('home');
```
