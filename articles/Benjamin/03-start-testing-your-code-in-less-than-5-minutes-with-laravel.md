---
title: Start Testing Your Laravel Code in Less Than 5 Minutes
description: Coding is fun. But debugging? ...Not so much. That's why testing is crucial for the success of your projects. In this article, I will show you how easy it is to begin testing your Laravel applications.
publish_date: "2023-09-19 10:00:00"
author: Benjamin Crozat
---

Coding is fun. But debugging? ...Not so much. That's why testing is crucial for the success of any non-trivial project. In this article, I will show you how easy it is to begin testing your Laravel applications. Let's break the ice once and for all!

## Why Write Tests?

Writing tests for your projects is essential for a variety of reasons, including:

{info}
- **Fix Bugs**: Before they ruin the experience for your users, you can catch and fix basic bugs during the development phase.
- **Deploy With Confidence**: With a well-tested application, you can deploy with confidence, knowing that your code is solid.
- **Skip the Browser**: Doing the same thing over and over manually can be time-consuming. Automated testing saves you a lot of time and effort.
- **Happier Customers**: A well-tested application leads to a smoother user experience, which results in happier customers and less churn.
- **Happier Clients**: Clients appreciate an application free of obvious bugs, just as in your sales pitch!
- **Happier Employer**: A well-tested, well-functioning application will make your employer happy as well. It shows professionalism and attention to detail. It can only be good for your career.
  {/info}
-
## Create a new project

Scaffolding a new project in Laravel with Pest is as simple as running a single command. Open your terminal and run:

```bash
laravel new hello-world --pest
```

At the time of this writing, Laravel ships with PHPUnit, by default. However, [Pest](http://pestphp.com), which is based on PHPUnit, has increasingly grown in popularity in the last few years. Let's use the `--pest` flag to generate Pest-specific tests.

{definition}
**Pest** is a testing framework with a focus on simplicity, meticulously designed to bring back the joy of testing in PHP.
{/definition}

`cd` into your new `hello-world` project, ensure that you successfully see Laravel's splash welcome page in the browser, and them move on to the next section!

## Make sure Pest can run

You can now execute the project's example tests to verify that everything is working as expected.

Have a look at the `tests/Feature/ExampleTest.php` file.

```php
<?php

it('returns a successful response', function () {
    $response = $this->get('/');

    $response->assertStatus(200);
});
```

Notice how the method name perfectly describes the intent of the test. Try to get in the habit of doing the same for your projects.

Run your test suite using the following command:

```bash
php artisan test
```

Everything should be green. Assuming that your environment is configured properly, we were able to install a fresh Laravel app, and run the initial test suite in just a few minutes. Not bad!

A basic test like this will ensure that, should something break during the rendering of the home page, the suite will properly fail. Let's try it out. Tweak `ExampleTest.php` to expect a different status code response. How about `302`?

```diff
<?php

it('returns a successful response', function () {
    $response = $this->get('/');

-    $response->assertStatus(200);
+    $response->assertStatus(302);
});
```

Run `php artisan test` again, and, yep, it fails as expected:

```bash
php artisan test
  
FAILED  Tests\Feature\ExampleTest > it returns a successful response
Expected response status code [302] but received 200.
Failed asserting that 302 is identical to 200.

  at tests/Feature/ExampleTest.php:6
      2▕
      3▕ it('returns a successful response', function () {
      4▕     $response = $this->get('/');
      5▕
  ➜   6▕     $response->assertStatus(302);
      7▕ });
      8▕


  Tests:    1 failed, 1 passed (2 assertions)
  Duration: 0.18s
```

## Real-Life Example

Of course, practice makes perfect. In this next section, let's see how we might write some tests that are a bit more representative of a real-world application. We'll create a tested contact form that delivers an email to a recipient.

### The Controller

To get started with our contact form, we'll generate a `SendContactEmailController` controller.

```bash
php artisan make:controller SendContactEmailController --invokable
```

Notice that we're using the `--invokable` flag to create a single-action controller, or a controller that can only ever have a single action.

### The Routes

Next, we should declare two RESTful routes to handle the form: one that responds to `GET` requests on the `/contact` path. This is where we'll display the form. Next, we listen for a `POST` request to `/contacts`. Here, we can receive the user's input and deliver the message.

```php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\SendContactEmailController;

Route::view('/contact', 'contact');
Route::post('/contact', SendContactEmailController::class);
```

### Create the Mailer

Creating a Laravel mailable is as simple as generating any other controller or model. We can use the `make:mail` command.

```bash
php artisan make:mail ContactMail
```

Let's modify it a bit with three properties: `$from`, `$name`, and `$message`. In the example below, we're using constructor property promotion, which makes for slightly less code.

```php
namespace App\Mail;

use Illuminate\Mail\Mailables\Address;
use Illuminate\Mail\Mailables\Envelope;

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

The nicely named `envelope()` method, as you might have guessed, is where we can customize the details for the email, such as the subject and from address.

### Construct the Form

We're almost there. We should now build the contact form, itself. Don't worry; there's nothing unique or fancy here. We'll add fields for a name, email, and message.

```bash
php artisan make:view contact
```

That command should generate the file,  `resources/views/contact.blade.php`. Within it, paste the following HTML:

```php
@if (session('status'))
    <p>{{ session('status') }}</p>
@endif

<form method="POST" action="/contact">
    @csrf

    <div>
        <label for="name">Name</label>
        <input type="name" id="name" name="name" value="{{ old('name') }}" />
        @error('name') <p>{{ $message }}</p> @enderror
    </div>

    <div>
        <label for="email">Email</label>
        <input type="email" id="email" name="email" value="{{ old('name') }}" />
        @error('email') <p>{{ $message }}</p> @enderror
    </div>

    <div>
        <label for="message">Message</label>
        <textarea id="message" name="message">{{ old('message') }}</textarea>
        @error('message') <p>{{ $message }}</p> @enderror
    </div>

    <button>Send</button>
</form>
```

You've likely written similar HTML countless times. Do note, however, that we're referencing Laravel's `@error()` directive to display validation error messages below each input. We also include the `@csrf` directive to protect against Cross-Site Request Forgery.

{definition}
Cross-Site Request Forgery (**CSRF**) is an attack that forces an end user to execute unwanted actions on a web application in which they’re currently authenticated
{/definition}

### Send the Email

In the `SendContactEmailController` controller, we can write the necessary logic to send the email. We should handle three things within this controller action:

1. **Validate** the user's input, because we should never trust it.
2. **Send** the email using the `Mail` Facade, and the `ContactMail` mailable.
3. **Redirect** the user to the previous page with a status message.

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

### Write the Tests

Creating new tests in Laravel is as easy as using the following command. Don't forget the `--pest` option.

```bash
php artisan make:test SendContactEmailTest --pest
```

Now, we can finally prepare the tests for our contact form.

#### The Contact Page Works

Let's start with a simple test which ensures that a `200` status code is returned when visiting the `/contact` page.

```php
use App\Mail\ContactMail;
use Illuminate\Support\Facades\Mail;
use function Pest\Laravel\{get,post};

test('the contact page works', function () {
    get('/contact')->assertOk();
});
```

Easy! We've written our first test to verify that the contact page is accessible. This test makes a _GET_ request to the _"/contact"_ route and asserts that the response status is OK (`200`).

#### The Contact Email Can Be Sent

Next, we'll confirm that the contact email can be sent.

```php
test('the contact email can be sent', function () {
    Mail::fake();

    post('/contact', [
        'name' => fake()->name(),
        'email' => fake()->safeEmail(),
        'message' => fake()->paragraph(),
    ])
        ->assertRedirect('/contact');

    Mail::assertSent(ContactMail::class);
});
```

- We first fake the `Mail` facade to prevent actual emails from being sent, and to assert that an email was sent.
- We then make a `POST` request to the `"/contact"` route with a randomly generated fake name, email, and message.
- We assert that the request is redirected back.
- Finally, we assert that the mailable `ContactMail` was sent.

We're at green! Let's keep going.

#### The Contact Email Requires a Name

We should now test that the contact email requires a name, a valid email, and a message.

```php
test('the contact email requires a name', function () {
    post('/contact', [
        'email' => fake()->safeEmail(),
        'message' => fake()->paragraph(),
    ])
        ->assertInvalid(['name' => 'required']);
});

test('the contact email requires a valid name', function () {
    // rinse and repeat...
});
```

- We make a POST request to the `"/contact"` route with only a fake email and message (no name).
- We then assert that the response is invalid and that the `name` field is required.

## Be pragmatic when writing tests

My strategy for writing tests has been the same for years:

{tip}
1. Write for all the happy paths. A happy path represents the best-case, expected path through your code.
2. Write for the obvious unhappy paths. Expect the unexpected. This is why we prepare tests for missing inputs.
3. Write tests for the bugs your users encounter. These are referred to as **regression tests**. Write the bug as a test, fix it, return to green, and ensure that it never happens again.
   {/tip}

## We've Got You Covered

If you'd like to dig deeper, of course we have you covered at Laracasts. You might start with a beginning Pest course, such as [Pest From Scratch](https://laracasts.com/series/pest-from-scratch). Once you feel comfortable, and
are ready to work on real-life applications, move on to [Pest-Driven Laravel](https://laracasts.com/series/pest-driven-laravel).

1.
2.

Until next time!
