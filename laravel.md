## Generate API key
php artisan key:generate

## Generate migration 
docker-compose exec transportapp php artisan make:migration create_modules_table --create=module

## migrate
docker-compose exec transportapp php artisan migrate

## add new dependencie with composer on container docker
docker-compose exec transportapp composer require laravel/sanctum

## migrate and seed
php artisan migrate:fresh --seed

