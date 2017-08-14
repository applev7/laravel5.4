#How to install Twitter Bootstrap using Mix

In webpack.mix.js, change to this file :

```
let mix = require('laravel-mix');

/*
 |--------------------------------------------------------------------------
 | Mix Asset Management
 |--------------------------------------------------------------------------
 |
 | Mix provides a clean, fluent API for defining some Webpack build steps
 | for your Laravel application. By default, we are compiling the Sass
 | file for the application as well as bundling up all the JS files.
 |
 */

mix.js('resources/assets/js/app.js', 'public/js')
    .sass('resources/assets/sass/app.scss', 'public/css')
    .copy('node_modules/bootstrap-material-design/dist/js', 'public/material/js')
    .copy('node_modules/bootstrap-material-design/dist/css', 'public/material/css')
;
```
Before that you might want to delete directory js, css and scss in public directory.

And then run following command :

	npm run dev

Above command will repopulate files in public directory.

In later production, you need to run this to generate minify css and js

	npm run production

#modify master layout

Now change our master layout to following

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
    <link rel="stylesheet" type="text/css" href="material/css/bootstrap-material-design.css">
    <link rel="stylesheet" type="text/css" href="material/css/ripples.css">
</head>
<body>

@include('shared.navbar')
@yield('content')
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

<script src="material/js/ripples.js"></script>
<script src="material/js/material.min.js"></script>
<script>
    $(document).ready(function() {
        // This command is used to initialize some elements and make them work properly
        $.material.init();
    });
</script>
</body>
</html>
```

