---
title: Standalone Types in PHP 8.2
date: 2022/12/22
description: Let's discover how the Standalone Types works in PHP 8.2!
tag: New in PHP 8.2
author: TheDevick
---

# The Standalone Types 😎

> The inability to type the unit type in PHP is a deficiency which should be resolved.

## The Example

An situation that every developer probably already passed through is when a database is down. In our example, we have an function that get the post from the database, but we're simulating that the database is down.

```php
function getPosts(): bool|array
{
    // Access the Database and get the Posts

    $posts = false; // Assigning the posts as false to simulate an error with the Database, for example

    return $posts;
}
```

Note that, the return of the function must be **False** or an **Array**. But, we can't define an false return type to a function, so we have to assign the **Bool** return type. Remember that this function will **Never Return True**.
Well, the [**Standalone Types**](https://www.php.net/releases/8.2/en.php#null_false_true_types), introduced in [**PHP 8.2**](https://www.php.net/releases/8.2/en.php) fixes that. In our example:

```php
function getPosts(): false|array
{
    // Access the Database and get the Posts

    $posts = false; // Assigning the posts as false to simulate an error with the Database, for example

    return $posts;
}
```

Our function will works in the same way, but now it's clear that this function will return just **False** or **Array**

## Others Standalone Types 🛒

You can use **Null** and **True** as standalone types too.

### Sources ✨

- [PHP Official Website](https://www.php.net/)
- [PHP 8.2 Release Announcement](https://www.php.net/releases/8.2/en.php)
- [PHP Standalone Types Documentation](https://www.php.net/releases/8.2/en.php#null_false_true_types)
