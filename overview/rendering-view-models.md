## Rendering View Models
When the content of the [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php) is a [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php), it is [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php) prior to [sending](https://github.com/mvc5/mvc5/blob/master/src/Response/Send.php) the [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php). Additionally, and [prior](https://github.com/mvc5/mvc5/blob/master/config/event.php#L17) to [rendering](https://github.com/mvc5/mvc5/blob/master/src/View/Render.php) the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php), if a [layout](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewLayout.php) is to be used, it will [add](https://github.com/mvc5/mvc5/blob/master/src/Mvc/Layout.php#L24) the current [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) to itself as a child model and the [layout](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewLayout.php) is then set as the content of the [response](https://github.com/mvc5/mvc5/blob/master/src/Response/Response.php#L13).

```php
function __invoke(LayoutModel $layout, $model = null)
{
    if (!$model || !$model instanceof ViewModel || $model instanceof LayoutModel) {
        return $model;
    }

    $layout->model($model);

    return $layout;
}
```

A [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) is [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php) as part of the [view render event](https://github.com/mvc5/mvc5/blob/master/src/View/Render.php), this allows other renderers to be [configured](https://github.com/mvc5/mvc5/blob/master/config/event.php#L63) and used instead of the default [view renderer](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php). The [view renderer](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php) will [bind](http://php.net/manual/en/closure.bind.php) the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) to a [closure](http://php.net/manual/en/class.closure.php) and will [extract](http://php.net/manual/en/function.extract.php) the view model variables prior to [including](http://php.net/manual/en/function.include.php) the template file. The scope of the template is the [view model](https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php) itself, this allows a template access to any of the view model's [private](http://php.net/manual/en/language.oop5.visibility.php) and [protected](http://php.net/manual/en/language.oop5.visibility.php) functions and [properties](http://php.net/manual/en/language.oop5.properties.php).

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
