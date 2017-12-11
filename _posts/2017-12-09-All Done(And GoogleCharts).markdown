---
layout: post
title:  "Week 12: All done! (And Google Charts)"
date:   2017-12-09 01:09:27 -0400
categories: google, googlecharts, charts
---

We just graduated! It feels strange and wonderful to be done, and while the last 12 weeks flew by, I can't believe how much I've learned in a very short period of time.

We had the opportunity to demo our final project this week, and get feedback from local business owners and more experienced developers.

I put together an inventory system for a local coffee and donut shop. While most of the project involved pulling together pieces we'd already learned, I did have to go out a limb for one area in particular: data visualization.

This was by far the hardest part. Reporting tends to be very personal and unique to the user, and trying to anticipate what data the customer would want to see, and how to communicate it effectively within no more than a quick five minute glance (the end user will also be busy making donuts and serving up lattes) was a mental workout. 

I still have a LOT to learn, and will probably switch up my final project to something a little more robust (like D3), but I at least got my feet wet in the basics of graphs and communicating data with Google Charts. 

Here's what I learned:

## **My Love Affair with Google Tools**

From day one of my attempt to become a full-time developer, it's been Google Chrome all-the-time. The developer tools are pretty handy and intuitive.

I also often use Google Docs for things like my resume or meeting minuts for an organization I'm on because: 1. Multiple people can view and edit and 2. You never need to worry about losing your files. 

Finally, when I had just started learning the basics I was also using Google Sheets with JavaScript as fill-in database until I was able to master something more robust. As a long time Excel-devotee (from my time working in Supply Chain), I was shocked to discover how much Google Sheets has improved in the last couple of years. It has a lot of the same functionality as Excel, and a large community that builds additional, fantastic add-ons. JavaScript is a much more transferable, easy-to-use language than VBA also...

I really appreciated that there was now a free, open-source, developer-friendly option for business tools. 

I swear this is not meant to be a Google Ad - it's just a little background on why I thought it might be cool to learn Google Charts to explore more from a platform I was already very familiar with.

## **Google Charts Basics**

When I found Google Charts, I was looking for an easy plug-in to an existing Laravel application that used pretty straightforward JavaScript. 

Google Charts fits all of those criteria, and its guides are very clear and easy to follow. 

To load Google Charts, you simply (typically in the head of your doc):
	1. Include the necessary CDN
	2. Call a function to load the essential packages
	3. Call a function to draw the chart after your packages are loaded:

	// CDN
  <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
  <script type="text/javascript">
  	// Load packages
    google.charts.load('current', {packages: ['corechart']});
    // Run function 'drawChart' after necessary chart packages are loaded
    google.charts.setOnLoadCallback(drawChart);
   </script>

Now you're ready to draw your first chart:
	
	function drawChart() {
      var data = new google.visualization.DataTable();
      data.addColumn('string', 'Name');
      data.addColumn('number', 'Age');
      data.addRows([
        ['Bob', 60],
        ['Sue', 24],
        ['Mary', 39]
      ]);

      var chart = new google.visualization.BarChart(document.getElementById('chart_div'));
      chart.draw(data, null);
    
    }


To break it down:
	
1. You define the data your chart will be modeling within the 'new google.visualization.DataTable()' call, and store it in a variable called 'data'. Your data will be represented as an array of arrays (with each record as its own array), and you can easily pass in data dynamically as long as it is formatted that way
	
2. Google Charts then provides handy functions to manipulate your data, like 'addColumn' (where you define the name and type of a required field), and 'addRows' (where you actually fill in the data for each of the fields you define with addColumn)
	
3. After you have your data ready, you then call 'new google.visualization.___Chart(document.getElementById('____'), to specify the type of chart you want (Bar, Pie, Line) and specify where you want the chart to be drawn in your html page by associating it with an element's id
	
4. Finally, you call 'chart.draw(data, null)' to actually draw the chart. You have the ability to define a set of "options" (like line size, line color, point size, label/legend behavior and placement, etc.) in drawChart before this function, and then call 'chart.draw(data, options)' to enforce the specific styling/behavior you've defined. 

To tell Google Charts where to put the chart in your html page, simply include a div, with the id of 'chart_div' (used in the statement above):

	<div id = "chart_div"></div>

## **Big Takeaway:**

Google charts are extremely customizable, and there's a decent community of support out there if you get stuck. While the results are not as beautiful or powerful as they would be using a tool like D3, it is a lot easier to learn, and probably a great option if you're stuck with a very limited timeline. 


