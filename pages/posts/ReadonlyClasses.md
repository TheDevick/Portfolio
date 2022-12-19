---
title: Readonly Classes in PHP 8.2
date: 2022/12/19
description: Let's learn how the Readonly Classes works in PHP 8.2!
tag: New in PHP 8.2
author: You
---

# Readonly Classes in PHP 8.2 📖

> Marking a class as readonly will add the readonly modifier to every declared property

In [PHP 8.1](https://www.php.net/releases/8.1/en.php), the developers team of PHP added the [Readonly Properties](https://www.php.net/manual/en/language.oop5.properties.php#language.oop5.properties.readonly-properties), which is a way to protect the properties, blocking any modification in his value after the Initialization.

```
class Client
{
  public function __construct(
		public readonly string $name,
		public readonly string $email
	)
	{
	}
}

$client = new Client("Erick", "email@domain.com");

$client->name = "TheDevick";    // This isn't allowed because the properties are readonly
$client->email = "other@mail.com" // This isn't allowed because the properties are readonly
```

It's cool, isn't it? But we can see that all the properties in the `Client` class are `readonly`.
Then, in PHP 8.2, you can say that a class is readonly, and all the declared properties will be readonly. Let's check:

```
readonly class Client
{
  public function __construct(
    public string $name,
    public string $email
  )
  {
  }
}

$client = new Client("Erick", "email@domain.com");

$client->name = "TheDevick";       // This isn't allowed because the class are readonly
$client->email = "other@mail.com"; // This isn't allowed because the class are readonly
```

Note that, in this example, we use the [Constructor property promotion](https://www.php.net/manual/en/language.oop5.decon.php#language.oop5.decon.constructor.promotion), a feature added in [PHP 8.0](https://www.php.net/releases/8.0/en.php)

### Sources ✨

- [PHP Official Website](https://www.php.net/)
- [PHP 8.2 Release Announcement ](https://www.php.net/releases/8.2/en.php)
- [PHP Readonly Classes Documentation](https://www.php.net/manual/en/language.oop5.basic.php#language.oop5.basic.class.readonly)
