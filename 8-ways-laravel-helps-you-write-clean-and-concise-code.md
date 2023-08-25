# 8 ways Laravel helps you write clean and concise code

The definition of clean code is subjective. It's often a source of heated debates between developers on the web. For this article, we will focus on features in Laravel that enable us to write less of it.

## Facades

Facades in Laravel are extremely controversial. Yet, they provide a straightforward interface to help you achieve your goals.

Facades are proxies of instances stored in the container.

When you write `Cache::get('foo')`, here's how it goes: `Illuminate\Support\Facades\Facade` uses `__callStatic()` to redirect your call to whatever object instance is supposed to be resolved. If you look into `Illuminate\Support\Facades\Cache`, you will see that it's supposed to be bound to the string `cache`.

For this to make sense, I suggest you learn about Laravel's container first. But don't worry, it's not as complicated as it sounds. See it as a big smart box capable of creating instances of objects or at least, following your instructions to do so.

Here's how life was before Facades:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Cache\Repository;

class FooController extends Controller
{
    public function __construct(
        protected Repository $cache
    ) {
    }

    public function foo()
    {
        $bar = $this->cache->get('bar');
    }
}
```

It's called Dependency Injection. Some people are comfortable with this pattern, but others might want to move forward on their projects and use something simpler in the meantime, like Facades:

```php
<?php

namespace App\Http\Controllers;

class FooController extends Controller
{
    public function foo()
    {
        $bar = Cache::get('bar');
    }
}
```

Oh, and before you skip to the next section, beware that using the `cache()` helper function is the same as calling the `Cache` Facade. Let's say it's just a matter of preference.

Learn more on the official documentation: https://laravel.com/docs/10.x/facades#how-facades-work

## Implicit Binding

Instead of manually fetching resources in the database, why don't you delegate it to Laravel?

It happens in your *routes/web.php* file. You must change your way of declaring routes as follows:

```diff
-Route::get('/blog/{id}', [PostController::class, 'show']);
+Route::get('/blog/{post}', [PostController::class, 'show']);
```

Then, as long as you specified the `{post}` parameter (named after your Model) in your route declaration, you can get the Post resource directly, without explicitly requesting it from your database:

```diff
<?php

namespace App\Http\Controllers;

use App\Models\Post;

class PostController extends Controller
{
-   public function show(int $id)
+   public function show(Post $post)
    {
-       $post = Post::findOrFail($id);
-
        return view('posts.show', compact('post'));
    }
}
```

Obviously, under the hood, Laravel does request your database. But your code now looks decluttered and cleaner.

Learn more on the official documentation: https://laravel.com/docs/routing#implicit-binding

## Automatic Injection

## Redirect Routes

## View Routes

## Collections

## The Conditionable Trait

## Numerous First-Party Packages
