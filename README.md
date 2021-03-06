# IpRestrict Stack middleware

[Stack](http://stackphp.com) middleware for allowing application access for specific IPs.


## Example
```php
require_once __DIR__ . '/../vendor/autoload.php';

use Alsar\Stack\RandomResponseApplication;
use Symfony\Component\HttpFoundation\Request;

$app = new RandomResponseApplication(1, 100);

$app = (new Stack\Builder())
    ->push('Alsar\Stack\IpRestrict', ['127.0.0.1'])
    ->resolve($app);

$request = Request::createFromGlobals();

$response = $app->handle($request);
$response->send();

$app->terminate($request, $response);
```

## Symfony example
```php
# web/app_dev.php

use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Debug\Debug;

$loader = require_once __DIR__.'/../app/bootstrap.php.cache';
Debug::enable();

require_once __DIR__.'/../app/AppKernel.php';

$kernel = new AppKernel('dev', true);
$kernel->loadClassCache();

$stack = (new Stack\Builder())
    ->push('Alsar\Stack\IpRestrict', ['127.0.0.1', 'fe80::1', '::1']);

$kernel = $stack->resolve($kernel);

Request::enableHttpMethodParameterOverride();
$request = Request::createFromGlobals();
$response = $kernel->handle($request);
$response->send();
$kernel->terminate($request, $response);
```
