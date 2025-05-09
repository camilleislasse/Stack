# Configure your operations

Read the previous chapter to [configure your resource](configure_your_resource.md).

Now, with your fresh new resource, you have to define the operations that you need to implement.
There are some basic CRUD operations and more.

<!-- TOC -->
* [Configure your operations](#configure-your-operations)
  * [Basic operations](#basic-operations)
    * [Index operation](#index-operation)
    * [Use a grid for your index operation](#use-a-grid-for-your-index-operation)
    * [Show operation](#show-operation)
    * [Create operation](#create-operation)
    * [Update operation](#update-operation)
    * [Delete operation](#delete-operation)
    * [Bulk delete operation](#bulk-delete-operation)
    * [State machine operation](#state-machine-operation)
  * [Advanced configuration](#advanced-configuration)
    * [Configure the path](#configure-the-path)
    * [Configure the short name](#configure-the-short-name)
    * [Configure the templates' dir](#configure-the-templates-dir)
    * [Configure the routes' prefix](#configure-the-routes-prefix)
    * [Configure the section](#configure-the-section)
    * [Configure the resource identifier](#configure-the-resource-identifier)
    * [Configure the vars](#configure-the-vars)
<!-- TOC -->

## Basic operations

### Index operation

`Index` operation allows to browse all items of your resource.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Index;
use Sylius\Resource\Model\ResourceInterface;

#[AsResource(
    operations: [
        new Index(),
    ],
)]
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `index` operation.

| Name           | Method | Path   |
|----------------|--------|--------|
| app_book_index | GET    | /books |

On your Twig template, these variables are available

| Name              | Type                                      |
|-------------------|-------------------------------------------|
| resources         | Pagerfanta\Pagerfanta                     |
| books             | Pagerfanta\Pagerfanta                     |
| operation         | Sylius\Resource\Metadata\Index            |
| resource_metadata | Sylius\Resource\Metadata\ResourceMetadata |
| app               | Symfony\Bridge\Twig\AppVariable           |

### Use a grid for your index operation

To use a grid for you operation, you need to install
the [Sylius grid package](https://github.com/Sylius/SyliusGridBundle/)

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use App\Grid\BookGrid;
use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Index;

#[AsResource(
    operations: [
        // You can use either the FQCN of your grid
        new Index(grid: BookGrid::class),
        // Or you can use the grid name
        new Index(grid: 'app_book'),
    ],
)]
class Book implements ResourceInterface
{
}
```
{% endcode %}

On your Twig template, these variables are available

| Name              | Type                                                    |
|-------------------|---------------------------------------------------------|
| resources         | Sylius\Bundle\ResourceBundle\Grid\View\ResourceGridView |
| books             | Sylius\Bundle\ResourceBundle\Grid\View\ResourceGridView |
| operation         | Sylius\Resource\Metadata\Index                          |
| resource_metadata | Sylius\Resource\Metadata\ResourceMetadata               |
| app               | Symfony\Bridge\Twig\AppVariable                         |

The iterator for your books will be available as `books.data` or `resources.data`.

### Show operation

`Show` operation allows to view details of an item.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Show;

#[AsResource(
    operations: [
        new Show(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `show` operation.

| Name          | Method | Path        |
|---------------|--------|-------------|
| app_book_show | GET    | /books/{id} |    

On your Twig template, these variables are available

| Name              | Type                                      |
|-------------------|-------------------------------------------|
| resource          | App\Entity\Book                           |
| book              | App\Entity\Book                           |
| operation         | Sylius\Resource\Metadata\Show             |
| resource_metadata | Sylius\Resource\Metadata\ResourceMetadata |
| app               | Symfony\Bridge\Twig\AppVariable           |

### Create operation

`Create` operation allows to add a new item of your resource.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Create;

#[AsResource(
    operations: [
        new Create(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `create` operation.

| Name            | Method    | Path       |
|-----------------|-----------|------------|
| app_book_create | GET, POST | /books/new |

On your Twig template, these variables are available

| Name              | Type                                      |
|-------------------|-------------------------------------------|
| resource          | App\Entity\Book                           |
| book              | App\Entity\Book                           |
| operation         | Sylius\Resource\Metadata\Create           |
| resource_metadata | Sylius\Resource\Metadata\ResourceMetadata |
| app               | Symfony\Bridge\Twig\AppVariable           |

The iterator for your books will be available as `books.data` or `resources.data`.

### Update operation

`Update` operation allows to edit an existing item of your resource.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Update;

#[AsResource(
    operations: [
        new Update(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `update` operation.

| Name            | Method          | Path             |
|-----------------|-----------------|------------------|
| app_book_update | GET, PUT, PATCH | /books/{id}/edit |

On your Twig template, these variables are available

| Name              | Type                                      |
|-------------------|-------------------------------------------|
| resource          | App\Entity\Book                           |
| book              | App\Entity\Book                           |
| operation         | Sylius\Resource\Metadata\Update           |
| resource_metadata | Sylius\Resource\Metadata\ResourceMetadata |
| app               | Symfony\Bridge\Twig\AppVariable           |

### Delete operation

`Delete` operation allows to remove an existing item of your resource.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Delete;

#[AsResource(
    operations: [
        new Delete(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `delete` operation.

| Name            | Method | Path        |
|-----------------|--------|-------------|
| app_book_delete | DELETE | /books/{id} |

### Bulk delete operation

`Bulk delete` operation allows to remove several items of your resource at the same time.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\BulkDelete;

#[AsResource(
    operations: [
        new BulkDelete(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `bulk_delete` operation.

| Name                 | Method | Path               |
|----------------------|--------|--------------------|
| app_book_bulk_delete | DELETE | /books/bulk_delete |

### State machine operation

`State machine` operation allows to apply a transition to an item of your resource.

As an example, we add a `publish` operation to our book resource.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\ApplyStateMachineTransition;
use Sylius\Resource\Metadata\AsResource;

#[AsResource(
    operations: [
        new ApplyStateMachineTransition(stateMachineTransition: 'publish'),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

It will configure this route for your `apply_state_machine_transition` operation.

| Name             | Method | Path                |
|------------------|--------|---------------------|
| app_book_publish | GET    | /books/{id}/publish |    

## Advanced configuration

### Configure the path

It customizes the path for your operations.

{% code title="src/Entity/Customer.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Create;
use Sylius\Resource\Metadata\Update;

#[AsResource(
    operations: [
        new Create(path: 'register'),
        new Update(path: '{id}/edition'),
    ],
)
class Customer implements ResourceInterface
{
}
```
{% endcode %}

| Name            | Method    | Path                |
|-----------------|-----------|---------------------|
| app_book_create | GET, POST | /books/register     |   
| app_book_update | GET, POST | /books/{id}/edition | 

### Configure the short name

It customizes the path for your operations.

{% code title="src/Entity/Customer.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Create;

#[AsResource(
    operations: [
        new Create(shortName: 'register'),
    ],
)
class Customer implements ResourceInterface
{
}
```
{% endcode %}

| Name              | Method    | Path            |
|-------------------|-----------|-----------------|
| app_book_register | GET, POST | /books/register |    

It influences the path by default too, but you can still customize the path if needed.

### Configure the templates' dir

It defines the templates directory for your operations.

As an example, we defines `index`, `create`, `update` and `show` operations to our book resource.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Create;
use Sylius\Resource\Metadata\Index;
use Sylius\Resource\Metadata\Show;
use Sylius\Resource\Metadata\Update;

#[AsResource(
    templatesDir: 'book',
    operations: [
        new Index(),
        new Create(),
        new Update(),
        new Show(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

| Operation | Template Path                    |
|-----------|----------------------------------|
| index     | templates/books/index.html.twig  |  
| create    | templates/books/create.html.twig |   
| update    | templates/books/update.html.twig |   
| show      | templates/books/show.html.twig   |


{% hint style="info" %}
You can use `@SyliusAdminUi/crud` as templates dir from the [sylius/admin-ui](../admin-ui/getting-started.md) package.
{% endhint %}

| Operation | Template Path                        |
|-----------|--------------------------------------|
| index     | @SyliusAdminUi/crud/index.html.twig  |  
| create    | @SyliusAdminUi/crud/create.html.twig |   
| update    | @SyliusAdminUi/crud/update.html.twig |   
| show      | @SyliusAdminUi/crud/show.html.twig   |

### Configure the routes' prefix

It adds a prefix to the path for each operation.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\BulkDelete;
use Sylius\Resource\Metadata\Create;
use Sylius\Resource\Metadata\Delete;
use Sylius\Resource\Metadata\Index;
use Sylius\Resource\Metadata\Show;
use Sylius\Resource\Metadata\Update;

#[AsResource(
    routePrefix: 'admin',
    operations: [
        new Index(),
        new Create(),
        new Update(),
        new Delete(),
        new BulkDelete(),
        new Show(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

| Name                 | Method          | Path                     |
|----------------------|-----------------|--------------------------|
| app_book_index       | GET             | /admin/books/            |
| app_book_create      | GET, POST       | /admin/books/new         |                     
| app_book_update      | GET, PUT, PATCH | /admin/books/{id}/edit   |        
| app_book_delete      | DELETE          | /admin/books/{id}        |
| app_book_bulk_delete | DELETE          | /admin/books/bulk_delete |               
| app_book_show        | GET             | /admin/books/{id}        |

### Configure the section

It changes the route name for each operation.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\BulkDelete;
use Sylius\Resource\Metadata\Create;
use Sylius\Resource\Metadata\Delete;
use Sylius\Resource\Metadata\Index;
use Sylius\Resource\Metadata\Show;
use Sylius\Resource\Metadata\Update;

#[AsResource(
    section: 'admin',
    routePrefix: 'admin',
    operations: [
        new Index(),
        new Create(),
        new Update(),
        new Delete(),
        new BulkDelete(),
    ],
)
#[AsResource(
    section: 'shop',
    operations: [
        new Index(),
        new Show(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

| Name                       | Method          | Path                     |
|----------------------------|-----------------|--------------------------|
| app_admin_book_index       | GET             | /admin/books/            |
| app_admin_book_create      | GET, POST       | /admin/books/new         |                     
| app_admin_book_update      | GET, PUT, PATCH | /admin/books/{id}/edit   |        
| app_admin_book_delete      | DELETE          | /admin/books/{id}        |
| app_admin_book_bulk_delete | DELETE          | /admin/books/bulk_delete |    
| app_shop_book_index        | GET             | /books/                  |
| app_shop_book_show         | GET             | /books/{id}              |

### Configure the resource identifier

It changes the resource identifier for each operation.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\BulkDelete;
use Sylius\Resource\Metadata\Create;
use Sylius\Resource\Metadata\Delete;
use Sylius\Resource\Metadata\Index;
use Sylius\Resource\Metadata\Update;

#[AsResource(
    identifier: 'code',
    operations: [
        new Index(),
        new Create(),
        new Update(),
        new Delete(),
        new BulkDelete(),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

| Name                 | Method          | Path                     |
|----------------------|-----------------|--------------------------|
| app_book_index       | GET             | /admin/books/            |
| app_book_create      | GET, POST       | /admin/books/new         |                     
| app_book_update      | GET, PUT, PATCH | /admin/books/{code}/edit |        
| app_book_delete      | DELETE          | /admin/books/{code}      |
| app_book_bulk_delete | DELETE          | /admin/books/bulk_delete |

### Configure the vars

It defines the simple vars that you can use on your templates.

{% code title="src/Entity/Book.php" lineNumbers="true" %}
```php
namespace App\Entity;

use Sylius\Resource\Model\ResourceInterface;
use Sylius\Resource\Metadata\AsResource;
use Sylius\Resource\Metadata\Create;

#[AsResource(
    vars: [
        'header' => 'Library', 
        'subheader' => 'Managing your library',
    ],
    operations: [
        new Create(vars: [
            'subheader' => 'Adding a book',
        ]),
    ],
)
class Book implements ResourceInterface
{
}
```
{% endcode %}

You can use these vars on your Twig templates.
These vars will be available on any operations for this resource.

{% code %}
```html
<h1>{{ operation.vars.header }}<!-- Library --></h1>
<h2>{{ operation.vars.subheader }}<!-- Adding a book --></h2>
```
{% endcode %}
