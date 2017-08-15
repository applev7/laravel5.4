In most of the application, pagination is very common task and Laravel PHP Framework has excellent library for pagination and with version of Laravel 5, you can easily customize pagination template either creating a own pagination file manually or using exist option is to run artisan command to publish the pagination template.

As all know Laravel has default bootstrap pagination view and i here let you know :

Basic example of pagination that use default pagination view
```
// routes file
Route::get('products', function () {
    return view('products.index')
        ->with('products', Product::paginate(20));
});
```
```
// resource/views/products/index.blade.php
@foreach ($products as $product)
    <!-- show the product details -->
@endforeach
{!! $products->links() !!}
```

Above example will give you simple default Laravel pagination view.

Ok, Now i will tell you how to pass custom view for Laravel pagination.

Custom Pagination Template in Laravel 5.3
If you go through publish file using php artisan command then run following command :

    php artisan vendor:publish --tag=laravel-pagination

You will get this message:

```
Copied Directory [\vendor\laravel\framework\src\Illuminate\Pagination\resources\views] To [\resources\views\vendor\pagination]
Publishing complete.
```

By running above command you will see the pagination directory in following path resources/views/vendor/ and in pagination directory you will get 4 files by default:

- bootstrap-4.blade.php
- default.blade.php
- simple-bootstrap-4.blade.php
- simple-default.blade.php

Laravel use default.blade.php file for default pagination view.

You can manually create your own custom pagination template and link it with pagination links method to show pagination view as per your needs.

We will use our ticket list as our practice. So you may want to add more tickets to the list.


Now in TicketsController, modify index method to following :
```
public function index(Request $request)
{
    $tickets = Ticket::paginate(5);
    return view('tickets.index',compact('tickets'))->with('i', ($request->input('page', 1) - 1) * 5);

}
```

Be sure to add this at the top

    use Illuminate\Http\Request;

We have created tickets/index.blade.php before.

Find :

```
<table class="table">
                    <thead>
                    <tr>
                        <th>ID</th>
                        <th>Title</th>
                        <th>Status</th>

                    </tr>
                    </thead>
                    <tbody>
                    @foreach($tickets as $ticket)
                        <tr>
                            <td>{!! $ticket->id !!} </td>
                            <td>{!! $ticket->title !!}</td>
                            <td>{!! $ticket->status ? 'Pending' : 'Answered' !!}</td>
                        </tr>
                    @endforeach
                    </tbody>
                </table>
```

Add this below it :

    {!! $tickets->links('pagination') !!}

In above links method, we pass the view name pagination to identify which custom template we are going to use for pagination.

# Create a custom template pagination.blade.php

Now in this last step we will create a template which is used by a single paginator.

resources/views/pagination.blade.php

```
@if ($paginator->hasPages())
    <ul class="pager">
        {{-- Previous Page Link --}}
        @if ($paginator->onFirstPage())
            <li class="disabled"><span>? Previous</span></li>
        @else
            <li><a href="{{ $paginator->previousPageUrl() }}" rel="prev">? Previous</a></li>
        @endif
        {{-- Pagination Elements --}}
        @foreach ($elements as $element)
            {{-- "Three Dots" Separator --}}
            @if (is_string($element))
                <li class="disabled"><span>{{ $element }}</span></li>
            @endif
            {{-- Array Of Links --}}
            @if (is_array($element))
                @foreach ($element as $page => $url)
                    @if ($page == $paginator->currentPage())
                        <li class="active my-active"><span>{{ $page }}</span></li>
                    @else
                        <li><a href="{{ $url }}">{{ $page }}</a></li>
                    @endif
                @endforeach
            @endif
        @endforeach
        {{-- Next Page Link --}}
        @if ($paginator->hasMorePages())
            <li><a href="{{ $paginator->nextPageUrl() }}" rel="next">Next ?</a></li>
        @else
            <li class="disabled"><span>Next ?</span></li>
        @endif
    </ul>
@endif
```

This pagination files can be used in any lists. It called repetitive code.

Thats it. You can see the result and change to any limit per page you want.


