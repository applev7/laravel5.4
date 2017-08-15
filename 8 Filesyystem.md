#FileSystem Storage

Since Laravel 5, the filesystem operations are abstracted thanks to the Flysystem PHP package by Frank de Jonge. This integration provides simple drivers for working with local filesystems, Amazon S3, and other Cloud Storage. The API remains the same for each system therefore is really simple to swap between each storage option.

#Flysystem Adapters

As I said, Laravel uses the Flysystem library to abstract the filesystem. This library makes use of the adapter pattern:

Flysystem provides all different kinds of adapters, that basically cover everything you need: local filesystem, AWS, Dropbox, Rackspace, FTP etc.

For testing purposes Flysystem provides three Adapters:

- Memory: the filesystem is keeped completely in memory
- Null/Test: acts like /dev/null
- VFS: allows to mount a virtual filesystem

For this tutrial we will examine the last one. The idea is to swap the local adapter with the VFS when the application is being tested. We will use PHPUnit as our main testing tool.

You can install the adapter through composer:

    composer require league/flysystem-vfs

The package should be installed automatically in vendor/league directory.

Now let's test run the function. Add following in our routes/web.php
```
Route::get('tickets/upload', function(){
    $contents = 'this is test file using flysystem';
    Storage::put('file.txt', $contents);
    echo $contents;
});
```
When you browse to http://yourlocalhost:8800/tickets/upload, the file will be generated and put in storage directory.

#Config File

 Go to config/filesystems.php, you will see

```
<?php

return [

    /*
    |--------------------------------------------------------------------------
    | Default Filesystem Disk
    |--------------------------------------------------------------------------
    |
    | Here you may specify the default filesystem disk that should be used
    | by the framework. The "local" disk, as well as a variety of cloud
    | based disks are available to your application. Just store away!
    |
    */

    'default' => env('FILESYSTEM_DRIVER', 'local'),

    /*
    |--------------------------------------------------------------------------
    | Default Cloud Filesystem Disk
    |--------------------------------------------------------------------------
    |
    | Many applications store files both locally and in the cloud. For this
    | reason, you may specify a default "cloud" driver here. This driver
    | will be bound as the Cloud disk implementation in the container.
    |
    */

    'cloud' => env('FILESYSTEM_CLOUD', 's3'),

    /*
    |--------------------------------------------------------------------------
    | Filesystem Disks
    |--------------------------------------------------------------------------
    |
    | Here you may configure as many filesystem "disks" as you wish, and you
    | may even configure multiple disks of the same driver. Defaults have
    | been setup for each driver as an example of the required options.
    |
    | Supported Drivers: "local", "ftp", "s3", "rackspace"
    |
    */

    'disks' => [

        'local' => [
            'driver' => 'local',
            'root' => storage_path('app'),
        ],

        'public' => [
            'driver' => 'local',
            'root' => storage_path('app/public'),
            'url' => env('APP_URL').'/storage',
            'visibility' => 'public',
        ],

        's3' => [
            'driver' => 's3',
            'key' => env('AWS_KEY'),
            'secret' => env('AWS_SECRET'),
            'region' => env('AWS_REGION'),
            'bucket' => env('AWS_BUCKET'),
        ],

    ],

];
```

By default Flysystem use local to store the file.

So we can change this setting directly in here oor we can use .env

I want to change from .env because in development, other teammates may want to use different configurations.

in .env file, add this configuration to the bottom :

    FILESYSTEM_DRIVER=public

If you test it again it will change to use public config.

And also edit APP_URL to reflect your localhost, for example :

    APP_URL=http://localhost/yourapp/public

#Implement In Ticket Form

We will use our support ticket system.

When user submit a ticket, they might want to upload a document or image.

Now, go to views/tickets/create.blade.php

Find :
```
<div class="form-group">
    <div class="col-lg-10 col-lg-offset-2">
        <button class="btn btn-default">Cancel</button>
        <button type="submit" class="btn btn-primary">Submit</button>
    </div>
</div>
```

Add above it :

```
<div class="form-group">
    <label for="content" class="col-lg-2 control-label">Attach a File</label>
    <div class="col-lg-10">
        <input type="file" name="file" class="form-control">
    </div>
</div>
```

Before we go save, you might want to change this

    <form class="form-horizontal" method="post">

Change to this

    <form class="form-horizontal" method="post" enctype="multipart/form-data">

Try to change store method in TicketsController wiht this :
```
public function store(TicketFormRequest $request)
{
    return print_r($request->all());
    /*
    $slug = uniqid();
    $ticket = new Ticket(array(
        'title' => $request->get('title'),
        'content' => $request->get('content'),
        'slug' => $slug,
        'uploaded_file' =>
    ));

    $ticket->save();
    return redirect('/contact')->with('status', 'Your ticket has been created! Its unique id is: '.$slug);
    */
}
```

And now submit a form with image :

You will see something similar like this :
```
Array ( [_token] => CzZ5WQ1lc4RWE5JBF8UQbgt76KnajuGbPciTkAKZ [title]
=> sff [content] => dsfsd [file] => Illuminate\Http\UploadedFile Object
( [test:Symfony\Component\HttpFoundation\File\UploadedFile:private] =>
[originalName:Symfony\Component\HttpFoundation\File\UploadedFile:private] =>
11271149_10153077608193743_658887523_n.jpg [mimeType:Symfony\Component\HttpFoundation\File\UploadedFile:private]
=> image/jpeg [size:Symfony\Component\HttpFoundation\File\UploadedFile:private]
=> 165379 [error:Symfony\Component\HttpFoundation\File\UploadedFile:private] => 0 [hashName:protected] =>
[pathName:SplFileInfo:private] => C:\xampp\tmp\php3593.tmp [fileName:SplFileInfo:private] => php3593.tmp ) )
```

Now we change back store method to this :
```
public function store(TicketFormRequest $request)
{
    $slug = uniqid();
    $uploadedFile = '';
    if($request->has('file')){
        $file = $request->get('file');
        $uploaded = Storage::put($slug.'/'.$file->getClientOriginalName(), file_get_contents($file->getRealPath()));
        if($uploaded){
            $uploadedFile = $file->getClientOriginalName();
        }
    }
    $ticket = new Ticket(array(
        'title' => $request->get('title'),
        'content' => $request->get('content'),
        'slug' => $slug,
        'file'  => $uploadedFile
    ));

    $ticket->save();
    return redirect('/contact')->with('status', 'Your ticket has been created! Its unique id is: '.$slug);
}

```

We have defined a new column in store method, so we need to create a migration to add new column in database.

Run this command :

    php artisan make:migration add_column_file_tickets --table=tickets

Go to our newly created migration file, modify to this:
```
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class AddColumnFileTickets extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('tickets', function (Blueprint $table) {
            $table->text('file')->nullable();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('tickets', function (Blueprint $table) {
            $table->dropColumn('file');
        });
    }
}
```

Now run migrate command :

    php artisan migrate

After this, we need to add fillable field in Ticket Model.
Add this in $fillable property :

    protected $fillable = ['title', 'content', 'slug', 'status', 'user_id', 'file'];

Now you can try to submit the form.

#View Uploaded File

Open TicketsController. The changes in the show method should look like this:

```
public function show($slug)
{
    $ticket = Ticket::whereSlug($slug)->firstOrFail();
    $comments = $ticket->comments()->get();
    $file = strlen($ticket->file) > 0 ? Storage::url($slug.'/'.$ticket->file) : '';
    return view('tickets.show', compact('ticket', 'comments', 'file'));
}
```

We need to change our tickets/show.blade.php also.

Find :

    <p> {!! $ticket->content !!} </p>

Add below it :

```
<p>
    @if(strlen($file) > 0)
        <a href="{{ $file }}" target="_blank">View File</a>
    @endif
</p>
```

Wait, to access file from browser we need too link storage directory to public.

Run following command :

    php artisan storage:link

Now it is complete.