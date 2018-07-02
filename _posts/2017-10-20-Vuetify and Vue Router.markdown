---
layout: post
title:  "Week 5: Vue.js: Part 3 (Vue Router)"
date:   2017-10-22 01:09:27 -0400
categories: Vue.js, Vue, Router, Frameworks, Events, Single Page 
---

We broke into smaller groups this week, and mine worked through a project the previous bootcamp had built. We were charged with translating the existing AngularJS frontend into Vue, while utilizing their existing backend. 

After diggin in, we decided to completely scrap all of the existing Angular and start fresh with Vue.

Fair warning- all of the learning shared here is self-taught this week (meaning it may not be 100% there).

<br>

## **Vue Router**

As a background, it helps to understand the concept of a **Single Page Application** (SPA). 

Let's use an example:

Say you want to build a website that starts off with an intro page about yourself. You then want to also include a blog, with a main blog homepage and pages for each post. 

The basic way you would approach this problem is to start with an index.html page to hold your intro. You might then create a blog.html to host your blog homepage, and a post1.html, post2.html, etc. for each post. Finally, you create a navigation bar that links you between all of these different pages. 

An SPA would create index.html - and that's it. 

With an SPA, you have only one page that is dynamically re-rendered and updated as the user interacts with it.

The easiest way to accomplish this with Vue is to utilize the [official vue-router library](https://github.com/vuejs/vue-router). To get started, you simply npm install it, and use it in conjunction with Vue CLI.

Before I start, here's a resource I found immensely helpful: [Scotch Tutorial](https://scotch.io/tutorials/getting-started-with-vue-router).

When you create a new template in Vue CLI, you have the option to include Vue Router. If you select "Yes", it will automatically create a src/router/index.js file for you. In this file, it will also have already imported Vue and Router from the appropriate files. 

Underneath this, you simply import the components you will be rendering on the page as different views. You then call **Vue.use(Router)** , and export ("default") a new Router. It's within this portion that you define your different routes.

### Here's an example (src/router/index.js): 

	import Vue from 'vue'
	import Router from 'vue-router'
	import Home from 'src/components/Home'
	import Blog from 'src/components/Blog'

	Vue.use(Router)

	export default new Router({
	  routes: [
	    {
	      path: '/',
	      name: 'Home',
	      component: Home
	    },
	    {
	      path: '/blog',
	      name: 'Blog',
	      component: Blog
	    }
	  ]
	})

#### To break it down:

Here, we are creating a router view for both a main homepage, and a blog homepage. 

* **'path'** : url path where the route will be accessed
* **'name'** : the name of the route
* **'component'** : the component being routed to

<br>

Next, in your src/main.js, under where you import App.Vue, import Vue Router. In your Vue Instance, include 'router'.

### Example (src/main.js): 

	import Vue from 'vue'
	import App from './App'
	import router from './router'

<br>

Finally, in src/App.vue, you will call both router-links and router-view in the template: 

### Example (src/App.vue): 

	<template>
  		<div id="app">
		    <router-link :to="/">Home</router-link>
		    <router-link to="/blog">Blog</router-link>
		    <router-view></router-view>
  		</div>
	</template>

#### To break it down:

* **'router-link'** : Links you can click on to render the different routes you created
* **'router-view'** : Placeholder where each different route gets rendered

<br>

Now, as you render the page it will initially go to whatever you define your homepage to be. To navigate to your blog, you simply click its router-link, and the page will dynamically re-render and show your blog page without needing to be refreshed or directed to a different HTML doc. 

Magic! 
<br>

## Big Takeaway:
In this journey, it sometimes seems like you learn rules only to figure out how to break them. 
Everyone knows: 'When in doubt, google.'

But do you know what might be even more important?

When NOT in doubt, google. The realms of what's possible in coding are ever-expanding.








