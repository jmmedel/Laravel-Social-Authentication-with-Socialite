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
    This is the Start part 
- [X] Add socialite in the composer
- [X] config/app.php 
- [X]  Create Facebook App 
- [X]  Create Migration and Model
- [X]  Create New Routes
- [X]  Create New FacebookController
- [X]  Create Blade File







# How to use Social-Authentication-with-Socialite
1. composer create-project --prefer-dist laravel/laravel Laravel_Socialite
2. Npm install composer install 
3. php artisan key:generate
4. .env file set your Database
5. php artisan make:auth
6. php artisan migrate 
7. if there is and error about Specified key was too long error 
    https://laravel-news.com/laravel-5-4-key-too-long-error

8. composer require laravel/socialite
9. config/app.php
```

'providers' => [

	....

	Laravel\Socialite\SocialiteServiceProvider::class,

],

'aliases' => [

	....

	'Socialite' => Laravel\Socialite\Facades\Socialite::class,

],

```

10. https://developers.facebook.com/apps 
nd after create account you can copy client id and secret.

Now you have to set app id, secret and call back url in config file so open config/services.php and .env file then set id and secret this way:

config/services.php


```
return [
	....
    'facebook' => [
        'client_id' => env('FACEBOOK_CLIENT_ID'),
        'client_secret' => env('FACEBOOK_CLIENT_SECRET'),
        'redirect' => env('FACEBOOK_CALLBACK_URL'),
    ],
]

```

11. add env file 

```

FACEBOOK_CLIENT_ID=xxxxxxxxx
FACEBOOK_CLIENT_SECRET=xxxxxxx
FACEBOOK_CALLBACK_URL=http://localhost:8000/auth/facebook/callback


```


12. In this step first we have to create migration for add facebook_id in your user table. so let's create new migration and bellow column this way:

Migration:  php artisan make:migration AddNewColunmUsersTable

```

<?php


use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;


class AddNewColunmUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table("users", function (Blueprint $table) {
            $table->string('facebook_id');
        });
    }


    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table("users", function (Blueprint $table) {
            $table->dropColumn('facebook_id');
        });
    }
}


```

13. 

Now add addNew() in User model, that method will check if facebook id already exists then it will return object and if not exists then create new user and return user object. so open user model and put bellow code:

app/User.php

```

<?php


namespace App;


use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;


class User extends Authenticatable
{
    use Notifiable;


    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password', 'facebook_id'
    ];


    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];


    public function addNew($input)
    {
        $check = static::where('facebook_id',$input['facebook_id'])->first();


        if(is_null($check)){
            return static::create($input);
        }


        return $check;
    }
}

```

14.  Create New Routes
In this step we need to create routes for facebook login, so you need to add following route on bellow file.

routes/web.php

```

Route::get('facebook', function () {
    return view('facebook');
});
Route::get('auth/facebook', 'Auth\FacebookController@redirectToFacebook');
Route::get('auth/facebook/callback', 'Auth\FacebookController@handleFacebookCallback');


```

15. 
Create New FacebookController

we need to add new controller and method of facebook auth that method will handle facebook callback url and etc, first put bellow code on your FacebookController.php file.

app/Http/Controllers/Auth/FacebookController.php


```

<?php


namespace App\Http\Controllers\Auth;


use App\User;
use App\Http\Controllers\Controller;
use Socialite;
use Exception;
use Auth;


class FacebookController extends Controller
{


    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function redirectToFacebook()
    {
        return Socialite::driver('facebook')->redirect();
    }


    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function handleFacebookCallback()
    {
        try {
            $user = Socialite::driver('facebook')->user();
            $create['name'] = $user->getName();
            $create['email'] = $user->getEmail();
            $create['facebook_id'] = $user->getId();


            $userModel = new User;
            $createdUser = $userModel->addNew($create);
            Auth::loginUsingId($createdUser->id);


            return redirect()->route('home');


        } catch (Exception $e) {


            return redirect('auth/facebook');


        }
    }
}

```


16.  Create Blade File

Ok, now at last we need to add blade view so first create new file facebook.blade.php file and put bellow code:

resources/views/facebook.blade.php

```
@extends('layouts.app')


@section('content')
<div class="container">
  <div class="row">
        <div class="col-md-12 row-block">
            <a href="{{ url('auth/facebook') }}" class="btn btn-lg btn-primary btn-block">
                <strong>Login With Facebook</strong>
            </a>     
        </div>
    </div>
</div>
@endsection

```

17. Ok, now you are ready to use open your browser and check here : URL + '/facebook'.

I hope it can help you.....