--- 
title: Sensitive Parameter Attribute in PHP 8.2
date: 2022/12/21
description: Let's learn how the new Sensitive Parameter works in PHP 8.2! 
tag: New in PHP 8.2 
author: TheDevick 
--- 

# Sensitive Parameter is being added in PHP 8.2! 🎉
> An sensitive Parameter have its value hidden if present in a stack trace.

## An Secret Parameter 🕵️‍♂️

Let's have an example that we're making a request to an Api that needs an **Api Token**, and the Application will throw an Exception if have any error with this Api Token.

```php
function makeApiRequest(string $apiToken): Response
{
    $client = new Client();
    $res = $client->get('https://api.example.com/user', [
        'auth' => ['apiToken', $apiToken]
    ]);

    if($res->getStatusCode() === 403) {
        throw new Exception("Wrong Api Token");
    }

    return $res;
}
```

If we run this code, and the example Api returns the 403 status code, the application will trow an **Wrong Api Token** Exception:

```txt
Fatal error: Uncaught Exception: Wrong Api Token in /app/App.php:15
Stack trace:
#0 /app/App.php(21): makeApiRequest('Secret Token')
#1 {main}
  thrown in /app/App.php on line 15
```

You can note that in the Stack Trace, the "Secret Token" value is **Appearing**. Well, this can be an **Big Security Problem**.

## The Sensitive Parameter Attribute 🤿

The new Sensitive Parameter Attribute, added in PHP 8.2 focus on solve this problem. We can target an Property with this Attribute and it will be **Hidden** in the Stack Trace. In our case:

```php
function makeApiRequest(#[SensitiveParameter] string $apiToken): Response
```

The Logs:

```txt
Fatal error: Uncaught Exception: Wrong Api Token in /app/App.php:15
Stack trace:
#0 /app/App.php(21): makeApiRequest(Object(SensitiveParameterValue))
#1 {main}
  thrown in /app/App.php on line 15
```

We can see that, now, the Secret Api Key **isn't appearing anymore**. Instead, have an `SensitiveParameterValue` object. This object have an interesting feature: the `getValue()` method, which allows the Developer to get the hidden value of the parameter.

### Sources ✨

- [PHP Official Website](https://www.php.net/)
- [PHP 8.2 Release Announcement](https://www.php.net/releases/8.2/en.php)
- [PHP Sensitive Parameter Documentation](https://www.php.net/manual/en/class.sensitive-parameter.php)
