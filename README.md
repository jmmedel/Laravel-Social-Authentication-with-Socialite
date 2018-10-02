# Laravel-Social-Authentication-with-Socialite
Laravel Social Authentication with Socialite


#Need to do 
- [x]  Create A Laravel project 
- [x]  Add .gitignore File 
- [x]  Npm and composer gererate key 
- [X]  Set .env file Create your database 
- [X]  Make Authentication 
- [X]  Migrating the Database 
- [X]  Fix the Error of there is and Error about Specified key was too long error
- [X]
- [X]
- [X]
- [X]
- [X]





# How to use Social-Authentication-with-Socialite
1. composer create-project --prefer-dist laravel/laravel Laravel_Socialite
2. Npm install composer install 
3. php artisan key:generate
4. .env file set your Database
5. php artisan make:auth
6. php artisan migrate 
7. if there is and error about Specified key was too long error 
    https://laravel-news.com/laravel-5-4-key-too-long-error

8. AppServiceProvider.php

```
    use Illuminate\Support\Facades\Schema;

public function boot()
{
    Schema::defaultStringLength(191);
}

```




