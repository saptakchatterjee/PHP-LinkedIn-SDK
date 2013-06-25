PHP-LinkedIn-SDK
================

A PHP wrapper for the LinkedIn API


Usage
======

Instantiate the class
```php
$li = new LinkedIn(array(
  'api_key' => 'YOUR_API_KEY',
  'api_secret' => 'YOUR_API_SECRET',
  'callback_url' => 'http://www.yourcallback.url/goeshere'
));

```

To get the login url and send the user to authenticate:
```php
$url = $li->getLoginUrl(array(LinkedIn::SCOPE_BASIC_PROFILE, LinkedIn::SCOPE_EMAIL_ADDRESS));
header("Location: {$url}");
```

After the user has authenticated, they will be redirected to 'callback_url' with 'code' and 'state' in the url. State is a unique identifier for the user. You must make sure the 'state' set during getLoginUrl is the same that is returned.
```php
$url = $li->getLoginUrl(array(LinkedIn::SCOPE_BASIC_PROFILE, LinkedIn::SCOPE_EMAIL_ADDRESS));
$_SESSION['li-state'] = $li->getState();
header("Location: {$url}");
```

You can also manually set the state if you wish
```php
$state = uniqid();
$url = $li->getLoginUrl(array(LinkedIn::SCOPE_BASIC_PROFILE, LinkedIn::SCOPE_EMAIL_ADDRESS), $state);
header("Location: {$url}");
```

Get the access token
```php
$token = $li->getAccessToken($_REQUEST['code']); 
$token_expires = $li->getAccessTokenExpiration();
$_SESSION['token'] = $token;
$_SESSION['token_expires_on'] = time() + $token_expires;
```

GET some data
```php
$info = $li->get('/people/~');
$specific = $li->get('/people/~:(first-name,last-name,positions)');
```
GET with payload
```php
$info = $li->get('/people/~/connections', array('modified' => 'new', 'modified-since' => 1267401600000));
```

POST 
```php
$response = $li->post($endpoint, array $data);
```

PUT 
```php
$response = $li->put($endpoint, array $data);
```
