---
title: Deprecated Dynamic Properties in PHP 8.2
date: 2022/12/20
description: Let talk about Dynamic Properties being deprecated in PHP 8.2!
tag: New in PHP 8.2
author: You
---

# The Dynamic Properties is being Deprecated in PHP 8.2 😲

> The creation of dynamic properties is deprecated to help avoid mistakes and typos

## First of All: What is Dynamic Properties in PHP? 🤔

Let's say we have our `Client` class in our codebase. An Client object must have **Name** and **Email**.

```
class Client
{
  public string $email;

  public function __construct(
    public string $name
  )
  {
  }
}
```

Then, let's create a Client and set an email.

```
$client = new Client("Erick");
$client->mail = "email@domain.com";
```

You've probably noticed that the Email property is `email`, but we're setting an `mail` property.
If we use [**var_dump**](https://www.php.net/manual/en/function.var-dump.php) in the class, we will note that PHP will **create** an public property in the class, called `mail` with `email@domain.com` as value, and maintain the `email` property uninitialized. Because of a typo, our code doesn't work as expected.

```
object(Client)#1 (2) {
  ["email"]=>
  uninitialized(string)
  ["name"]=>
  string(5) "Erick"
  ["mail"]=>
  string(16) "email@domain.com"
}
```

In my opinion, it's a functionality that create more bugs than features in our code.

## So, why is there Dynamic Properties in PHP, after all? 💁‍♂️

Well, some classes, such as `stdClass` depends of Dynamic Properties to work. An good example is the `json_decode` function, which return an `stdClass` that uses a lot of Dynamic Properties.

But let's agree that in modern code, we **rarely** use Dynamic Properties intentionally.

## Dynamic Properties being Deprecated 👓

Well, the PHP Developers Team decided that, in [PHP 8.2](https://www.php.net/releases/8.2/en.php), the Dynamic Properties will be deprecated.

**Remember**: A Functionality being deprecated doesn't means that it will stop working. Indeed, it will display an warning saying that this functionality is deprecated, then, in PHP 9.x, the Dynamic Properties will be discontinued. It will stop working.

---

In our example, if we run the code, it will print in terminal:

```
Deprecated: Creation of dynamic property Client::$mail is deprecated in /app/app.php on line 15
```

Note that the code still works. This is just a warning to the developer to prepare the code for the new version of PHP

## And if I still wanna use Dynamic Properties? 🙋‍♂️

Well, if you still want to use Dynamic Properties in a class, you can use the `#[AllowDynamicProperties]` attribute. In our example:

```
#[AllowDynamicProperties]
class Client
{
  public string $email;

  public function __construct(
    public string $name
  )
  {
  }
}
```

Now, you can still use Dynamic Property in the code.

### Sources ✨

- [PHP Official Website](https://www.php.net/)
- [PHP 8.2 Release Announcement ](https://www.php.net/releases/8.2/en.php)
- [PHP Deprecated Dynamic Properties Documentation](https://www.php.net/manual/en/migration82.deprecated.php#migration82.deprecated.core.dynamic-properties)
