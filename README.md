<div align="center">
 <img alt="Unostentatious Repository" src="https://repository-images.githubusercontent.com/240037373/291a4280-4e92-11ea-817d-dd947c29c107" width="300px" height="150px" />
</div>


<div align="center">
 
[![Latest Stable Version](https://poser.pugx.org/unostentatious/repository/v/stable?format=flat-square)](https://packagist.org/packages/unostentatious/repository)
[![Total Downloads](https://poser.pugx.org/unostentatious/repository/downloads?format=flat-square)](https://packagist.org/packages/unostentatious/repository)
[![License](https://poser.pugx.org/unostentatious/repository/license?format=flat-square)](https://packagist.org/packages/unostentatious/repository)



<br />

# Unostentatious Repository
An abstraction layer that let's you implement repository pattern for your models.

</div>
<br />

### Requirements
* PHP 7.4^
* Laravel 8.x / Lumen 8.x

### Installation
#### Step 1: Install through Composer

````shell script
composer require unostentatious/repository
````
---
#### Step 2: Publish the service provider
##### In Laravel:

1. In `Laravel`, edit `config\app.php` and add the provider under package service provider section:

````php
 /*
  * Package Service Providers...
  */
 \Unostentatious\Repository\Integration\Laravel\UnostentatiousRepositoryProvider::class,       
````

2. Then open your terminal, while in the `Laravel` app's root directory, publish the vendor:

````php
php artisan vendor:publish --provider="Unostentatious\Repository\Integration\Laravel\UnostentatiousRepositoryProvider"
````
---
##### In Lumen:

1. Copy the `unostent-repository.php` config file from `vendor/unostentatious/repository/Integration/config/` directory
2. If the `{root}/config/` directory is not existing in your Lumen app, make sure to create it first, paste the config file you just copied
3. Edit `bootstrap/app.php` then register the service provider and add the package's config explicitly like so:

````php
// Other actions...

$app->register(\Unostentatious\Repository\Integration\Laravel\UnostentatiousRepositoryProvider::class);
$app->configure('unostent-repository');
`````

---
#### Step 3: Custom Configurations
Right now the package's configuration is already residing to your app's config directory `/config`,
there are 3 values in the package's config that you can customize to fit your needs:

````php
<?php
declare(strict_types=1);

return [
    'root' => null,
    'destination' => null,
    'placeholder' => null
];
````

| Key                                    | Value                                                              
| -------------------------------------- | ---------------------------------------------------------------------
| **root**                               | The base directory in which the application is assigned the folder,
|                                        | and the structure of the repositories where will be based on.
|                                        |     sample value:                        
|                                        |        - Laravel: \app_path()
|                                        |        - Lumen: \base_path() . '/app'  
|                                        |
| **destination**                        | Define the repositories destination within the `{root}` directory.
|                                        |     sample value:
|                                        |        - 'Database'
|                                        |
|                                        | When a value is given ie `Database` the default path will be:
|                                        | `{root}/Database/{placeholder}
|                                        |
| **placeholder**                        | Define the repositories placeholder within the `{root}/{destination}`,
|                                        |      sample value:
|                                        |         - Repo 
|                                        | when a value is given ie `Repo` the default path will be:
|                                        |`{root}/{destination}/Repo`.
|                                        |
|                                        | The default value is null, which makes the folder structure into:
|                                        | `{root}/{destination}/Repositories`


#### Installation Done:
Viola! Just like that your ready to use `Unostentatious Repository` in your Laravel or Lumen application, happy coding!

-----------------------------

#### Usage:
When creating the repository classes it **MUST** reside on the specified path `{root}/{destination}/{placeholder}`, 
where in this case the default path will be `app/Database/Repositories`:

#### Step 1: Create the Repositories
It must be composed of a **concrete** class, and it's corresponding **interface**.

#### Step 2: Follow the convention
Then the concrete class **MUST** extend the **AbstractEloquentRepository** from the package.

See the example:

```php
<?php

namespace App\Database\Repositories;

use App\Database\Repositories\Interfaces\UserRepositoryInterface;
use Unostentatious\Repository\AbstractEloquentRepository;

class UserRepository extends AbstractEloquentRepository implements UserRepositoryInterface
{
    // Business logic goes here. 
}
```

#### Step 3: Load them to the IoC
Then after these classes has been written, just execute `composer dump-autoload` to invoke the IoC and these classes will now be injectable to consuming classes ie: `Controllers`.
