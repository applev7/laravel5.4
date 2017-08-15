#Submit A Comment Using Ajax Submit

In show Ticket.

At the bottom, find :

	@endsection 

Add this jquery function before it :

```
<script type="text/javascript">
    $('#reply_form_id').on('submit',function(e){
        e.preventDefault(e);
        $.ajax({
            method:"POST",
            url: "{{ url('/comment') }}",
            data:$("#reply_form_id").serialize(),
            dataType: 'html',
            success: function(data){  
               $( "#comment_list" ).append( data );
               $('#content').html('');
               
            },
            error: function(data){
                console.log("request failed");
            }
        });
    });
</script>
```
This method will trigered when submit button clicked.

It will run $.ajax with method post and data serialized from our form.

Upon success, controller will return html and appended to id comment_list.

Find :

```
@foreach($comments as $comment)
    <div class="well well bs-component">
        <div class="content">
            {!! $comment->content !!}
        </div>
    </div>
@endforeach
```

Change to this :

```
<div id="comment_list">
	@foreach($comments as $comment)
	    <div class="well well bs-component">
	        <div class="content">
	            {!! $comment->content !!}
	        </div>
	    </div>
	@endforeach
</div>
```

In comment controller, change to the following code

```
public function newComment(CommentFormRequest $request)
{
    $comment = new Comment(array(
        'post_id' => $request->get('post_id'),
        'content' => $request->get('content')
    ));
    $comment->save();
    $status = 'Your comment has been created!';
    return view('tickets.show_comment', compact('comment', 'status'));
}
```

We dont have show_comment template yet. 
Create show_comment.blade.php in tickets directory.

And add following code 

@if(isset($status))
	<div class="alert alert-success" >
		<button type="button" class="close" data-dismiss="alert">x</button>
        {{ $status }}
    </div>
@endif
<div class="well well bs-component">
    <div class="content">
        {!! $comment->content !!}
    </div>
</div>

Also make sure your jquery is defined before @yield('content') like this :

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
	    <link rel="stylesheet" type="text/css" href="{{ asset('material/css/bootstrap-material-design.css') }}">
	    <link rel="stylesheet" type="text/css" href="{{ asset('material/css/ripples.css')}}">
	</head>
	<body>

	<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
	@include('shared.navbar')
	@yield('content')
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

	<script src="{{asset('material/js/ripples.js') }}"></script>
	<script src="{{ asset('material/js/material.min.js')}}"></script>
	<script>
	    $(document).ready(function() {
	        // This command is used to initialize some elements and make them work properly
	        $.material.init();
	    });
	</script>
	</body>
</html>
```

Now it is complete. You can test it now.