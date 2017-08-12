**Building Our Homepage**

What we will do?

We will learn about :-
- Navigation,
- Controllers,
- Blade templates,
- Artisan commands,
- Mix
- and many basic features that will come handy when building Laravel applications.


Changing Laravel home page

To change our homepage, we need to know the file located. In previous we change the route to HomeController.

And index method return welcome template.

Go to the views folder, find a file called welcome.blade.php. It's the welcome view. Open it.

It's time to modify the home page!

What should we do?...

**Adding more pages to our first website**

Imagine that we're going to build a website to introduce the Learning Laravel book. Our website will have three pages:
- Home page.
- About page.
- Contact page.

Because we have many pages, and we might add more pages into our website, we should organize all our pages in PagesController.

As you've noticed, we don't have the PagesController yet, thus we're going to create it.

**Create our first controller**

You have known how to use Git Bash or Command Prompt, let's create a controller by running this command:

		php artisan make:controller PagesController

Next, let's add a new a home action:
```
public function home()
{
    return view('welcome');
}
```

**Change The Home Route**

Cool! When you have PagesController, the next thing to do is modifying our routes!

Open the web.php file. Change the default route to:

		Route::get('/', 'PagesController@home');

**Create other pages**

Edit the web.php file. Add the following lines:
```
Route::get('/about', 'PagesController@about');
Route::get('/contact', 'PagesController@contact');
```

Open PagesController, add:
```
    public function about() {
        return view('about');
    }
    public function contact() {
        return view('contact');
    }
```

Next you will want to create about view and contact view. Create two new files in the resources/views directory named about.blade.php and contact.blade.php.

Finally, add the following contents (copy from the welcome view) to each file:

```
<html>
    <head>
        <title>About Page</title>
        <link href='//fonts.googleapis.com/css?family=Lato:100' rel='stylesheet' type='text/css'>
        <style>
            body {
                margin: 0;
                padding: 0;
                width: 100%;
                height: 100%;
                color: #B0BEC5;
                display: table;
                font-weight: 100;
                font-family: 'Lato';
            }
            .container {
                text-align: center;
                display: table-cell;
                vertical-align: middle;
            }
            .content {
                text-align: center;
                display: inline-block;
            }
            .title {
                font-size: 96px;
                margin-bottom: 40px;
            }
            .quote {
                font-size: 24px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="content">
                <div class="title">About Page</div>
                <div class="quote">Our about page!</div>
            </div>
        </div>
    </body>
</html>
```
Edit title, and content.
Save these changes, go to yourlocalhost/about and yourlocalhost/contact:

**Integrating Twitter Bootstrap**

Using Twitter Bootstrap, we can quickly develop responsive, mobile-ready web applications. Millions of beautiful and popular sites across the world are built with Bootstrap.

In this section, we will learn how to integrate Twitter Bootstrap into our Laravel application.

You can get Bootstrap and read its official documentation here:

http://getbootstrap.com

Three Popular Methods
- Using Bootstrap CDN
- Using Precompiled Bootstrap Files
- Using Bootstrap Source Code (Sass or Less)

**Using Bootstrap CDN**
```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css">

<script src="//code.jquery.com/jquery-3.1.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
Using Precompiled Bootstrap Files
<link rel="stylesheet" href="{!! asset('css/bootstrap.min.css') !!}" >
<link rel="stylesheet" href="{!! asset('css/bootstrap-theme.min.css') !!}">

<script src="{!! asset('js/jquery-3.1.1.min.js') !!}"></script>
<script src="{!! asset('js/bootstrap.min.js') !!}"></script>
```

**Using Bootstrap Source Code (Sass or Less)**

The good news is, Laravel 5.4 has officially supported Sass. Sass files are placed at resources/assets/sass.

We'll use Laravel Mix to automatically compile Sass files to app.css file.

To load Twitter Bootstrap framework for styling our home page, open home.blade.php and place these links inside the head tag:
```
<link rel="stylesheet" href="/css/app.css">
<script src="/js/app.js"></script>
```

Also remove those font links styles tag in head. We not using them anymore.

Here is our new home.blade.php:
```
<html>
<head>
     <title>Home Page</title>
      <link rel="stylesheet" href="/css/app.css">
      <script src="/js/app.js"></script>
</head>
<body>

<div class="container">
    <div class="content">
        <div class="title">Home Page</div>
        <div class="quote">Our Home page!</div>
    </div>
</div>
</body>
</html>
```

**Introducing Laravel Mix**

Laravel Mix is a new feature of Laravel 5.4. In older versions of Laravel, we have Elixir, which is pretty similar.

We can use Laravel Mix to compile Sass files, Coffee Scripts or automate other manual tasks.

To use Laravel Mix, we must have NPM(Node Package Manager) and Node.js installed.

To install Node.js (v7.5.0), visit:

	https://nodejs.org/en/blog/release/v7.5.0/

**Check Node Version**

Follow instructions on the site, you should have installed Node.js easily.

You may check the version of Node.js by running:
		
		node -v
		
You may check the version of NPM by running:

		npm -v

		
The last step is to install Laravel Mix. Laravel 5 has included a file called package.json. You use this file to install Node modules. Open the file, you should see:
```
{
  "private": true,
  "scripts": {
    "dev": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "hot": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
    "production": "node node_modules/cross-env/bin/cross-env.js NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
  },
  "devDependencies": {
    "axios": "^0.15.2",
    "bootstrap-sass": "^3.3.7",
    "jquery": "^3.1.0",
    "laravel-mix": "^0.6.0",
    "lodash": "^4.16.2",
    "vue": "^2.0.1"
  }
}
```
Navigate to our app root , run this command to install Laravel Mix:
	
	npm install


**Running first Laravel Mix task**

You can write a new Mix task in webpack.mix.js. By default, you can find a Mix task that compiles app.sass file into app.css, and bundles all JS files there:
```
	mix.js('resources/assets/js/app.js', 'public/js') 	 	
		.sass('resources/assets/sass/app.scss', 'public/css');
```

Feel free to add more tasks by reading the official Laravel Mix documentation:

	https://laravel.com/docs/master/mix

To execute your Mix task, run this command:

		npm run dev

By running the task, Laravel will automatically compile the app.sass file and bundle all JS files.

**Adding Twitter Bootstrap components**

Now we can add Twitter Bootstrap components into our website.

There are many reusable Bootstrap components built to provide navbars, labels, dropdowns, panels, etc. You can see a full list of components here:

	http://getbootstrap.com/components

To use the components, you can copy the example codes, and paste them into your application. For instance, we can add Twitter Bootstrap navbar to our app by adding these codes to home.blade.php:
```
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">Brand</a>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
 <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>

<li class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
      <form class="navbar-form navbar-left" role="search">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>
          </ul>
        </li>
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```

Here is our new home.blade.php after adding the navbar:
```
<html>
<head>
    <title>Home Page</title>

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css">

    <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

</head>
<body>

<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Brand</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                <li><a href="#">Link</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu" role="menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">Link</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu" role="menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

<div class="container">
    <div class="content">
        <div class="title">Home Page</div>
        <div class="quote">Our Home page!</div>
    </div>
</div>
</body>
</html>
```

It's time to refresh our browser:

**Creating a master layout**

We already go through this before, so now we will change our master layout.

Open our file master.blade.php

Modify with following code.

```
<html>
<head>
    <title> @yield('title') </title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css">
    <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
     @include('shared.navbar')
     @yield('content')
</body>
</html>
```

You may notice that we've embedded a view called navbar here.

However, we don't have the navbar view yet, let's create a shared directory and put navbar.blade.php there.

Copy the Twitter Bootstrap navbar and put it into the navbar.blade.php:

```
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Brand</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                <li><a href="#">Link</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu" role="menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">Link</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu" role="menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>
```

**Extending the master layout**

Now we can change the home view to extend our master layout.
```
@extends('master')
@section('title', 'Home')

@section('content')
    <div class="container">
        <div class="content">
            <div class="title">Home Page</div>
            <div class="quote">Our Home page!</div>
        </div>
    </div>
@endsection
```

**Using other Bootstrap themes**

The best thing about Twitter Bootstrap is, it has many themes for you to "switch". If you don't like the default Bootstrap theme, you can easily find another one to use. Here are some popular themes for Bootstrap:

	- http://bootswatch.com
	- http://fezvrasta.github.io/bootstrap-material-design
	- http://designmodo.github.io/Flat-UI
	
In this tutorial, we're going to apply Bootstrap Material Design theme for our website.

First, head over to:

	https://github.com/FezVrasta/bootstrap-material-design
	
Download the zip file. Uncompressed it. Go to the dist directory.

If you're using the latest version of Material Design, you should only see two folders:
- css
- js

Copy css and js directory to our Laravel public folder.

Open master.blade.php, modify its content to look like this:
```
<html>
<head>
    <title> @yield('title') </title>
    <!-- Material Design fonts -->
    <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Roboto:300,400,500,700">
    <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/icon?family=Material+Icons">
    <!-- Bootstrap -->
    <link rel="stylesheet" type="text/css" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <!-- Bootstrap Material Design -->
    <link rel="stylesheet" type="text/css" href="/css/bootstrap-material-design.css">
    <link rel="stylesheet" type="text/css" href="/css/ripples.min.css">
</head>
<body>

@include('shared.navbar')
@yield('content')
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<script src="/js/ripples.min.js"></script>
<script src="/js/material.min.js"></script>
<script>
    $(document).ready(function() {
        // This command is used to initialize some elements and make them work properly
        $.material.init();
    });
</script>
</body>
</html>
```

Congratulations! You now have a beautiful Material Design website!

**Refine our website layouts**

Update our navbar.blade.php template
```
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="/">Learning Laravel</a>
        </div>
        <!-- Navbar Right -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav navbar-right">
                <li class="active"><a href="/">Home</a></li>
                <li><a href="/about">About</a></li>
                <li><a href="/contact">Contact</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Member
                    <span class="caret"></span></a>
                    <ul class="dropdown-menu" role="menu">
                        <li><a href="/users/register">Register</a></li>
                        <li><a href="/users/login">Login</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

Modify home.blade.php template
```
@extends('master')
@section('title', 'Home')
@section('content')
    <div class="container">
        <div class="row banner">
            <div class="col-md-12">
                <h1 class="text-center margin-top-100 editContent">
                    Learning Laravel 5
                </h1>
                <h3 class="text-center margin-top-100 editContent">Building Practical Applications</h3>
                <div class="text-center">
                    <img src="https://learninglaravel.net/img/LearningLaravel5_cover0.png" width="302" height="391" alt="">
                </div>
            </div>
        </div>
    </div>
@endsection
```
**Summary**

Good job! We now have a fully responsive template! We will use this template to build other applications to learn more about Laravel.

In this chapter, you've learned many things:
- You've known about Laravel structure, how Laravel works and where to put the files.
- You've learned about Laravel routes.
- You've learned Controllers. Now you can be able to create web pages using Controllers.
- You've known what Blade is. It's easy to create Blade templates for your next amazing applications.
- You've known how to integrate Twitter Bootstrap, CSS, JS and apply different Bootstrap themes.
- You've known Laravel Mix, how to install Node.js and how to create a basic Mix task.

