# Mizmoz Hoard

Storage Abstraction for use with Mizmoz Supermodel.

## Preface

The Repository pattern is a result of tightly coupled storage abstraction layers.

Here's an example of Repo usage in Laravel.

```php
interface UserRepoInterface
{
    public function find($userId);
}

class UserRepoEloquent implements UserRepoInterface
{
    public function find($userId)
    {
        return User::find($userId)->toArray(); 
    }
}

App::bind('UserRepoInterface', function ($app) {
    return new UserRepoEloquent();
});

class UserController extends Controller
{
    public function getIndex($userId, UserRepoInterface $userRepo)
    {
        return $userRepo->find($userId);
    }
}
```

You might be thinking "Great! Now I can switch out Eloquent and all I need to do is create a new UserRepoFile... and Products, ProductData, Images etc... wait?!". Also in order to completely remove the Eloquent dependency we're now reduced to working with arrays. We could return a `UserModel` from UserRepoEloquent::find but now for each model we want we're creating 1 interface and 4 classes.

## The fix

Really what we need is a storage abstraction layer that returns Models that are completely oblivious to the storage backend, say hello to Supermodel.

Hoard allows you to use your favourite storage engine and return Supermodels, models that can be as open or closely defined as you want. If you need a basic key, value store, fine, or if you need your models to be validated, no problem!

```php
class User extends Model
{
}

// Tell Hoard which adapter to use by default
Hoard::default(new Eloquent('mysql', 'localhost', 'dbuser', 'dbpass'));

// Or if you're testing
Hoard::test();

// Create a new User model
$user = new User(['name' => 'Ian']);

// Save
Hoard::save($user);

// Find
$user = Hoard::find($user);

// Now we have a nice User model to play with
$user->name = 'Bob';
```
