## Rendering View Models
When the content of the [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php) is a [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php), it is [rendered](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php) prior to [sending](https://github.com/mvc5/framework/blob/master/src/Response/Send/Sender.php) the [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php). Additionally, and [prior](https://github.com/mvc5/framework/blob/master/config/event.php#L19) to [rendering](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php) the [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php), if a [layout model](https://github.com/mvc5/framework/blob/master/src/View/Layout/LayoutModel.php) is to be used, it will [add](https://github.com/mvc5/framework/blob/master/src/Mvc/Layout/Renderer.php#L25) the current [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) to itself as its child content and the [layout model](https://github.com/mvc5/framework/blob/master/src/View/Layout/LayoutModel.php) is then set as the content of the [response](https://github.com/mvc5/framework/blob/master/src/Response/Response.php).

```php
function __invoke($model = null, ViewModel $layout = null)
{
    if (!$model || !$layout) {
        return $model;
    }
    
    if (!$model instanceof ViewModel || $model instanceof LayoutModel) {
        return $model;
    }
    
    $layout->child($model);
    
    return $layout;
}
```

The [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) is then [rendered](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php#L32) via the [view render event](https://github.com/mvc5/framework/blob/master/src/View/Render/Render.php) which allows other renderers to be [configured](https://github.com/mvc5/framework/blob/master/config/event.php#L48) and used instead of the default [view renderer](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php).

The [view renderer](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php) will [bind](http://php.net/manual/en/closure.bind.php) the [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) to a [closure](http://php.net/manual/en/class.closure.php) that will [extract](http://php.net/manual/en/function.extract.php) the view model variables and then [include](http://php.net/manual/en/function.include.php) the template file. The scope of the template is the [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) itself which gives the template access to any of the view model's [private](http://php.net/manual/en/language.oop5.visibility.php) and [protected](http://php.net/manual/en/language.oop5.visibility.php) [properties](http://php.net/manual/en/language.oop5.properties.php) and functions.

```php

$render = Closure::bind(function() {
    extract((array) $this->assigned());
    
    ob_start();
    
    try {
    
        include $this->path();
        
        return ob_get_clean();
    
    } catch(Exception $exception) {
    
        ob_get_clean();
    
        throw $exception;
    }

},
$model
);

return $render();

```

## View Model Plugins
The default [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) also supports [plugins](https://github.com/mvc5/framework/blob/master/config/alias.php) which require the [view manager](https://github.com/mvc5/framework/blob/master/src/View/Manager/ViewManager.php) to be injected prior to [rendering](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php) it. However, because they can be created by a controller, this may not of happened. To overcome this, the current [view manager](https://github.com/mvc5/framework/blob/master/src/View/Manager/ViewManager.php) will be [injected](https://github.com/mvc5/framework/blob/master/src/View/Renderer/RenderView.php#L47) into the [view model](https://github.com/mvc5/framework/blob/master/src/View/Model/ViewModel.php) if it does not already have one.

```php
<?php

/** @var Mvc5\View\Model\ViewModel $this */

echo $this->url('home');
```
