---
layout: post
title:  "Networking Basics"
date:   2014-09-24 12:47:00
comments: true
categories: beginner
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

This gets created immediately after you hit the submit button, and then gets attached to the HTTP request.  The HTTP request is then encoded into binary, and then sent off to its destination IP address to be dealt with (I will be doing another post on binary, hexadecimal, and ASCII soon).  This request was probably POSTed to a URL that looks something like www.facebook.com/login, which is simply a folder on facebook’s servers called “login” that contains the code to deal with this exact request.  Specifically, it contains code with the correct logic that “parses” this JSON and checks the entry in a database to make sure it’s a match.  

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

##A Non-Trivial Example
//TODO

