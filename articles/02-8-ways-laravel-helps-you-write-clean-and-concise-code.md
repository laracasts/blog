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

It's called Dependency Injection. Some people are comfortable with this pattern and it's perfectly fine to do it that way. But others, less familiar with it, might want to move forward on their projects and use something simpler in the meantime, like Facades:

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

Laravel can magically inject dependencies into your code. OK, let me explain, because this sounds way more complicated than it is.

You surely have delt with a request object in Laravel, right? And I bet you never had to create the instance yourself. Most of the time, you get it via your controller this way:

```php
<?php

namespace App\Http\Controllers;

class PostController extends Controller
{
    public function store(Request $request)
    {
        //
    }
}
```

But how does it work? Again, it's not magic. Using [PHP's reflection capabilities](https://www.php.net/manual/en/book.reflection.php) under the hood, Laravel automatically injects an instance of whatever you typehinted in your `store()` method (not just `Illuminate\Http\Request`).

If you really want to, you can even make it happen in the constructor:

```php
<?php

namespace App\Http\Controllers;

class PostController extends Controller
{
    public function __construct(
        public Request $request
    ) {
    }

    public function store()
    {
        $this->request->validate(…);

        //
    }
}
```

This not only works in controllers, but also in console commands, jobs, or whatever else you'd need to.

## Redirect Routes

Laravel's router contains a few surprises that will delight people who are always looking to unclutter their code.

For instance, let's say you want to redirect an old URL to a new one. Would you create an entire controller just for this? I don't know about you, but I wouldn't. Here's one way you can do it:

```php
Route::get('/old', function () {
    return redirect('/new');
});
```

Or even shorter thanks to short closures:

```php
Route::get('/old', fn () => redirect('/new'));
```

You can even go a step further and use a more explicit way of doing it:

```php
Route::redirect('/old', '/new');
```

Cool, huh?

## View Routes

In the same vein, Laravel can also accommodate you when you only want to create a route that serves a simple view. Instead of creating a controller or a closure-based route, use the `view()` method instead:

```php
Route::view('/about', 'pages.about');
```

The */about* path will now serve the view located in _resources/views/pages/about_.

## Collections

[Collections](https://laravel.com/docs/collections) in Laravel are essentially a wrapper around arrays, providing a whole bunch of really useful methods for manipulating them, and a more consistent experience compared to native PHP functions.

Want to loop through an array? Wrap it in a collection and use a more modern object-oriented syntax to do so:

```php
collect([1, 2, 3])->each(function ($item) {
    //
});
```

Want to send the user's invoices by email? Use [higher-order messages](https://laravel.com/docs/collections#higher-order-messages) to keep your code tidy:

```php
$user->invoices()->get()->each->sendByEmail();
```

Not only collections are more readable and concise, but also more intuitive. You will never ask yourself if the needle comes first or not again!

## The Conditionable Trait

The Conditionable trait is a very simple piece of code that contains only two methods: `when()`, and `unless()`.

```php
namespace Illuminate\Support\Traits;

use Closure;
use Illuminate\Support\HigherOrderWhenProxy;

trait Conditionable
{
    public function when($value = null, callable $callback = null, callable $default = null)
    {
        …
    }

    public function unless($value = null, callable $callback = null, callable $default = null)
    {
        …
    }
}
```

It's used accross may classes in the framework such as `Builder`, `Factory`, `Filesystem`, `Logger`, `PendingRequest`, `Carbon`, and many others to offers a way to conditionally apply logic using a fluent API instead of if statements.

One common use case is to use it with Eloquent's query builder. Instead of doing this:

```php
$query = Model::query();

if ($something) {
    $query->where('something', true);
} else {
    $query->where('something_else', true);
}

$models = $query->get();
```

You could write more fluent and clean looking code:

```php
$models = Model::query()
    ->when(
        $something,
        fn ($query) => $query->where('something', true),
        fn ($query) => $query->where('something_else', true),
    )
    ->get();
```

I like this a lot. What about you?

## Numerous First-Party Packages

One of the greatest strengths of Laravel we still haven't talked about is its vast collection of [first](https://laravel.com/docs) and [third-party packages](https://packagist.org).

These packages take care of common tasks, saving developers the nightmare of doing all the work from scratch. Imagine billing, authentication, debugging, feature flags, and so much more, just a `composer require` away.

By leveraging the available packages, developers can defer responsibilities onto others who might have more experience, and who regularly work on the packages. This not only saves time and effort but also ensures that the codebase remains clean and concise.

And after all, why reinvent the wheel when there are experts who have already built and refined the components you need?

## Conclusion

No matter how your coding style, the only things that matter are results and having fun. Experiment with the `Conditionable` trait and see how it goes!
