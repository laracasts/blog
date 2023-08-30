# Start testing your code in less than 5 minutes with Laravel

Coding is fun, but debugging? Not so much. That's why testing is crucial for the success of your project. I will show you how easy it is to start testing your Laravel applications. Let's break the ice once and for all!

## Here's why you want to write tests for your projects

Writing tests for your projects is essential for various and very good reasons:

- **Fix basic bugs**: Before they ruin the experience for your users, you can catch and fix basic bugs during the development phase.
- **More confidence for deployments**: With a well-tested application, you can deploy with confidence, knowing that your code is solid.
- **Less time spent in your web browser or HTTP client**: Doing the same thing over and over manually can be time-consuming. Automated testing saves you a lot of time and effort.
- **Happier customers**: A well-tested application leads to a smoother user experience, which results in happier customers and less churn.
- **Happier clients**: Clients appreciate an application free of obvious bugs, just as in your sales pitch!
- **Happier employer**: A well-tested, well-functioning application will make your employer happy as well. It shows professionalism and attention to detail. It can only be good for your career.

## Create a new project

Creating a new project in Laravel with Pest is as simple as running a single command. Open your terminal and run:

```bash
laravel new hello-world --pest
```

This will create a new Laravel project with Pest installed and ready to be used. `cd` into the newly created folder, make sure you can display Laravel's welcome page in your browser using whichever environment you like, and read to the next section!

## Make sure Pest can run

Once the project is created, you can run your tests to make sure everything is working as expected.

Check out the _ExampleTest.php_ file in _tests/Feature_. It clearly states that it will test for the homepage to return a 200 HTTP status code.

Run your tests using the following command:

```bash
php artisan test
```

Everything should be green. And if your environment was properly working, I bet 5 minutes was more than enough! If not, don't be discouraged. Spending some time in making sure your environment is correctly configured is a great investment.

Now, any moment something breaks during the process of rendering the home page, the test will fail. Try to change some code incorrectly, run `php artisan test` again, and you will see it for yourself.

## Create a tested contact form

Practice makes perfect. Let's see how we can write more advanced tests now that we saw how easy it is.

### Create the controller

To get started with our contact form, let's create a single-action controller by using the `--invokable` flag: 

```bash
php artisan make:controller SendContactEmailController --invokable
```

### Create the routes

Then, we must declare two routes: one that answers _GET_ requests on the _/contact_ path and displays the form, and the other using _POST_ that processes the user's input and sends the message:

```php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\SendContactEmailController;

Route::view('/contact', 'contact');
Route::post('/contact', SendContactEmailController::class);
```

### Create and set up the Mailable

Creating a mailable is as easy as creating a controller using the following command:

```bash
php artisan make:mail ContactMail
```

Then, let's modify it a bit with three properties. For that, we'll use constructor property promotion, which makes us write less code. Then, We need to build a new Envelope object and pass relevant information about the sender.

```php
namespace App\Mail;

…
use Illuminate\Mail\Mailables\Address;

class ContactMail extends Mailable
{
    use Queueable, SerializesModels;

    public function __construct(
        protected string $from,
        protected string $name,
        protected string $message
    ) {
    }

    public function envelope(): Envelope
    {
        return new Envelope(
            from: new Address($this->from, $this->name),
            subject: 'Contact Mail',
        );
    }
}
```

### Create the form

```bash
touch resources/views/contact.blade.php
```

```blade
<form method="POST" action="/contact">
    @csrf

    <div>
        <label for="name">Name</label>
        <input type="name" id="name" name="name" value="{{ old('name') }}" />
    </div>

    <div>
        <label for="email">Email</label>
        <input type="email" id="email" name="email" value="{{ old('name') }}" />
    </div>

    <div>
        <label for="message">Message</label>
        <textarea id="message" name="message">{{ old('message') }}</textarea>
    </div>

    <button>
        Send
    </button>
</form>
```

### Send the email

```php
namespace App\Http\Controllers;

use App\Mail\ContactMail;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Mail;

class SendContactEmailController extends Controller
{
    public function __invoke(Request $request)
    {
        $validated = $request->validate([
            'email' => 'required|email',
            'name' => 'required|string',
            'message' => 'required|string',
        ]);

        Mail::to($validated['email'])
            ->send(new ContactMail(
                $validated['name'],
                $validated['message'],
            ));

        return back()->with('status', 'Your email has been sent!');
    }
}
```

### Create the tests

```bash
php artisan make:test SendContactEmailTest --pest
```

```php
<?php

use App\Mail\ContactMail;
use Illuminate\Support\Facades\Mail;
use function Pest\Laravel\{get,post};

test('the contact page works', function () {
    get('/contact')
        ->assertOk();
});

test('the contact email can be sent', function () {
    Mail::fake();

    post('/contact', [
        'name' => fake()->name(),
        'email' => fake()->safeEmail(),
        'message' => fake()->paragraph(),
    ])
        ->assertRedirect();

    Mail::assertSent(ContactMail::class);
});

test('the contact email requires a name', function () {
    post('/contact', [
        'email' => fake()->safeEmail(),
        'message' => fake()->paragraph(),
    ])
        ->assertInvalid(['name' => 'required']);
});

test('the contact email requires a valid name', function () {
    …
});

…
```

## Be pragmatic when writing tests

My strategy when writing tests has been the same for years:
1. Write for all the happy paths. An example of a happy path would be "the contact email can be sent."
2. Write for the obvious unhappy paths like "the contact email requires a name."
3. That's it. Don't put too much effort into writing tests if your app doesn't have users yet. It's difficult to anticipate what could go wrong and your time and energy could be invested better elsewhere. Write tests for the bugs your users will actually encounter. Because let's face it, no software is flawless.

## The case of untested existing projects

Existing and successful projects are usually big, and I stumbled upon many of them during my freelance career. Usually, things are so bad that even a basic test that checks for the 200 HTTP code may not work. But you can try anyway! Baby steps and sustained efforts can change things for the better.

And if Laravel's feature tests are too time-consuming to write because of what I just said, you might want to take a look at [Laravel Dusk](https://laravel.com/docs/dusk). It uses a headless Google Chrome to help you test if your application works, from the perspective of your users and without any regard for the backend code.
