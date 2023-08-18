# Build a simple online store using Laravel Folio, Livewire v3, and Volt

What makes the Laravel ecosystem magical is that there are always new things to learn and experiment with. Some of the latest additions to it are Laravel Folio and Laravel Volt.

**[Laravel Folio](https://github.com/laravel/folio) offers a file-based approach to defining routes.**

**And [Laravel Volt](https://laravel.com/docs/volt) introduces single-file components and a new composition API to [Livewire v3](https://livewire.laravel.com).**

Embracing these new principles can enhance your productivity and transform how you approach web applications with Laravel. Let me show you.

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

```bash
composer install laravel/folio:^1.0@beta
```

Once done, we must add the Service Provider (from which you can customize Folio's behavior), and create the *pages* directory that will be used by Folio:

```bash
php artisan folio:install
```

The new directory is located in *resources/views/pages*.

## Install Livewire v3 and Laravel Volt

When added to a project, Laravel Volt also installs Livewire v3 since it's entirely dependent on it.

```bash
composer install livewire/volt:^1.0@beta
```

## Make sure Laravel Folio is operational

To create your first page, I suggest that you move and rename *resources/views/welcome.blade.php* to *resources/views/pages/index.blade.php*.

Then, go into *routes/web.php* and remove the route declaration.

Finally, visit http://127.0.0.1:8000. If you still see Laravel's welcome page, it means Folio is ready to be used.
