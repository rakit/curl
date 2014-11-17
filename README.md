rakit-curl
==========

Just another PHP cURL wrapper

## Installation
As you can see, this library contain `composer.json` file. But at this time, i have not post this library on packagist. So if you want to use it via composer, you can manually add this repository in your `composer.json` file.

So, your `composer.json` file should look like this:

```json
{
    "require": {
        "rakit/curl": "dev-master"
    },
    "repositories": [
        {
            "url": "https://github.com/emsifa/rakit-curl",
            "type": "vcs"
        }
    ]
}
```
Then you can run `composer install` or `composer update`.

Optionally you can download/clone this library and load it into your project.


## Examples

#### Example Get Request
```php
use Rakit\Curl\Curl;

$request = new Curl('http://wikipedia.com');
$response = $request->get();

// then, you can do something with response object
if(! $response->error()) {

  // getting response file content
  $html = $response->getBody();
  
  // simply get response content type
  $content_type = $response->getContentType(); 
  
  // get content type via curl info
  $content_type = $response->getInfo('content_type'); 
  
  // getting response cookies
  $cookie = $response->getCookie();
  
  // simply get response header item
  $http_version = $response->getHeader('http_version');
  
  // get all header items
  $all_headers = $response->getHeaders();

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

#### Auto Redirect
Auto redirect allow cURL to auto redirect while response is redirection (status: 3xx). 

Example:
```php
use Rakit\Curl\Curl;

$request = new Curl('http://targetsite.com/admin/product/delete/1');
$request->autoRedirect(5); // 5 is maximum redirection

$response = $request->get();

// what if 6th request is also redirection? it's good to check
if($response->isRedirect()) {
    throw new \Exception("Too many redirection");
}

// do something with response
```

#### Storing Session
It is easy way to store session automatically with CURLOPT_COOKIEJAR and CURLOPT_COOKIEFILE. It is useful when you want to grab something that require session cookies to login or anything else. For use this, you must have directory that have access to write file for storing session.

For example you want grab redirected page after login:
```php
use Rakit\Curl\Curl;

$session_file = "path/to/store/sitetarget.session";

$request = new Curl("http://sitetarget.com/login");
$request->autoRedirect(5);
$request->storeSession($session_file);

$response = $request->post(array(
    'username' => 'my_username',
    'password' => 'my_password'
));

// do something with response object
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

## Response Object
In examples above, i always told you to do something with response object, but what it is? what you can do with response object?

Response object is object that returned from your cURL request. Response object contain result data from HTTP response such as response headers, cookies, body, etc.

Here is that you can do with response object:

#### isNotFound()
Shortcut for check response status code is 404 or not.
```php
$response = Curl::doGet("http://sitetarget.com/blablabla");

if($response->isNotFound()) {
   // 404 page not found
}
```

#### isRedirect()
Return true if status code is 3xx.
```php
// redirecting example

$response = Curl::doGet("http://sitetarget.com/redirect-me");

while($response->isRedirect()) {
    $redirect_url = $response->getHeader("location");
    $response = Curl::doGet($redirect_url);
}

```

#### getBody()
Getting response body like html code, image content, etc.

#### length()
Getting response length (response body string + response header string).

#### isHtml()
Return true if response content-type is text/html.

#### error()
Return true if there is error in request

#### getErrno()
Getting error code, 0 = no error.

#### getErrorMessage()
Getting error message, empty if no error

#### getHeader($key, $default = null)
Getting header item
```php
$response = Curl::doGet("http://sitetarget.com");

$http_version = $response->getHeader("http_version");
$content_type = $response->getHeader("content-type");
// and many more
```
#### getHeaders()
Getting all header items as array.

#### getHeaderString()
Get response headers as raw header string.

#### getStatusCode()
Getting response status code.

#### getContentType()
Shortcut for getting response content-type.

#### getInfo($key, $default = null)
Getting info item like curl_getinfo()

#### getAllInfo()
Getting all info as array.



