---
layout: post
title:  "Week 1: Boostrap, Github, Gulp, and more"
date:   2017-09-24 01:09:27 -0400
categories: Bootstrap, Github, npm, Gulp, Sass
---

Before I get into the individual tools, understanding the whole "front-end" vs "back-end" piece early on was crucial for me. 

Front-End (also called "client-side") consists of everything that is processed *in* your browser. It is more focused on the look and feel of a project, and how the user interacts with the site. 

The 3 main front-end languages are:
  
  1.**HTML** (Determines the general framework of your site)
  
  2.**CSS** (Determines the styling of your site)
  
  3.**JavaScript** (Adds interactivity- IE forms, buttons, log-igns - and animation to your site)

I will get more into Back-End later on, but essentially it's "server-side" functionality (mainly database interaction).

<br>

### **Bootstrap**

Ok, Bootstrap is what inspired the mild rant in my intro. Boostrap was very intimidating to me for a long time mainly because I couldn't wrap my mind around exactly what it was. Once I finally understood, I was really frusterated. 

As it turns out, the idea behind Bootstrap is actually extremely simple and intuitive. 

While some people love the intricacy of front-end design, and adjusting tiny fragments of their CSS doc until everything lines up and looks juuuust perfect, for many people (like me) it's a nightmare. 

Lucky for us, the lovely people at twitter created some beautiful, complex css styles for all different kinds of HTML elements, and categorized them all in hundreds of different classes. 

Essentially, it's just a really robust CSS document that's already written for you. There's also javascript functionality that can be easily applied. 

All you have to do is link the Bootsrap CSS and Javascript documents to your HTML page in the head (just as you would with your own CSS or Javascript docs), and then read the Bootstrap instructions or google until you figure out which classes and functions you would like to apply to your page. 

If you want to customize bootstrap, you can do so through the official getbootstrap website, and then download your resulting CSS doc and link to your HTML Page. OR, you can simply link your own CSS document AFTER you link the Bootstrap doc, or directly include style tags in your page. 

Note- you can either download the Bootstrap CSS doc so that it lives in your repository, OR you can link to the hosted docs like so: 

    link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/css/bootstrap.min.css" integrity="sha384-/Y6pD6FV/Vv2HJnA6t+vslU6fwYXjCFtcEpHbNJ0lyAFsXTsjBbfaDjzALeQsN6M" crossorigin="anonymous"

<br>
### **Github**

Again, so many people use and love github, and yet for many of us it can feel like we've run into some gatekeeping.

I honestly believe that is not the intention of anyone involved, yet I had the hardest time finding a simple explanation of the purpose and value of github.

It comes down to gaining a solid understanding of 2 things:

  **1.** Web hosting services

  **2.** Git

Once you've delved into version control and Git, you begin to understand that at the core of everything is the concept of having what's called a "central repository." Basically, many different people can work on, edit, and save content loaded into one storage location. It helps for me to think of it as a Sharepoint site document. There is a master version of the document that you will eventually submit, or want to be the referenced version of something. You have to check the document out, make whatever changes you want, and then submit that version for review by the site admin. If they approve, your changes will ne incorporated into the master version of that document. 

Git is basically the same thing. You have a group of developers working on an application. Each can download the current version of the app from a central repository (or storage area), create a new branch, make all of their desired edits, and then submit their changes to whoever has control over the final, master version for approval and incorporation into the application. 

In a company, the central repository probably lives on the company's servers. What github is doing is essentially providing a FREE storage place for central repository's through a universally accesible website. Therefore, developers from all over the world can work with and contribute to different developers projects that live on Github. 

The process still works the same- you download the current master version from Github onto your desktop. You create a new branch, make your changes, and re-upload (or "push") your changes back to Github. Then you can create a request for your changes to be approved, and "merged" or incorporated into the master version. 

OR you can create your own brand new repository, download or "clone" it onto your computer, and go from there. 

Github also allows each user to host one website for free, that can be accessed by typing username.github.io into the web browser. Github will hold all of the required documents and assets for that website. 

<br>

### **npm**

npm's value is two-fold: 1. It serves as a package manager (automates the installation, upgrading, and configuring process), and 2. It makes it easy for JavaScript developers to share and reuse code.  

These bits of reusable code are called packages or modules, and can easily be downloaded and applied with npm. 

I'm not going to go too much further because the npm website actually has a fantastic explanation: https://docs.npmjs.com/getting-started/what-is-npm

<br>

### **Sass**

Unlike everything else descriped here, Sass in actual programming language (like HTML, CSS, or Javascript). It's basically a better CSS, that can then be translated into actual CSS. Sass documents end in .scss. 

Sass allows you to do a lot of things you can't do in CSS, like create and use variables, do calculations (for numerical styling values like width), use element nesting when defining class characteristics, @import other Sass documents to automatically apply their styling, and @extend to have certain classes inherit characteristics from other classes.

The common practice is to code in Sass in your .scss docs, and then run code to translate it into CSS for production/serving your website. 

This can be easily accomplished with...

<br>

### **Gulp**

Gulp is used to easily automate repetitive tasks.

Rememberthose packages of useful code developers like to share with npm? This is one of them. 

You download gulp using "npm install gulp" in your command line (Look up the actual, detailed instructions if you're actually implementing- this is just for high level understanding). 

You then add a gulpfile.js (js for javascript) to your directory, and voila. 

Within gulpfile.js, you define the gulp tasks you would like to do using gulp.task(task_name, function (){}), and any variables they may require. Often, the gulp task you are looking for is easily google-able and downloadble using npm. 
<br>
<br>

**Commonly gulp-ed tasks include:**
  
*Translating multiple Sass documents into one, unified CSS document
  
*Compressing new/modified images
  
*"Minifying" CSS & JavaScript files (meaning, removing all of the blank space to reduce the file size)

**Or my favorite:**
  
  *Auto-refreshing your browser window when you make a change to your HTML, CSS, or JavaScript files


<br>
For example, to translate Sass documents into CSS, you might google "gulp Sass into CSS".

You would then find that you need to "npm install gulp --sass" in your command line. 

Then, in your gulpfile.js, you would add var sass = require ('gulp-sass') - the package you just downloaded. At the bottom, you would also include gulp.task('sass', function() {
	return gulp.src('source_directory/file.scss')
	.pipe(sass())
	.pipe('gulp.dest(destination_directory'));
})

To break it down, this function first grabbed your current scss document from your source directory. It then piped this through the gulp 'sass' function you downloaded with npm. Finally, it piped the results of the function to your destination directory. 

You now easily run this task using "gulp sass" in your command line whenever you update your scss documents. 

<br>

## Big Takeaway:
Developers are smart, generous, and lazy - and that's a beautiful combination. Many of us are looking to do the same tasks, and instead of every developer having to start from scratch, the community has created a number of amazing tools to share great code so that we can continue solving new and more interesting problems.  







