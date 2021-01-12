## Generate API key
php artisan key:generate

## Generate migration 
docker-compose exec transportapp php artisan make:migration create_modules_table --create=module

## migrate
docker-compose exec transportapp php artisan migrate