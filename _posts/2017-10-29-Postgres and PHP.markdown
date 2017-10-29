---
layout: post
title:  "Week 6: Postgres and php"
date:   2017-10-29 01:09:27 -0400
categories: php, databases, postgres, sql
---

On to the back-end!

We're getting into some pretty powerful stuff now. While I've had some light experience with SQL in the past, php is pretty much brand new. 

## **Postgres and SQL**

Supply chain never gets the budget for new tools. That budget typically goes to sales, marketing, and finance. I get it - you have to have sales to need a supply chain in the first place, but it doesn't make the situation any less frustrating. 

Which is why every supply chain role ever demands that you be an excel expert. In lieu of real tools, we have to build our own with popsicle sticks, glue, and vba. After all, who would learn VBA by choice? (Not a judgement if the answer is you... it's just not my personal favorite).

All of this to say, for the first few years I was in the workforce the only real tool I had in my toolbelt to do reporting or analysis was excel. 

But then I moved into distribution (a field that demands up to the minute, real-time data), and I started learning SQL.

At the time, SQL was the most powerful, coolest thing I'd experimented with. I suddenly had access to important data right at my fingertips. It made me feel like a programmer (even though I clearly wasn't), and I loved being able to magically input a few lines of code and suddenly have a clear view to what was going on all across our network. 

This was way better than excel. 

Needless to say, I was really excited this week to return to the tools that initially inspired me to want to become a developer. While most of my previous experience was in conjunction with an Oracle ERP system (and, therefore, MySQL), I loved getting to know it's open-source cousin, Postgres. 

And now, into it.

To create a database for use in your web app using Postgres, the steps are straightforward and fairly simple:

1. Download psql and your gooey of choice (we used PSequel)
2. Open psql in your command line by typing 'psql'
3. Create your database with the following 2 commands (fill in values where there are parentheses): 
	a. create user (username) with password '(password)';
	b. create database (database_name) with owner (username from 3a)
4. Close psql using '\quit'
5. Open PSequel
6. Enter the host (localhost), port (5432), user (username you just created), password (password you just created), and database (database you just created).
7. Start creating and manipulating your table in the 'Query' tab using sql

At this point, we start pulling in php. 

## **php**

Php is AWESOME! While I loved all the cool things I could do with JavaScript, it seemed like there were a lot of workarounds, add-ons and band-aids involved (like frameworks, Express Servers, etc.). Just to be able to use require or include browser-side, we had to download a compiler tool with webpack (Vue CLI).

Php, on the other, is able to easily include other php docs, comes with its own built in server, and can do a lot of what JavaScript can WHILE being able to easily communicate with databases. It's also object-oriented, which gives it some more muscle. 

The only thing I don't love about php? It's syntax is pretty similar to JavaScript, except that there are dollar signs friggin everywhere. I have examples of this coming up soon.

So, once you have your database up and running in Postgres, you can start building your website with php. 

Assuming you have php installed, you can just start by creating an index.php (yes, instead of your standard index.html). You structure it exactly the same as an html doc, except that it's now capable of processing php. 

Somewhere in the body of index.php, you open a php tag like so: '<?php '.

You then add a function to connect to your database:

	function getDb() {

		$db = pg_connect("host=localhost port=5432 dbname=database_name user=database_owner password=password");

		return $db;
	}

Then, it's time to fetch the data: 

	function getItems() {
		$request = pg_query(getDb(), "
			SELECT from table_name;**
		");
		return pg_fetch_all($request);
	}

In place of the select statement, you can substitute any sql query you'd like depending on the data you are trying to grab. After that, you close your php section with a tag: '?>'

Now, all that's left is to actually render the data. A common way of doing this to use a foreach statement. 

Here's an example:
	
	<?php foreach(getItems() as $item) { ?>
		echo "<td>$item['id']</td>";
		echo "<td>$item['name']</td>";
	<?php } ?>

Notice that you can open and close one php section where you begin/call a function, put in some html code you would like the php function to operate on in the middle, and then close the function in a second, seperate php section with its own open and closing tags.

What if you wanted to clean up your code, and compartmentalize your php functions into a seperate document?  

You create another php doc, and then include it in your original index.php. 

For example, you could pull out the code above that connects to the database into a doc titled database.php. Then, you simply use (in your index.php, within php tags):

	include('./database.php');

Voila! No special tools or compilers required. 

## Big Takeaway:
Php is built to do powerful things, with little effort. 

What also probably helps is that the basic logic so far is pretty similar to JavaScript (conditionals, for loops, etc). I think the learning curve may be flattening out! 











