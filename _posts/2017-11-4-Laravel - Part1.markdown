---
layout: post
title:  "Week 7: Laravel: Part 1"
date:   2017-11-4 01:09:27 -0400
categories: php, databases, postgres, sql, laravel
---

Laravel - in its entirety - can be scary for an inexperienced developer. 

There is a LOT of stuff going on in this framework. There are certainly similarities with Vue CLI, but it's seemingly much more complex. 

So it helps to break it down into little bites. If you understand some of the pieces, and how those pieces work together and why, then the big picture starts to make sense. 

Just like with any frameworks, when you create a new app it gives you a big directory with tons of folders, files, and tools all ready to go. 

I'll be going through some of the tools, and where they're stored in the directory, here. 

## **Views**

**Path:** resources/views

**Example File:** 'home.blade.php'

Exactly what it sounds like! These are php files that hold all of your html templating and links to external libraries or resources (like Bootstrap). Basically, this is your index.php, blog.php, etc. 

Views are written using a templating language specific to Laravel called blade. Blade gives you a ton of handy shortcuts for standard php actions, like for-loops or conditional statements. 

For example:

	    <body>
        <div class="flex-center position-ref full-height">
            
            @if (Route::has('login'))
                <div class="top-right links">
            			@auth
                        <a href="{{ url('/home') }}">Home</a>
            @else
                        <a href="{{ route('login') }}">Login</a>
                        <a href="{{ route('register') }}">Register</a>
                    @endauth
                </div>
            @endif

        </div>
    </body>

Here, you've got a conditional 'if' statement that starts with '@if' (if the route has a login), defines the corresponding html content to dislay if true, then gives you its '@else', defines the corresponding html content to display if false, and finally ends with '@endif'.

View docs are saved as ____.blade.php.

## **Routes**

**Path:** routes/

**Example File:** 'web.php'

Essentially the same thing as Vue Router (discussed 2 blog posts ago). You call a route, define the url path you want asssociated with the route, and then any functions that need to be performed when that url path is navigated to (like returning the correct view, for example). 

Here's what a basic route looks like: 

	Route::get('/', function () {
    	return view('welcome');
	});

Using the example of a blog, when the user navigates to 'myblog.com/', Laravel will fetch and display the html template and styling stored in file called "welcome.blade.php" in the view folder.

If you wanted to give this route a name, so that you can easily refer to it later, that's pretty easy, too. Here's how we would assign a name of 'welcome to the route above: 

	Route::get('/', function () {
    	return view('welcome');
	}); -->name('welcome');

## **Migrations, Models, and Controllers**

You kind of have to explain these 3 together because they are all tied together. 

Note that all of these pieces have to do with how you interact with your database, and they follow a naming convention that allows Laravel to automatically link them together. 

For example, let's say you have a table called 'posts' holding the title and body of all of your blog posts in your database. You would name your models, controllers, and migration as follows:

	* Table: 'posts'
	* Migration: create_posts_table.php
	* Model: 'Post.php'
	* Controller: 'PostController.php'
	

We'll start with migrations:

### **Migrations**

**Path:** /database/migrations

**Example File:** '2017_11_02_201107_create_posts_table.php' 

All a migration does is set up a table for you. It works in place of the 'Create Table' command in sql. 

If you wanted to create a table to hold your blog posts, you would type 'php artisan make:migration create_posts_table --create=posts' into the command line and hit enter.

Now, you would have a file titled 'YYYY_MM_DD_######_create_posts_table.php' in your migrations folder. 

To set up the fields in your table, you would do type something like this in the section of that php file called 'public function up()':

    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title);
            $table->text('body');
            $table->timestamps();
        });
    }

Now you can run 'php artisan migrate' in your command line, and it will automatically create a 'posts' table in your database, with a primary key field called 'id', a string field called 'title', a text field called 'body', and a field that holds the date and time when each record is created. 

If you ever want to undo creating a table, you can just run 'php artisan migrate:rollback'. Pretty nifty. 

### **Models**

**Path:** /app

**Example File:** Post.php 

This gets a little weird. Models are still hard for me to conceptualize, but I'll do my best. 

From what I can tell, the biggest benefit of Laravel is how easy it makes interacting with databases in your website. 

Laravel uses something called ORM, or object-relational-mapping. Basically, ORM just allows you to interact with your data using an object-oriented programming language (like PHP, Python, etc.) rather than having to write tedious sql queries in a seperate application (like PSequel). You can still use PSequel with Laravel, but you don't HAVE to. You can do all of your database structuring, formatting, table, etc within Laravel using simple PHP. 

Laravel's version of ORM is called Eloquent. The Models in Laravel use Eloquent to let you easily interact with all of your tables. 

Therefore ... 

A Model is just a visual, easy-to-interact with representation of a table in your database that allows you to do stuff to it using php instead of sql. 

For example, if I have a table called 'posts', I could create a model for that table by running the following command in the command line: 'php make:model Post'.

Now, because of the naming convention I described above, Laravel will automatically know that this model corresponds to a table called 'posts'. It will look for, and link it to, that table. 

Within my model file called 'Post.php', I can do stuff like this:
	
	class Post extends Model
	{
	  public $colors = ['Blue', 'Red'];

	}

I don't know why I would need colors for my blog posts, but now the variable 'colors' is now available to my entire app by referencing '$post -> colors;'.

I can also set up relationships between fields in different tables here (one-to-one, one-to-many, etc.). If there was only one post allowed per user, that would be a one-to-one. If, more realistically, each user could write as many posts as they want, but each post can only have one author (user), that would be a one-to-many relationship. 

Most importantly, I can reference this model later to interact with the entries in my posts table to do stuff like:
	
* Display All Posts
* Display One Post
* Save a New Post

### **Controllers**

Controllers do stuff. It's where you store your functions. 

You can create a new controller by using 'php artisan make:controller PostController'.

Now, you can call your controller in a route:

For example: 

	Route::get('/', 'PostController@index');

In the example above, when you navigate to the home page ('myblog.com/') it will run whatever is stored in the **index** function in your PostController. This index function will interact with your Post model (and, therefore, your 'posts' table).

Here's what your index function might look like within your PostController.php doc:

	public function index() {
		$posts = App\Post::all();
		return view ('welcome');
    }

To break it down, when you run the **'index'** function, it goes into your model titled 'Post' (or App\Post) and queries for ALL of the records, or all of your posts. Then, it pumps the returned data into your 'welcome' view, where it is formatted and displayed. On your home page, you would likely have some sort of display that involves listing attributes of all of your posts (maybe all of the titles, most recent to least recent). 

There are some conventions for how you name functions within your controllers. 'Index' is typically always used to just display all records within a Model (or given table). 'Show', on the other hand, will typically search for specific post, and return it. 

In a different route, you might use:

	Route::get('/{post}', 'PostController@show');

The part in curly brackets - **{post}** - is called a wildcard. Laravel will always assume that this corresponds to a primary key, unless you tell it otherwise. In this case, that would be the id of each post. 

Now, when you navigate to '**myblog.com/1**' it will run the '**show**' function in your Post Controller. The show function might look something like this:

	public function show($id) {
		$post = App\Post::find($id);
		return view ('post.show',compact('id'));
    }

There's a lot going on here. 

In this example, you will have created another view (or HTML template) called '**show.blade.php**'. It will be used to format how you want a page displaying just a singular, full post to look. That is what's being called in "return view('post.show')". 

Laravel will also automatically take whatever is in the wildcard ( **{post}** ), and allow us to pass it into a function as a variable. In this case, we call that variable **$id**. So, whatever comes after the '/' is the value of $id. 

Then, we run a query on our Post model (or posts table) using ::find($id). This will search for any post that has an id matching the current value of $id:

	myblog.com/1

Here, $id = 1.

Finally, it will pump the returned data into our 'show' view. 

Note that when we pass a variable into a function in laravel, we have to format it so that it can be returned and passed on into a view. This is where 'compact' comes in. 

Without getting too into the weeds, just know that whenever you pass a variable into a function ($variable1, $variable2), you use compact('variable1','variable2') to allow it to be returned and passed correctly. 

## Big Takeaway:

There is a LOT going on behind the scenes in Laravel. I'm not sure I understand most of it, or ever will, which can make learning it a little hard. 

I'm going to go a little off topic here... 

But I think the biggest gap coming into development at this point in history is that you miss out on a lot of the why. 

There are so many incredible tools that cut out a lot of tedious work now, and all of them have evolved over decades because certain parts of programming were a pain in the behind. 

Now, if you've been programming for decades (or at least before frameworks blew up) you not only experienced that pain firsthand, but also had to manually perform a lot of the tasks that are now going on invisibly behind the scenes in a framework. You had to actually go through the motions of setting these things up yourself. 

But with a framework, it's all just magic. If someone has only ever learned to program using frameworks (IE me), he or she may never have to break down the mechanics of what a compiler (like babel or webpack) is doing. He or she may never have to figure out how to write a program that creates and populates database tables for you. 

That's an incredible gift! But it's also really tough to troubleshoot - or innovate - when you have a very limited understanding of what's going on under the hood.  

I hope this doesn't sound ungrateful because I'm constantly amazed by how many generous, talented developers put their brilliant work and tutorials out there for free. 

It's just that I'm a why person. And while there are so many great resources out there by excellent instructors on HOW to do things (Laracasts and Jeffrey Way are a godsend!), there isn't nearly as much if you want to understand why. 

Alright, I'm done. 

In the meantime, I can at least appreciate that Laravel is really powerful. The fact that one command creates a basic user-password authentication setup is ridiculous. 

For now, I'll just have to appreciate the simplicity of the how. 











