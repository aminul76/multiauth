<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>

work process
first install laravel command line
composer create-project --prefer-dist laravel/laravel multiauth"5.6.*"

laravel auth command 

php artisan make:auth

resources>views>auth. auth folder copy past rename admin that means 
resources>views>admin

Controllers>Auth .Auth folder copy past remane Admin that means 
Controllers>Admin

make Admin model ands Admins tables command

php artisan make:model Admin -m
app>Admin.php. copy User.php past Admin.php
change 
protected $guard = 'admin';
from class User extends Authenticatable to class Admin extends Authenticatable

Admin for route group

Route::group(['prefix'=>'admin'],function(){

});

in route grop create route


    Route::get('/login', 'Admin\LoginController@showLoginForm')->name('admin.login');

create showLoginForm in Admin>LoginController
 
 public function showLoginForm(){
        return view('admin.login');
    }
    
create gust:admin in Admin>loginController
'guest:admin'

in route group create route 

Route::post('/login', 'Admin\LoginController@login')->name('admin.login.submit');

create login function in Admin>login

 public function login(Request $request)
    {
        $this->validate($request, [
            'email' => 'required|string',
            'password' => 'required|string',
        ]);
        $credential = [
            'email'=>$request->email,
            'password'=>$request->password,
        ];

        if(Auth::guard('admin')->attempt($credential, $request->member)){
            return redirect()->intended(route('admin.home'));
        }
        return redirect()->back()->withInput($request->only('email,remember'));
    }

and use Admin model ,use Auth namespace Admin,use reguest  
namespace App\Http\Controllers\Admin;
use Illuminate\Http\Request;
use App\Admin;
use Illuminate\Support\Facades\Auth;


set route resources>views>admin>login.blage.php

 action="{{ route('admin.login.submit') }}"

in admin route group create route
Route::get('/', 'AdminController@index')->name('admin.home');
Crate Controller AdminController command line

php artisan make:controller AdminController

in Controlles>AdminController create index function

 public function index()
    {
        return view('admin/admin_home');
    }

in resources>views>admin folder create file admin_home
copy home.blade.php past in admin_home.blade.php 

in admin route group create route 

 Route::get('/register', 'Admin\RegisterController@showRegistrationForm')->name('admin.register');

create showRegistrationForm in Admin>RegisterController

 public function showRegistrationForm()
    {
         return view('admin.register');
    }


in admin route gropu create route 

  Route::post('/register', 'Admin\RegisterController@register')->name('admin.register.submit');
 
in Admin>RegisterController

protected function create(array $data)
    {
        return Admin::create([
            'name' => $data['name'],
            'email' => $data['email'],
            'password' => Hash::make($data['password']),
        ]);
    }
only   change User to Admin 

and use namespace Admin, Admin model,

namespace App\Http\Controllers\Admin;

use App\Admin;


resources>views>admin . change route 
form route('register') to route('admin.register.submit')


url:localhost:8000/admin/login
localhost:8000/admin/register 


