### Template Layouts
If a [layout](https://github.com/mvc5/mvc5/blob/master/src/View/ViewLayout.php) is required, the [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) will be [assigned](https://github.com/mvc5/mvc5/blob/master/src/Template/Layout/Layout.php#L33) to it as part of the [web](https://github.com/mvc5/mvc5/blob/master/config/event.php#L23) function.
```php
function layout(TemplateLayout $layout, $model)
{
    return !$model instanceof TemplateModel || $model instanceof TemplateLayout ? $model : 
        $layout->withModel($model);
}
```
