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
- [X]  If there is no error all fix and try the login page reset all the migration 
- [X] Edit the migration for twiiter 
- [X] add provider and provider_id to the list of mass assignable attributes in the User model.


- [X] Add socialite package 
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
9. php artisan migrate:reset 

10.  
 
 If you look inside database/migrations you will notice that the generated laravel app already came with migrations for creating the users table and password resets table.

Open the migration for creating the users table. There are a few things I want us to tweak before creating the users table.

A user created via OAuth will have a provider and we also need a provider_id which should be unique. With Twitter, there are cases where the user does not have an email set up and thus the hash sent back by the callback won't have an email resulting to an error. To counter this, we will set the email field to nullable. Also, when creating users via Oauth we won't be passing any passwords therefore we need to set the password field in the migration nullable.

Here, we are doing four things: adding provider and provider_id columns to the users table (note the provider_id column should be set to unique). We also make the email and password fields in the users table nullable.

The up method in your migration should now look like this:

database/migrations/{titmestamp}createusers_table.php

```


public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('email')->nullable();
            $table->string('password', 60)->nullable();
            $table->string('provider');
            $table->string('provider_id');
            $table->rememberToken();
            $table->timestamps();
        });
    }


```


11. php artisan migrate 

12. app/User.php

```

protected $fillable = [
        'name', 'email', 'password', 'provider', 'provider_id'
    ];

```









