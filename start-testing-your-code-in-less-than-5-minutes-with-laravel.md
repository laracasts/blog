# Start testing your code in less than 5 minutes with Laravel

Coding is fun, but debugging? Not so much. That's why testing is crucial for the success of your project. I will show you how easy it is to start testing your Laravel applications. Let's break the ice once and for all!

## Why You Want to Write Tests for Your Projects

Writing tests for your projects is essential for various reasons:

- **Fix basic bugs**: Before they ruin the experience for your users, you can catch and fix basic bugs during the development phase.
- **More confidence for deployments**: With a well-tested application, you can deploy with confidence, knowing that your code is solid.
- **Less time spent in your web browser or HTTP client**: Doing the same thing over and over manually can be time-consuming. Automated testing saves you a lot of time and effort.
- **Happier customers**: A well-tested application leads to a smoother user experience, which results in happier customers and less churn.
- **Happier clients**: Clients appreciate a bug-free and well-functioning application, just as in your sales pitch!
- **Happier employer**: A well-tested, bug-free application will make your employer happy as well. It shows professionalism and attention to detail. It can only be good for your career.

## Create a New Project

Creating a new project in Laravel with Pest is as simple as running a single command. Open your terminal and run:

```bash
laravel new hello-world --pest
```

This will create a new Laravel project with Pest pre-installed and ready to be used. `cd` into the newly created folder, make sure you can display Laravel's welcome page in your browser using whichever environment you like, and read to the next section!

## Make sure Pest can run

Once the project is created, you can run your tests to make sure everything is working as expected.

Check out the ExampleTest.php file in tests/Feature. It clearly states that it will test for the homepage to return a 200 HTTP status code.

Run your tests using the following command:

```bash
php artisan test
```

Everything should be green. If not, don't be discouraged. Spending some time in making sure your environment is correctly configured is a great investment.

Now, any moment something breaks during the process of rendering the home page, the test will fail. Try to change some code incorrectly, run `php artisan test` again, and you will see it for yourself.

## Create a new test

## The case of untested existing projects
