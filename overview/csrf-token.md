## CSRF Token
A CSRF [token](https://github.com/mvc5/mvc5/blob/master/src/Session/CSRFToken/Generate.php#L25) is used to [protect](https://github.com/mvc5/mvc5/blob/master/src/Route/Match/CSRFToken.php#L43) routes against [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet) attacks. A new token is [generated](https://github.com/mvc5/mvc5/blob/master/src/Session/CSRFToken/Generate.php) every time a new PHP session is [created](https://github.com/mvc5/mvc5/blob/master/config/service.php#L72) for the user. The token is then added to a `POST` form using a hidden HTML input element. The `csrf_token` helper function can be used to retrieve the current token. 
```html
<input type="hidden" name="csrf_token" value="<?php echo htmlspecialchars($this->csrf_token()); ?>">
```
The HTTP methods `GET`, `HEAD`, `OPTIONS` and `TRACE`, are considered "safe" and do not require a CSRF token. Safe HTTP methods should not be used to change the state of the application. Any other HTTP method is considered "unsafe" and requires a CSRF token to be sent with the request, either as a `POST` parameter, or using the `X-CSRF-Token` HTTP header. A `403 Forbidden` HTTP Error is returned when the token is not valid.
```php
new Request([
    'method' => 'POST', 
    'data' => ['csrf_token' => '882023fdc5f837855a...'],
    'headers' => ['X-CSRF-Token' => '882023fdc5f837855a...'],
]);
```
Routes can be configured not to verify the CSRF token by setting the `csrf_token` route attribute to `false`. Child routes inherit the `csrf_token` value of a parent route.  
```php
'api' => [
    'path' => '/api',
    'controller' => Api\Controller::class,
    'csrf_token' => false,
],
```
