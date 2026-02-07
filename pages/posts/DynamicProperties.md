---
title: Deprecated Dynamic Properties in PHP 8.2
date: 2022/12/20
description: Let's talk about Dynamic Properties being deprecated in PHP 8.2!
tag: New in PHP 8.2
author: TheDevick
---

# The Dynamic Properties is being Deprecated in PHP 8.2 👮‍♂️

> The creation of dynamic properties is deprecated to help avoid mistakes and typos

## First of All: What is Dynamic Properties in PHP? 🤔

Let's say we have our `Client` class in our codebase. An Client object must have **Name** and **Email**.

```php
class Client
{
  public function __construct(
    public string $name,
    public string $email
  )
  {
  }
}

$client = new Client("Erick", "erick@abellabilhalba.eng.br");
$client->age = 15;
```

You've probably noticed that we're attributing an `age` property to the object, but the class `Client` doesn't have an `age` property.
If we use [**var_dump**](https://www.php.net/manual/en/function.var-dump.php) in the object, we will note that PHP will **create** an property in the class, called `age` with 15 as value.

```txt
object(Client)#1 (3) {
  ["name"]=>
  string(5) "Erick"
  ["email"]=>
  string(21) "erick@abellabilhalba.eng.br"
  ["age"]=>
  int(15)
}
```

In my opinion, it's a functionality that create more bugs than features in our code, and in modern code, we **rarely** use Dynamic Properties intentionally.

## So, why is there Dynamic Properties in PHP, after all? 🤔

Well, some built-in classes, such as `stdClass` depends of Dynamic Properties to work. An `stdClass` means an [**Standard Class**](https://www.php.net/manual/en/class.stdclass.php). An Standard Class is a class without any properties. Then, the Dynamic Properties are used to attribute properties into the object.
The Standard Class are used also in [Json Decode](https://www.php.net/manual/pt_BR/function.json-decode.php) Function and [Object Casting](https://www.php.net/manual/en/language.types.object.php#language.types.object.casting).

## Dynamic Properties being Deprecated 🙅‍♂️

Well, the PHP Developers Team decided that, in [PHP 8.2](https://www.php.net/releases/8.2/en.php), the Dynamic Properties will be deprecated.

**Remember**: A Functionality being deprecated doesn't means that it will stop working. Indeed, it will display an warning saying that this functionality is deprecated, then, in PHP 9.x, the Dynamic Properties will be discontinued. It will stop working.

---

In our example, if we run the code, it will print in terminal:

```txt
Deprecated: Creation of dynamic property Client::$age is deprecated in /app/App.php on line 14
```

Note that the code still works. This is just a warning to the developer to prepare the code for the new version of PHP

## And if I still wanna use Dynamic Properties? 🙋‍♂️

Well, if you still want to use Dynamic Properties in a class, you can use the `#[AllowDynamicProperties]` attribute. The Standard Class, for example, uses this attribute. In our example:

```php
#[AllowDynamicProperties]
class Client
{
  public function __construct(
    public string $name,
    public string $email
  )
  {
  }
}

$client = new Client("Erick", "erick@abellabilhalba.eng.br");
$client->age = 15; // This will no longer be discontinued
```

### Sources ✨

- [PHP Official Website](https://www.php.net/)
- [PHP 8.2 Release Announcement](https://www.php.net/releases/8.2/en.php)
- [PHP Deprecated Dynamic Properties Documentation](https://www.php.net/manual/en/migration82.deprecated.php#migration82.deprecated.core.dynamic-properties)
