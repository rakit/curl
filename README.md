rakit-curl
==========

just another PHP cURL library

## Examples

#### Example Get Request
```php
use Rakit\Curl\Curl;

$request = new Curl('http://wikipedia.com');
$response = $request->get();

// then, you can do something with response object
if(! $response->error()) {

  $html = $response->getBody();
  $content_type = $response->getInfo('content_type');
  $cookie = $response->getCookie();
  $http_version = $response->getHeader('http_version');
  $all_headers = $response->getHeaders();
  $all_data = $response->toArray();

} else {

  $error_code = $response->getErrno();
  $error_message = $response->getErrorMessage();

}
```

#### Using Request Parameter
```php
use Rakit\Curl\Curl;

$request = new Curl('http://targetsite.com/products');
$request->param('page', 2);
$request->param('category', 'software'); 
$response = $request->get();

// do something with Response object

```

**or**

```php
use Rakit\Curl\Curl;

$params = array(
  'page' => 2,
  'category' => 'software'
);

$request = new Curl('http://targetsite.com/products');
$response = $request->get($params);

// do something with Response object

```

#### Example Post Request
```php
use Rakit\Curl\Curl;

$request = new Curl('http://targetsite.com/register');
$request->param('name', 'John Doe');
$request->param('email', 'johndoe@mail.com');
$request->param('password', '12345');
$response = $request->post();

// do something with Response object

```

#### Upload file? use addFile method

```php
use Rakit\Curl\Curl;

$request = new Curl('http://targetsite.com/register');
$request->param('name', 'John Doe');
$request->param('email', 'johndoe@mail.com');
$request->param('password', '12345');

$request->addFile('avatar', 'path/to/avatar.png');

$response = $request->post();

// do something with Response object
```

#### Send a cookie
```php
use Rakit\Curl\Curl;

$request = new Curl('http://targetsite.com/products');
$request->cookie('key', 'value');
$response = $request->get();

// do something with Response object

```

#### Create Simple Request

With simple request, you dont't need to create new Curl request object. 
But you can't send a cookie, modify request headers, or even curl options.

```php
use Rakit\Curl\Curl;

// simple GET
$response = Curl::doGet('http://targetsite.com', array('page' => 2));

// simple POST 
$response = Curl::doPost('http://targetsite.com/product/delete', array('id' => 5));
```
