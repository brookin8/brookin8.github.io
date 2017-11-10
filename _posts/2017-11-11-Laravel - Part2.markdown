---
layout: post
title:  "Week 8: Laravel: Part 2"
date:   2017-11-10 01:09:27 -0400
categories: php, databases, postgres, sql, laravel
---
I'm going to focus on my new favorite tool (and the one thing that finally helped me grasp the structure and inner workings of Laravel): Resourceful Controllers. 

I'm shamelessly using a blog example just like Jeffrey Way does in his Laracasts because it is the perfect simple, all-inclusive demonstration. 

## **The Basics**

A resource controller comes loaded with everything you need to do the basic actions of a data-driven web app (or website): 

1. Fetch and **display all records** in a table
2. Fetch and **display only one record** in a table (detail view)
3. **Create a new record** to add to a table (through a form)
4. **Edit an existing record** in a table (through a form)
5. **Delete an existing record** in a table

I really like the blog example a lot of sites (and Jeffrey!) have been using. In relation to the actions above:

1. **Blog Home Page** - shows title and date of all posts
2. **Individual Blog Post Page** - when you click on a blog post, it shows the full body the title, date, AND all body content
3. **New Blog Post Page** - from the home page, you can click a button that says 'new post', and it takes you to a form where you can fill out a title and body content
4. **Individual Blog Post Page (Edit)** - If you click on the detailed individual blog post page, it has a button that says edit. When you click that button, it takes you to a page where you can go into the body, fix a typo, and then hit 'save'
5. **Blog Home Page (Delete)** - From the blog home page, there is a little red button with an x next to each post. When you click it, the post is deleted. 

If you can conceptualize these functions as the basic functions of a website - viewing all records, viewing one record (detailed), adding a new record, editing an existing record, and deleting an existing record - it helps you appreciate how easy a resourceful controller makes things.  

You create a resourceful control by typing into your command line: 'php artisan make:controller PostController --resource' (see my previous post for more on controllers and naming conventions).

This automatically creates a new controller with the following built in functions (it will look slightly different but I've slimmed it down here): 

    class ResourceController extends Controller
    {

        public function index() {}

        public function create() {}

        public function store(Request $request) {}

        public function show($id) {}

        public function edit($id) {}

        public function update(Request $request, $id) {}

        public function destroy($id) {}
    }

This will all be explained by...

## **The Chart**
<br>
#### Actions Handled By A Resource Controller (From Laravel's Documentation)

| Verb           | URI                    | Action |
|:-------------- |:-----------------------| ------:|
| GET            |  /posts                | index  |
| GET            |  /posts/create         |create  |
| POST           |  /posts                | store  |
| GET            |  /posts/{post}         |  show  |
| GET            |  /posts/{post}/edit    |  edit  | 
| PUT/PATCH      |  /posts/{post}         |update  |
| DELETE         |  /posts/{post}         |destroy |

<br>

The **action** column corresponds to functions provided to you in your resource controller. Using the blog actions I described in the section above you can think of them like this:

1. **INDEX** : Fetch and **display all records** in a table
2. **SHOW** : Fetch and **display only one record** in a table (detail view)
3. **CREATE** : **Create a new record** to add to a table (through a form)
    a. Then you **STORE** your new record
4. **EDIT** : **Edit an existing record** in a table (through a form)
    a. Then you officially **UPDATE** your edited record with your changes
5. **DESTROY** : **Delete an existing record** in a table

## **Index and Show**
These are by far the 2 easiest pieces of the resourceful controller puzzle. 

You'll be working with these two functions provided in your resourceful PostController:

      public function index() {}

      public function show($id) {}

You'll want to create a view for each as well. I would call them index.blade.php and show.blade.php. 

Let's start with indexing. I will assume you have a 'posts' table storing all of your posts, and an associated Post model. 

If your home page is 'myblog.com/', then (following the convention in the chart above) you would call the page where you can view ALL of your blog posts (or INDEX them) : **'myblog.com/posts'**

You need to hook this up to your index.blade.php in your routes file.

While you could use: 
    
    Route::get('/posts', PostController@index);

Resourceful controllers come with a nice shortcut. We can just put: 
    
    Route::resource('posts', 'PostController');

Now, it knows that it's acting upon the posts table, and will link up each appropriate url with the appropriate function **based on the chart above**. 

So now - /posts will trigger the route to run the index function in your PostController, /posts/create will trigger the route to run the create function in your PostController, and so on.

So now we have your 'myblog.com/posts' hooked up to the index function in your Post Controller, but we still need to connect it to a view. 

Edit your post controller as follows:

    public function index() {
        $posts = \App\Post::all();
        return view('index', compact('posts'));
    }    

This is saying: 

1. Grab all posts from your posts table, and store them in a variable called posts. 
2. Send this data to your index.blade.php view (by 'compact'-ing it), and then display it using the formatting found in that file. 

Your index.blade.php (view) might look something like this:

    @foreach ($posts as $post)
        <h1> {{ $post -> title }} </h1>
        <h4> {{ $post -> date }} </h4>
        <h4> {{ post -> author }} </h4>
    @endforeach

So now, when you navigate to **myblog.com/posts** you will see the title, date, and author of each blog post in your posts table displayed. 

Now, let's imagine you want to be able to click on the title of each post and see the body (full content) in a new view. You might edit your index.blade.php to include a link like so: 

    @foreach ($posts as $post)
        <h1><a href="/posts/{{ $post->id }}"> {{ $post -> title }} </h1>
        <h4> {{ $post -> date }} </h4>
        <h4> {{ post -> author }} </h4>
    @endforeach

Each post's title is now a link to a url ending in its unique id. For example, if you click on your 2nd post, you would get the link:

    myblog.com/posts/2

But what do we see when here? 

The good news is that because we hooked up our resource controller in our route file already, the app already knows that /posts/{post} (where {post} is equal to the primary key of the table you are using) should run either the show, update, or destroy function. 

Unless you use a form to send a **PUT** or **DELETE** request, it will assume you want it to run the show function. 

So, in your controller, you would update your show function like so:

    public function show($id) {
        $post = \App\Post::find($id);
        return view('show', compact('post'));
    }  

Note that laravel automatically assumes whatever comes after the posts/ is the $id you are passing in, and when you pass it into the find function it is also automatically looking to find a record where that record's **primary key matches $id**.

Continuing with the example of **myblog.com/posts/2**, your controller went into the posts table, found the record with the unique primary key of 2, and passed all of its associated fields and data into your show.blade.php file. 

Now that you have all of that data, you might want to do something to format it in your show.blade.php. It could look something like this:

    <h1> Title: {{ $post->title }} </h1>
    <p> {{ $post->body }} </p>

And now you can see the title and full body content of each post when you navigate to it's posts/{post} page (/posts/3, /posts/4, etc.)

## **Create (Store) and Edit (Update)**

Anytime you want to perform any of these functions, you have to use a form. 

Let's say you want to be able to create a new blog post. You'll start by creating a form where you can write and submit a post. This will require a new view, which you can call 'create.blade.php'.

Again, your resourceful controller is hooked up in your routes, so no need to add a specific route for any of these functions. 

Now, in your PostController, you'll want to update your create function like so (assuming each post has a title, author, body, and date):

    public function create()
    {
        return view('create');
    }

Pretty simple. Now in your create.blade.php, you'll want to set up your form:

    <form method="post" action="/posts">
        {{ csrf_field() }}
        
        <label for="title">Title</label>
        <input name="title" id="title">

        <label for="body">Body</label>
        <textarea name="body" id="body"></textarea>
    </form>

And now you're done with create. But for create to actually do something, you have to pair it with STORE. 

We'll start by adding a submit button within the form:

    <form method="post" action="/posts">
        {{ csrf_field() }}
        
        <label for="title">Title</label>
        <input name="title" id="title">

        <label for="body">Body</label>
        <textarea name="body" id="body"></textarea>

        <button type="submit">Submit</button>
    </form>
 
The csrf_field is just something laravel requires (and generously provides!) that adds an extra level of security to your requests. Not work digging too deep into at this point.

Now for update, the chart told us that we need to refer to the '/posts' URI with an action of 'POST':

| Verb           | URI                    | Action |
|:-------------- |:-----------------------| ------:|
| POST           |  /posts                | store  |

We've address this already with our form. You'll notice that used a method of "post" (our Verb), and an action of "/posts" (our URI). 

When we hit the submit button, it will generate a **POST request**.

But we still need to actually tell our app what to store. We do this in the Store Function:

    public function store(Request $request)
    {
        $post = new \App\Post;
    
        $post->title = request('title');
        $post->body = request('body');
        $post->author = \Auth::user()->id;

        $post->save();

        return redirect('/posts');
    }

So now, when we hit the submit button, we send a POST request from our form, the resource controller funnels this to our STORE function. Then:

1. We create a new post, and save it to a variable called $post.
2. We assign the different fields a post has ($post->title, etc) to the correct corresponding request fields. We get these fields using the 'name' that we assigned the various inputs in the form above(ex: input name="title" id="title"). 
3. The post's author will automatically pull whoever is signed in for this session using the built in \Auth::user()->id function (which is available once your run 'php artisan make:auth')
4. We save the post into the posts table.
5. We redirec to the /posts (index page), where we will now see our new post's title, author, and date displayed along with all other posts. 

And that's it! 

(Note that 'id' and 'created_at' (or date) generate automatically).

Edit and update work very similarly, but we'll walk through anyway:

On our original show.blade.php (linked to posts/{post}), we'll add an edit button with a link:

    <h1> Title: {{ $post->title }} </h1>
    <p> {{ $post->body }} </p>

    <button><a href="/posts/{$post->id}/edit"> Edit </button>    
    
If we refer back to our chart, we see that this will automatically trigger our resourceful edit function:

| Verb           | URI                    | Action |
|:-------------- |:-----------------------| ------:|
| GET            |  /posts/{post}/edit    |  edit  | 
| PUT/PATCH      |  /posts/{post}         |update  |

You'll want to create a form for editing similar to what you did for create. This will require an edit.blade.php view. 

Create this doc, and then add the following code:

     <form method="post" action="/posts/{$post->id}">
        {{ method_field('PUT') }}
        {{ csrf_field() }}
        
        <label for="title">Title</label>
        <input name="title" id="title" default="$defaultTitle">

        <label for="body">Body</label>
        <textarea name="body" id="body">{{ $defaultBody }}</textarea>

        <button type="submit">Submit</button>
    </form>    

Note that we added two new features here. One is the 'method_field('PUT')' line right below form. We use this because browsers sometimes only accept 'get' and 'post' as methods, so we're cheating by using post in our form, but sneaking through a 'PUT' method for laravel. Now that laravel has the method 'PUT' with the URI (form action) of '/posts/{post}', it will run the update function. 

But first, we need to pass in some data in to our edit view. We'll do this in our resourceful edit function:

    public function edit($id)
    {
        $post = \App\Post::find($id);
        $defaultTitle = $post->title;
        $defaultBody = $post->body;

        return view('edit');
    }

Now it will load the data for the post related to the specific posts/{post} page you hit the edit button on. Using our example of 2, it will navigate you from posts/2 to posts/2/edit, and pull all of the fields from your posts table related to your 2nd post. Then, it will assign that post's current title and current body to the variables $defaultTitle and $defaultBody.

When you go to the page, you will see the post 2's current title and body displayed in your input fields, but they will editable. 

You can make your changes, and then hit submit! That will move us to update:

| Verb           | URI                    | Action |
|:-------------- |:-----------------------| ------:|
| PUT/PATCH      |  /posts/{post}         |update  |

We already have 'PUT/PATCH' and the URI '/posts/{post}' taken care of in the form. Now we just need to fill in our update function:

    public function update(Request $request, $id)
    {
        $post = new \App\Post;
    
        $post = \App\Post::find($id);
        
        $post->body = request('body');
        $post->title = request('title');

        $post->save();

        return redirect('/posts');
    }

So now when you hit 'Submit', it will generate a request, assign the data from the request in the appropriate fields for the current post, save the post with its changes, and redirect you to the index '/posts' page with all blog posts. 

## **Destroy**

Destroy is fairly simple. It works similarly to edit, in that you submit a form, include both csrf_token and method_field, and redirect.

No new view is needed for destroy, but we will add a new button to our index (/posts/) page that shows up next to each post. Let's refer back to our chart for the proper formatting:

| Verb           | URI                    | Action |
|:-------------- |:-----------------------| ------:|
| DELETE         |  /posts/{post}         |destroy |

Looks like we'll need a form. We'll change index.blade.php to look like this:

    @foreach ($posts as $post)
        <h1> {{ $post -> title }} </h1>
        <h4> {{ $post -> date }} </h4>
        <h4> {{ post -> author }} </h4>

        <form method="post" action="/posts/{{ $post->id }}">
            {{ method_field('DELETE') }}
            {{ csrf_field() }}
            <button type="submit">Delete</button>  
        </form>
    @endforeach
    
When we hit the 'Delete' button, we will submit a request with the verb of 'DELETE' (from our method_field) and an action of '/posts/{post}'. We're all set!

The last step is to update the resourceful destroy function:

    public function destroy($id)
    {
        $post = \App\Post::find($id);
        $post->delete();
        return redirect('\posts');
    }

## **Big Takeaway:**

Now you can:

1. View all posts (with index)
2. View the details of one particular post (with show)
3. Update and save that post (with edit and update)
4. Create and save a new post (with create and store)
5. Delete a post (with destroy)

Resourceful controllers are pretty great. 












