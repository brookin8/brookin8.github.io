---
layout: post
title:  "Week 2: JavaScript- JSON, XML HTTP Requests, APIs, and Testing"
date:   2017-10-01 01:09:27 -0400
categories: JavaScript, JSON, API, XML, Testing, Mocha, Chai
---


<br>

### **Tools Covered:** APIs, Mocha, and Chai
### **Languages Covered:** JavaScript- Standard, JSON

<br>

This week was all about added functionality. Although many people now interact with JavaScript through a library, like JQuery or React (more on that in a later post), we went straight JavaScript in our files. We covered the basics, like ensuring a page is fully loaded before 'turning on' your JavaScript functions, assigning 'click' actions to buttons, and pulling and interacting with data from an API. Moving one step beyond this foundation, we also covered not only how to test your JavaScript using a combination of Mocha and Chai, but how to build your JavaScript from day one in a way that enables you to test it more effectively as you go. 

<br>

#### **JSON**

'JavaScript Object Notation' exists so that when data is exchanged, both computers and humans can easily read and access it. 

With JSON, data is formatted in an associative array, with keys and values. 

For example, if you're requesting data about an employee named Jane Smith from a database, you might get back a JSON file that looks like this:

	{
	{
	  "firstName": "Jane",
	  "lastName": "Smith",
	  "isAlive": true,
	  "age": 30,
	  "address": {
	    "streetAddress": "21 2nd Street",
	    "city": "New York",
	    "state": "NY",
	    "postalCode": "10021-3100"
	  },
	  "phoneNumbers": [
	    {
	      "type": "home",
	      "number": "555 555-5555"
	    },
	    {
	      "type": "office",
	      "number": "555 555-5555"
	 	  },
	 ],
	 },
	}

To access a value in the JSON array, you would do the following:
1. Set a variable equal to a parsed version of this array. JavaScript has some built in functions to help you with this. For example:
	var results = JSON.parse(--JSON ARRAY GOES HERE--);
2. Access the values you want using their associated keys:
	var age = results[0].age
	var officeNumber = results.phoneNumbers[0].number

JSON is very useful for APIs, as well as package managers like npm. APIs will return their results in JSON format, so that a human can easily find and pull out the data he or she wants. npm stores its installed packages and dependencies in JSON so that you can easily keep track. 

<br>

#### **XML HTTP Request**

An XML HTTP Request allows you to request, receive, or send data to and from a server after your web page has loaded. You can then update your web page accordingly, without ever having to reload it. 

Here's some example JavaScript:

	var xhttp = new XMLHttpRequest(); (Setting a variable equal to your request)

	xhttp.onreadystatechange = function() { (When the page is loaded ...)

	    if (this.readyState == 4 && this.status == 200) { //if there are no errors...
	       document.getElementById("refreshedSection").innerHTML = xhttp.responseText;
	       //You are refreshing a particular section with the ID 'refreshedSection' by resetting its innerHTML to be the response you get when you send your XMLHttpRequest
	    }
	};
	xhttp.open("GET", "filename orURL", true); //Starts the request- it tells you to GET the info from either a filename or a URL (could be an API). 
	xhttp.send(); //Sends the XML Http Request

<br>

#### **APIs**

API stands for 'Application Programming Interface.' In my prior life (pre-coding), when a developer would start mentioning API interactions, this is where I would get overwhelmed. But really, all that an API is doing is making it easier for one software component to get exactly the data it needs in exactly the format it needs from another software component. 

In the example we used this week, our website that we built needed to get refreshed weather data from another website (we'll call the 'weather website') on a daily basis. Our website was always going to need the same data points: general outlook, temperature, precipitation, etc. In fact, most websites that use the weather website are looking for exactly the same thing. 

In anticipation of this, the weather website has set up an API that is standard, predictable, and easy to interact with. 

When you send an XML HTTP Request for it's data, it expects a zipCode input from you (to tell it what city you would like weather for). This zip code usually goes in a specific spot in the URL you send, and can be manipulated with JavaScript using a variable for zip code.  Example: http://api.openweathermap.org/data/2.5/weather?zip=<**zipcode**>&us&appid=c9b81efe12b3ffd7f55a54235d37c77a

The http://api.openweathermap.org/data/2.5/weather? will always be the same, and the &us&appid=c9b81efe12b3ffd7f55a54235d37c77a will also always be the same (representing that the city is in the US and an ID for the account I created for the website). But the zipcode piece will change depending up on your request.

When you submit this request, the weather website will send back a JSON file with all of the data you need. This format will be the same no matter what you zip code you put in. 

For example (You can see this by either going to the URL you send the request, too, or opening your developer tools in Google Chrome and viewing the console):

{
  "coord": {
    "lon": -84.5,
    "lat": 38.05
  },
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "clear sky",
      "icon": "01d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 287.41,
    "pressure": 1025,
    "humidity": 55,
    "temp_min": 286.15,
    "temp_max": 289.15
  },
 } 

To access these values, just like in the example above, you assign the results to a variable, and use syntax like this:
1. var temp = results.main.temp
2. var general = results.weather[0].description

You hear a lot about RESTful APIs these days, too. Twitter, Facebook, and Google all use RESTful APIs. While we didn't really cover this topic, I did a little bit of independent research on my own. 

I'm a 'why?' person, so from what I gather it exists mainly to standardize URLs and API structure to reduce the learning curve when working on a new project. It also can be used with any language, and each request is a new, seperate thing (no data is stored on the server between requests). I think that the main value lies in having a predictable framework to work with, and scalable, interchangeable servers. Because I am still very much on square one with this one, I'll just recommend a good source I found: https://www.sitepoint.com/developers-rest-api/. 

<br>
#### **JavaScript Testing: Mocha & Chai**
Setting up a JavaScript testing framework is a great way to avoid refreshing your browser a hundred times.

It's also a great way to build in a check to make sure you hit all of your requirements (or 'user stories' in Scrum terms) early on. 

You can use testing frameworks to test individual functions in your code one by one, and print all of the combined results out on a webpage. Anything function passes your test criteria will have some type of intuitive designation (in this case a green checkmark next to the cases that passed), and anything that fails will also be clearly called out (a red x). 

We used Mocha and Chai this week to do our testing. 

Both Mocha and Chai are installed using a package manager, in our case npm. Mocha is the testing framework, and provides the bones to run through your test cases and print the results to a webpage. Mocha can be paired with any version of what's called an 'assertion library'.

An assertion library is just a JavaScript library (common, useful functions that have been pre-defined in JavaScript and assigned to easy, intuitive syntax) that focuses on defining common testing functions for use. In this case, we used Chai. 

In your application directory, you create a new folder called 'test'. Within this test folder, you create a 'testrunner.html' document and a new javascript document to hold all of the tests you will write. 

In the testrunner.html doc, you include links to (after installing both mocha and chai with npm):
1. the mocha css document.
2. the mocha and chai javascript docs in node modules
3. the javascript doc you want to test
4. the javascript doc with your tests in it

You also include a div with the id "mocha" (this will display your test results), and two boilerplate mocha functions:
1. mocha.setup('bdd'); (This sets up mocha to run)
2. mocha.run(); (This tells mocha to start running)

For example (I'm jotting in my note format, but you would use standard HTML): 

	Head:
		Title: Mocha Tests

		Link: rel="stylesheet" href="../node_modules/mocha/mocha.css"
		(Found in the node modules folder: Mocha CSS sheets)

	Body:
		Div: id="mocha"

		Testing Suite JS Docs:
			*Script: src="../node_modules/mocha/mocha.js"
			*Script src="../node_modules/chai/chai.js"
			*Script: mocha.setup('bdd')

		JS Doc(s) you want to test:
			*Script: src = '../app/main.js'

		JS Doc with your tests defined:
			*Script: src = 'testArray.js

		Start Running Mocha!:
			*Script: mocha.run();

In your testing doc (testArray.js or whatever you call it), you start by declaring the assert language you're using. For example: var assert = chai.assert. 

You then describe your function, and assert what it should do to pass your test. For example, if you have an Adding function in your main.js doc you want to test with a few cases:

	describe('Adding', function() {

		it('add 1 and 1', function() {
			assert.equal(adding(1,1),2);
		});

		it('add 2 and 3', function () {
			assert.equal(adding(2,3),5);
		});
	});

This example would test that your function takes in 1 and 1, and gives you 2. It would also check a second case that takes in 2 and 3, and should give you five. 

In the testrunner.html page, if it passes it will show up like this:

	Adding
		(green check mark) add 1 and 1
		(green check mark) add 2 and 3

One things to note about testing: mocha does NOT recognize global variables when testing a function.

This means that the more you can 'compartmentalize' (that is, keep everything local) and simplify functions, the easier they will be to test.  

<br>

## Big Takeaway:
Most of the time, we're all trying to do the same things. The more we can standardize the way that software interacts with other software, and the more that we can make it intuitive for humans to read and work with, the easier our jobs become. 

Additionally, test early and test often (if you can). Testing can be a luxury if you're really pressed for time, but it will be much more attainable if you build your code in a way that will allow you to easily zero in on trouble spots. 






