---
layout: post
title:  "Networking Basics"
date:   2014-09-24 12:47:00
comments: false
categories: networking
---
##Client-Server Architecture
A friend asked me to teach him about JSON, API calls, HTTP Requests, and general networking basics at a high level, and I've been wanting to start a blog for a long time, so I decided this is a perfect opportunity to get one going.  So, without further ado, let's get started with the client-server architecture.  

Client-Server architecture is a very common design for sending and receiving data over a network.  I will start with the example of logging in to Facebook.  In your browser when you type in www.facebook.com, your internet browser makes an HTTP(s) request to facebook’s servers (more on HTTP requests later).  A tunnel (physically a wire) is opened up for this connection first to a DNS, or Domain Name Server.  The DNS that owns ‘www.facebook.com’ knows exactly what IP address is behind ‘www.facebook.com’, so it then re-routs your HTTP request to the corresponding IP address. In this case, Facebook owns a large range of IP addresses.  Facebook’s routers will direct your “give me the facebook.com website” request to one of their servers that has the facebook.com files stored on it, and return the necessary files back to you, which are then displayed on your browser! 

####**Glossary: The url you send a request to in a HTTP request is sometimes referred to as an “endpoint”.**

This client server architecture works on a request-response basis.  You (the client) REQUEST facebook.com’s server to give you its data, and facebook RESPONDS with the data.  This is a type of HTTP request called GET.  You are GETting data from the server – that is, you are not sending any data to facebook, you are simply asking them for data.   The only data you are sending them is the request data itself.   In a POST http request, however, you do send the server data.  For example, during the actual login process – where you type in your username and password and hit a submit button, those forms are then added to a POST http request that sends the text you just entered from your machine to the facebook server in the form of JSON (more on JSON later).  So, you’ve gotten the facebook.com site with a GET request, but now you want to POST (send) your login credentials to facebook’s servers so they can verify who you are.  Request-Response.  You are REQUESTing that facebook verify your credentials, and facebook RESPONDS with a success or failure.  This happens almost identically to the GET request, except the request contains a little bit more data attached onto it, and facebook’s servers treat it a little bit differently.  When a (HTTP) request is started, the request consists of headers, and a body.   Headers can include many things, but the most common are what type of request it is (GET, POST, etc.), date, user-agent (what type of device the client is), Host (location of the server), among others.  The body consists of the data you are sending, so for the login POST example, the body will contain your username and password.  For simply retrieving the site itself, i.e. the GET request, the request going TO the server will have only headers and no body, but the response FROM the server will have the data for the site within the body.  

####**Glossary: Sometimes the body of a request or response (to or from the server) is called a “payload”.**

##JSON

JSON stands for JavaScript Object Notation, and can essentially be thought of as a data structure, most similar to a hashtable.  In the facebook login example, when you POST your login credentials to the server, before the HTTP request is even created, a JSON object is created with your username and password.  A typical object for a login environment looks as follows: 

{% highlight javascript %}
{
	'username': 'andymiller813',
	'password' : 'thisisnotmypassword'
}
{% endhighlight %}

This gets created immediately after you hit the submit button, and then gets attached to the HTTP request.  The HTTP request is then encoded into binary, and then sent off to its destination IP address to be dealt with (I will be doing another post on binary, hexadecimal, and ASCII soon).  This request was probably POSTed to a URL that looks something like www.facebook.com/login, which is simply a folder on facebook’s servers called “login” that contains the code to deal with this exact request.  Specifically, it contains code with the correct logic that “parses” this JSON and checks the entry in a database to make sure it’s a match.  An alternative to JSON is XML which has a syntax much more similar to HTML.  

##JSON Parsing

A JSON object is comprised of key-value pairs and nothing else.    In the example above, the string “username” is the KEY that corresponds to the VALUE “andymiller813”.  The KEY is what facebook is expecting to find in the JSON object.  That is, the string “username” is static – no matter who logs in to facebook, “username” will be there.  The value, on the other hand, is of course dynamic – my username might be andymiller813 while yours might be joeblow27. So, facebook’s parsing code on the server-side gets this object, and has code similar to String username = object.get(“username”), and String password = object.get(“password”). Now they have “andymiller813” stored in the variable username, and “thisisnotmypassword” stored in the variable password.  They can then check that the two match together in their database (think an excel spreadsheet with a table of rows and columns – one column of usernames, and one column of passwords) in the same row.  If they’ve found a match, a response code of 201 will be attached to the HTTP response and sent back to the client, as well as a JSON object with something like {“result” : “success”}.  Now, likely many other things will be attached to the HTTP response upon successful login.  I won’t get into too much detail here, but some probable things that would be attached are a cookie, cached on your computer, which allows you to come back to the site without having to login again, as well as other data which allows the next page to load properly and more quickly – such as a profile image URL or an e-mail address associated with the account.  The client will then receive the response, and go forward accordingly.  I.e. on a successful login, the next page will be loaded, and a new HTTP request will be created to fetch the data for the next page.  
JSON objects can be much more complex than this, but it never gets more complex than Key-Value pairs.  For example, you can nest JSON objects.  That is, you can have a Key in a JSON object that corresponds to another JSON object as the value.  Here’s one: 

{% highlight javascript %}
{
	'name' : {
		'first-name' : 'andy',
		'last-name' : 'miller'
	},
	'password' : 'thisisnotmypassword',
	'email' : 'andymiller813@gmail.com'
}
{% endhighlight %}

The password and email keys are implemented the same was as before, but the name key holds another JSON object as its value.  JSON-ception.  These can be nested as many times as desired.  There are also JSON arrays, which are implemented as follows:

{% highlight javascript %}
{
	'users' : {
		'free-users' : [
			{ 'name' : 'andy' },
			{ 'name' : 'john' },
			{ 'name' : 'joe' }
		],
		'premium-users' : [
			{ 'name' : 'billie' }, 
			{ 'name' : 'smitty' }
		]
	}
}
{% endhighlight %}

The users key contains one JSON object as its value, and this JSON object contains two JSON arrays (lists).  One of the arrays has the key “free-users” and the other “premium-users”.  In conclusion, there are many ways to structure a JSON object, but they need to follow this structure in order for a client or server to properly parse them once they get across the internet network. 

## Application Programming Interface - A non-trivial example

So, what I've described so far in this article is an itsy bitsy teeny weeny little part of what is Facebook's internal [behemoth] web **API**, or Application Programming Interface.  We went over  Facebook's login page, but think about some of the other components to the website: There's a sign-up page to register a new account, there's the home feed, the profile feed, profile image, like buttons, comments, news articles, settings/preferences page, and the list goes on and on and on.  Facebook has all of this data stored in a data center somewhere like this one in Prineville, Oregon: ![assets/prineville.jpg]{Prineville, Oregon} 

All of the client applications (There are over 1.3 billion monthly active users as of this writing) are, for the most part, simply GETting and POSTing data to and from the servers.  Recall Client-Server architecture.  Once a user is authenticated on the login page, a GET HTTP request is spawned to GET a populated feed of his/her friends' statuses and images.  This feed component makes up an additional part of Facebook's API and is probably located at some location such as www.facebook.com/feed, which again is just a directory called "feed" within all of facebook's servers that contains logic to deal with these HTTP GET requests.  The logic, in this case, is quite complex.  Facebook needs to dig through, or query, its database for the most recent, and most relevant posts from your friends that it wants to show you, and then return them in order to be displayed.  Here's what some returned JSON might look like:

{% highlight javascript %}
{
	'posts' : [
		{ 
			'ID' : 'Some Unique ID',
			'name' : 'Name of the person who wrote this status',
			'time' : 'Time this status was written',
			'profile_pic' : 'URL of the profile thumbnail image',
			'num_likes' : 27,
			'num_comments' : 94,
			'first_2_comments' : 	
				{	
					'comments' : [
						{
							'name' : 'name of person who posted comment',
							'comment-text' : 'the NSA is watching you'
							'comment-time' : 'time the comment was posted'
							'num-likes' : 'number of likes on the comment'
							...
						}
					]
				}
			...
		},
		{
			...
			Another Post
			...
		}
	]
}
{% endhighlight %}

A few things to note here: notice how the profile picture itself wasn't sent back, but rather a URL for the profile picture.  This is because when you are GETting your feed, you are loading dozens of these posts at once, and images are relatively large file sizes.  If you were to load all of this feed data as well as 20-30 images all in ONE request, you would be waiting for websites to load for a very long time.  Instead, if 20 of these posts are loaded, we now also have 20 profile pictures still to load.  So a new HTTP request from the client is spawned for each one to GET all of the images one by one.  This is why you sometimes notice the images loading on websites _after_ some of the text has shown up.  This request returned the number of likes, but what if we want to see who actually liked it?  When that 27 likes button is pressed, and we want to see the names of everyone who liked the post, the user doesn't want to have to wait several more seconds for this data to load, they want it to already be there!  As soon as this data comes back, another HTTP request might be created to pre-fetch (load them before they are actually displayed) the names of all of the users that liked this post for a much cleaner user experience.  Also, keep in mind there is still loads of other data to be fetched on the facebook home screen alone!!  While the feed is being loaded, your friends list is also being loaded, your own profile picture, your inbox, parts of your profile that are being displayed, etc.  There are many many ways to optimize all of these different requests that are going on including concurrency - these requests can be happening at roughly the same time - as well as caching locally.  These are all topics for other posts in the future.

####**Glossary: Network requests and many other types of computing can be done asynchronously, or at the same time. You might hear this referred to as "in parallel", or "concurrently" **

#Thanks!!!

If you've made it this far, thank you so much for reading.  This is my first blog post on this site and I hope you come back for more in the future.  Getting a high level understanding of programming before diving into it can save you a **TON** of time as it allows you to learn things much more quickly.  Happy coding!

-Andy

