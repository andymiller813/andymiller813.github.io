---
layout: post
title:  "Uncontext Creative Coding Project"
date:   November 11, 2014 12:24:00
comments: true
categories: projects
dateShort: "2014-11-09"
---

##Uncontext 
This weekend I attended a meetup group in Portland, OR for a project called [Uncontext](http://www.uncontext.com/).  Taken directly from the website, "Uncontext is an art project where any collaborator can access an unending stream of structured—but seemingly random—data. Use this data to generate art, write poems, control robots, or anything else."  This weekend, the creator of Uncontext, [John Brown](https://twitter.com/thisisjohnbrown), released a new data set for us all to create a project with.  There were 7 different data points written to a web socket every second, all values between 0 and 1, inclusive.  The first two data points were X-Y coordinate pairs, others were floats, and one was just a binary 1 or 0. There were absolutely no rules on what we needed to do with this data, which sparked an incredibly fun creative thought process in all of us.  You can view some of the past project ideas [here](http://www.uncontext.com/literature/) to get an idea. 

##My Project
For Uncontext, I only had about 5 hours to get a project up and running, so I needed to keep it pretty simple.  I really wanted to utilize some of the new material design libraries in Android Lollipop, but when I started looking into it, I realized it would take me too long to execute on the ideas I had.  So, I ended up trying out the facebook spring dynamics animation library, [Rebound](http://facebook.github.io/rebound/), because I've been wanting to do that for a while.  I used the different data streams from Uncontext to apply different properties to "springs" attached to rectanglular Views.  This was simply just scaling the height and width and generating different colors.  For the binary data point, I displayed an image of Randy Marsh as Lorde if the value was 1, and hid it if the value was 0, haha.   

Here's a video of the app in action:

<iframe width="360" height="640" src="/img/uncontext/uncontext.mp4" frameborder="0" allowfullscreen></iframe>

You can find the source for this project [here](https://github.com/andymiller813/Uncontext).  You'll see that in the [build.gradle](https://github.com/andymiller813/Uncontext/blob/master/app/build.gradle) file within the "app" directory, I've included the facebook spring dynamics library called "Rebound" as a dependency, as well as a Web Socket library to make connecting to the data stream easier.  The rest of the magic is in the [MainActivity.java](https://github.com/andymiller813/Uncontext/blob/master/app/src/main/java/andytheandroidguy/com/uncontext/MyActivity.java) file and should be pretty self explanatory.  