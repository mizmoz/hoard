# Mizmoz Hoard

Storage Abstraction for use with Mizmoz Supermodel.

## Preface

The Repository pattern is a result of tightly coupled storage abstraction layers.

Here's an example of Repo usage in Laravel.

```

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

```
Hoard::default(new Eloquent('', '', ''));
```
