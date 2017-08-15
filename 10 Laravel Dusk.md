Laravel Dusk was one of the new features introduced in Laravel 5.4. Dusk is a tool for application testing.

One of the challenges of testing with PHPUnit was the inablity to test JavaScript based application functionality.
With Dusk the test is run in the browser so client-side features like JavaScript are testable.



Dusk is based on the open source tools ChromeDriver and Facebook Php-webdriver which makes it simple to use without the need to experience the intense procedure of setting up Selenium. In this article we will would cover installing Dusk in a Laravel application and running a few basic tests including authentication testing.

# Why Dusk?

Prior versions of Laravel used Symfony BrowserKit component to simulate a web browser. This had it's limitations as this is not a real browser. With Dusk the automation comes natively through it's use of ChromeDriver and Facebook Php-webdriver.

One of Dusk features also inlcudes the ability to wait for a condition to be true at the frontend, before executing tests. For example it could wait for a JavaScript component or CSS selector to load before taking any action.

# Installation

To begin create a new Laravel project:

`laravel new dusk`

Then go into the directory and pull in the laravel/dusk package

`cd dusk`

`composer require laravel/dusk`

When installing a Laravel package the service provider class is usually registered in the config\app.php file. However Dusk exposes some security holes that you don't want to have in the production environment, for example the ability to log in as other users.

To get around this, you should register the service provider class in a conditional that makes sure we are in a local environment in the


app\Providers\AppServiceProvider.php file.
```
use Laravel\Dusk\DuskServiceProvider;// Importing DuskServiceProvider class

...

public function register()
{
    if ($this->app->environment('local', 'testing')) {
        $this->app->register(DuskServiceProvider::class);
    }
}
```

Next run the artisan command to install Dusk:
```
php artisan dusk:install
```

This command creates a Browser directroy inside the tests folder of your project and adds a basic example test. Also amongst other sub-folders you would notice a folder named screenshots, when a test fails Dusk takes a picture of the screen and saves it in this folder. This can be useful when debugging.

Next you need to update the APP_URL key of your .env file, with the URL you use to access your application. For example if you are using PHP's built-in development server to serve your application the value of App_URL could be http://localhost:8000. Make sure your developement server is turned on before running a test otherwise you would have a failed test. For PHP's built-in server run php artisan serve.

You also have the option of letting Dusk use it's own environment file, to do this create a .env.dusk.{environment} file. For example, in a local environment this file would be .env.dusk.local This is useful when you do not want to test on your current database but you want to run a test off a test database.

# Running Tests

To run your tests use the command:

    php artisan dusk

This will run the ExampleTest in tests\Browser\ExampleTest.php. You should see something like this: This shows the test was passed and the result: OK (1 test, 1 assertion) was displayed. If you open the tests\Browser\ExampleTest.php file you would see the following method:
```
...
public function testBasicExample()
    {
    //Visit the homepage and look for the text 'Laravel'
        $this->browse(function (Browser $browser) {
            $browser->visit('/')
                    ->assertSee('Laravel');
        });
    }
```

Here the browse method accepts a callback which is an instance of the Browser class. Inside this callback you can call different methods and assertion unique to Dusk. The visit() method as its name implies is a test that visits the URL corresponding to the arguement provided. The assertSee() method checks if the text corresponding to the arguement given is on the page.

So in the above example the hompage is visited and a check is made for the text 'Laravel'. There are several assertions available, see the documentation for more information.

What happens if we replace the 'Laravel' text in the arguement of the assertSee() method with a text that is not on the visited page? Obviously you would get a failed test.



The test will fail because Dusk could not find the text in the homepage to which I have changed the arguement in the assertSee() method. Also take a look at your tests\Browser\screenshots directory you should see a screenshot of the just fail test.



# Forms and Authentication

With Dusk you can also interact with forms and test authentication. To demostrate this let's setup Laravel's basic login and registration forms.

First create a database and update your .env file with the database information. Then run:

    php artisan make:auth

This command creates scaffolding and migration files for authentication. Note that If you are running MariaDB or MYSQL version lower than 5.7.7 you need to edit the app\Providers\AppServiceProvider.php file setting the default string length in the boot method:
```
use Illuminate\Support\Facades\Schema;

public function boot()
{
    Schema::defaultStringLength(191);
}
```

Next run:

    php artisan migrate

This command generates our database tables from the migration files. If you go to the homepage of your project you should see the following with the Login and Register links at the far right corner:



Next create a new test named RegisterTest test by running

    php artisan dusk:make RegisterTest
Now in your tests\Browserfolder you will see the file RegisterTest.phpwhich look like this:
```
<?php

namespace Tests\Browser;

use Tests\DuskTestCase;
use Illuminate\Foundation\Testing\DatabaseMigrations;

class RegisterTest extends DuskTestCase
{
    /**
     * A Dusk test example.
     *
     * @return void
     */
    public function testExample()
    {
        $this->browse(function ($browser) {
            $browser->visit('/')
                    ->assertSee('Laravel');
        });
    }
}
```

Edit the testExample() method to look like this
```
public function testExample()
    {
        $this->browse(function ($browser) {
            $browser->visit('/') //Go to the homepage
                    ->clickLink('Register') //Click the Register link
                    ->assertSee('Register') //Make sure the phrase in the arguement is on the page
            //Fill the form with these values
                    ->value('#name', 'Joe')
                    ->value('#email', 'joe@example.com')
                    ->value('#password', '123456')
                    ->value('#password-confirm', '123456')
                    ->click('button[type="submit"]') //Click the submit button on the page
                    ->assertPathIs('/home') //Make sure you are in the home page
            //Make sure you see the phrase in the arguement
                    ->assertSee("You are logged in!");
        });
    }
```

You may also choose to delete the ExampleTest.php file as it is no longer required. Now run

    php artisan dusk


# Conclusion

Laravel Dusk makes it a lot easier and efficient to write tests. To recap we have covered the basic setup of Dusk, introduced how to write tests and wrote an authentication test. These by no means exhausts all it's features, there is still a lot more to write about. In a later article we would delve into more of its features.