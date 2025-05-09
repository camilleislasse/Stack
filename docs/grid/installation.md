Installation
============

We assume you're familiar with [Composer](http://packagist.org), a
dependency manager for PHP. Use the following command to add the bundle
to your composer.json and download the package.

If you have [Composer installed globally](http://getcomposer.org/doc/00-intro.md#globally).

```bash
composer require sylius/grid-bundle
```

Otherwise, you have to download a .phar file.

```bash
curl -sS https://getcomposer.org/installer | php
php composer.phar require sylius/grid-bundle
```

Adding required bundles to the kernel
-------------------------------------

You need to enable the bundle inside the kernel.

If you're not using any other Sylius bundles, you will also need to add
`SyliusResourceBundle` and its dependencies to kernel. Don't worry,
everything was automatically installed via Composer.

{% code title="config/bundles.php" %}
```php
<?php

return [
    Sylius\Bundle\GridBundle\SyliusGridBundle::class => ['all' => true],
];
```
{% endcode %}

Congratulations! The bundle is now installed and ready to use. You need
to define your first resource and grid!
