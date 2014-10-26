---
layout: post
title:  "What is Binary?"
date:   2014-09-24 12:47:00
comments: true
categories: networking
---
##Introduction
Binary, also known as Base-2, is a number system, just like the Dewey Decimal system that we all learned in 
kindergarten - counting 0, 1, 2, 3, 4, 5, 6, etc.  The only difference is that the decimal system has 10 
characters to utilize: **(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)** while the binary system only uses two characters 
- **0** and **1**.  It just so happens that binary is the basis of computers and the *entire* internet as 
we know it.  In this post, I will go into detail about how to count in binary, how to convert between 
binary and decimal and vice versa, and what [the heck] binary has to do with computers and the internet.  
In order to fully understand computers and programming, understanding binary is a *critical* step, even at 
the beginner level.

##Counting in Binary
Counting in binary is just like counting in decimal, it just requires a little bit more abstracted 
thinking.  Let's start counting in binary... remember we only have '0' and '1'

![Table 1: Begin Counting in Binary](/assets/what_is_binary/table_1.png)

To figure out what comes next in binary, let's take a look at what we do when we count in decimal

![](/assets/what_is_binary/table_2.png)

In binary, you count by doing the exact same thing.  When you get to 1 in the right-most digit place, you 
are now out of characters to use and you simply start back at the beginning (0) then add one to the next 
place over.  In binary you end up with 10 as the next number: 

![](/assets/what_is_binary/table_3.png)

In decimal, what comes next? You continue to count the right-most digit upwards until you've once again run 
out of characters: 11, 12, 13, 14, ... , 19, and then you follow the same procedure: reset the right-most 
digit to 0 and add 1 to the next digit over: 20.

If you try to do this in binary, you'll notice you run into a problem.  



