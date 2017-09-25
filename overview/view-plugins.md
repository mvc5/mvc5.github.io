### View Plugins
The default [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) supports [plugins](https://github.com/mvc5/mvc5/blob/master/config/service.php) and requires a [service locator](https://github.com/mvc5/mvc5/blob/master/src/Service/Service.php) to be injected prior to being [rendered](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Render.php). However, because a [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) can be created by a controller, this may not of happened. To overcome this, the current [service locator](https://github.com/mvc5/mvc5/blob/master/src/Service/Service.php) will be [injected](https://github.com/mvc5/mvc5/blob/master/src/View/Template/Model.php#L52) into the [view model](https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php) if it does not already have one. Below is an example of the [url generator](#url-generator).
```php
<?php

/** @var Mvc5\ViewModel $this */

echo $this->url('home');
```
