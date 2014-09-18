rakit-curl
==========

just another PHP cURL library

## Examples

#### Example Get Request
```php
use Rakit\Curl\Curl;

$request = new Curl('http://wikipedia.com');
$response = $request->get();

if(! $response->error()) {

  // then, you can do something with response object
  $html = $response->getBody();
  $content_type = $response->getInfo('content_type');
  $cookie = $response->getCookie();
  $http_versison = $response->
  $all_headers = $response->getHeaders();
  
  
  $all_data = $response->toArray();

} else {
  
  $error_code = $response->getErrno();
  $error_message = $response->getErrorMessage();
  
}
```
#### Example Post Request
```php
use Rakit\Curl\Curl;

$request = new Curl('http://yoursite.com/register');
$request->param('name', 'John Doe');
$request->param('email', 'johndoe@mail.com');
$request->param('password', '12345');
$response = $request->post();

// do something with Response object

```

#### Upload file? use addFile method

```php
use Rakit\Curl\Curl;

$request = new Curl('http://yoursite.com/register');
$request->param('name', 'John Doe');
$request->param('email', 'johndoe@mail.com');
$request->param('password', '12345');

$request->addFile('avatar', 'path/to/avatar.png');

$response = $request->post();

// do something with Response object
```

#### Create Simple Request

With simple request, you dont't need to create new Curl request object.

```php
use Rakit\Curl\Curl;

// simple GET
$response = Curl::doGet('http://yoursite.com', array('page', 2));

// simple POST 
$response = Curl::doPost('http://yoursite.com/product/delete', array('id', 5));
```
