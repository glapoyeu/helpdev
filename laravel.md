## Generate API key
php artisan key:generate

## Generate migration 
docker-compose exec transportapp php artisan make:migration create_modules_table --create=module

## migrate
docker-compose exec transportapp php artisan migrate

## Generate seeder
php artisan make:seeder ModulesTableSeeder

## add new dependencie with composer on container docker
docker-compose exec transportapp composer require laravel/sanctum

## migrate and seed
php artisan migrate:fresh --seed

## Generate migration foreign key from table
php artisan make:seeder PermissionsTableSeeder


## Update laravel 8
https://blog.laravel.com/laravel-php-8-support
There are also a couple of commonly used dependencies you'll need to update in your composer.json file: 

- PHP to php:^8.0
- Faker to fakerphp/faker:^1.9.1
- PHPUnit to phpunit/phpunit:^9.3
