### Rendering View Models
[View models](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) specify the name of the template to be [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Renderer.php) with. Template names can also have an associated [template configuration](https://github.com/mvc5/mvc5-application/blob/master/config/template.php) specifying the full path to the template file. If the template name [contains a dot](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Find.php#L34) it is considered to be a full path to the template file. Otherwise it is the file path [relative to the application view directory](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Find.php#L35) without the <code>.phtml</code> file extension.
```php
function __invoke($model, array $vars = []) : string
{
    return $this->view->render($model, $vars);
}
```
The [renderer](https://github.com/mvc5/mvc5/blob/master/src/View/Renderer.php) function accepts two arguments. The first argument is the name of the view model or the name of the relative template path. The second argument is the array of variables to assign to the view model and template.
```php
echo $this->render('/home/index', ['request' => $request]);
```
By [prefixing](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Model.php#L30) the template name with the directory separator, the view [renderer](https://github.com/mvc5/mvc5/blob/master/src/View/Renderer.php) will not use the [service locator](https://github.com/mvc5/mvc5/blob/master/src/Service/Service.php) to find an associated [view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) and instead will create a [default view model](https://github.com/mvc5/mvc5/blob/master/src/ViewModel.php) with the assigned variables.
### View Engine
The [default view engine](https://github.com/mvc5/mvc5/blob/master/src/View/Engine/PhpEngine.php) will [bind](http://php.net/manual/en/closure.bind.php) the [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) to a [closure](http://php.net/manual/en/class.closure.php) and [extract](http://php.net/manual/en/function.extract.php) its variables before [including](http://php.net/manual/en/function.include.php) the template. The scope of the template is the [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) itself.
```php
function render(TemplateModel $template) : string
{
    return (function() {
        /** @var TemplateModel $this */

        extract($this->vars(), EXTR_SKIP);

        ob_start();

        try {

            include $this->template();

            return ob_get_clean();

        } catch(\Throwable $exception) {
            throw $exception;
        }
    })->call($template);
}
```

