# Build a simple online store using Laravel Folio, Livewire v3, and Volt

What makes the Laravel ecosystem magical is that there are always new things to learn and experiment with. Some of the latest additions to it are Laravel Folio and Laravel Volt.

**[Laravel Folio](https://github.com/laravel/folio) offers a file-based approach to defining routes.**

**And [Laravel Volt](https://laravel.com/docs/volt) introduces single-file components and an optional new composition API to [Livewire v3](https://livewire.laravel.com).**

Embracing these new principles can enhance your productivity and transform how you approach web applications with Laravel. Let me show you by building a simple online store.

Before you start, check out [the live demo](https://dummy-store.benjamincrozat.com).

## Create your new Dummy Store project

Use Laravel's official installer to create your new project:

```bash
laravel new dummy-store
```

Once done, `cd` into the newly created folder:

```bash
cd dummy-store
```

Is the project ready to go? Let's check this out:

```bash
php artisan serve
```

If you see Laravel's welcome page on http://127.0.0.1:8000, we can now start bringing some companion packages to this project.

## Install Laravel Folio

Laravel Folio is a new approach to routing. Instead of declaring routes with lines of PHP, we create views following conventions.

Let's start by installing the package:

```bash
composer install laravel/folio:^1.0@beta
```

Once done, we must add the Service Provider (from which you can customize Folio's behavior and add middlewares), and create the *pages* directory that will be used by Folio:

```bash
php artisan folio:install
```

The new directory is located in *resources/views/pages*.

## Install Livewire v3 and Laravel Volt

**If you never tried Livewire before, I suggest you to level up your skills first. Laravel Volt can't be of any use to you without Livewire.**

Laravel Volt makes single-file components with Livewire v3 possible, and also adds an optional composition API.

When added to a project, it also brings Livewire v3 since it's entirely dependent on it. Let's install it using the following command:

```bash
composer install livewire/volt:^1.0@beta
```

Then, we finalize the process by publishing Volt's Service Provider:

```bash
php artisan volt:install
```

Let's not worry about what's inside for now. We won't need to change anything for this project.

## Make sure Laravel Folio is operational

To create your first page, I suggest that you move and rename *resources/views/welcome.blade.php* to *resources/views/pages/index.blade.php*.

Then, go into *routes/web.php* and remove the route declaration.

Finally, visit http://127.0.0.1:8000. If you still see Laravel's welcome page, it means Folio is ready to be used.

## Create the layout

```blade
```

## Create your first Livewire component using Volt

```bash
php artisan make:volt Cart
```

## Create the homepage

```blade
```

## Leverage anonymous Volt components for small things

```blade
```

## Transform your app to a SPA with wire:navigate

Our online store already feels pretty good. But what if I tell you that we can make it even better wire the lowest amount of effort possible?

Let's add the `wire:navigate` attribute on the link to the cart:

```diff
-<a href="/cart">
+<a href="/cart" wire:navigate>
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="inline-block mr-1 w-6 h-6 translate-y-[-2px]">…</svg>

    <span class="font-bold">
        {{ $count }}
    </span>
</a>
```

Before you can see for yourself how it feels, don't forget the link that goes back to the homepage in the cart:

```diff
-<a href="/">
+<a href="/" wire:navigate>
    ← Back
</a>
```

Now, test this in your browser. That's the cherry on top, isn't it?

## Conclusion

We saw how to orchestrate these three marvelous packages together and build something incredibly nice to use. You now have the basics knowledge to craft modern SPA-like web applications quickly, without being proficient with JavaScript.

[Access the code of this tutorial on GitHub.](https://github.com/benjamincrozat/dummy-store)
