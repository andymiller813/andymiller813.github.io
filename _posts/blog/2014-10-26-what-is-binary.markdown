---
layout: post
title:  "What is Binary?"
date:   2014-10-26 12:47:00
comments: true
categories: blog
dateShort: "2014-10-26"
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

If you try to do this in binary, you'll notice you run into a problem:

![](/assets/what_is_binary/table_4.png) 

When you add one to the second-to-right-most digit place over, you get two, which is not a valid number in binary. What do we do in decimal when we've run out of numbers in both the first and second (one's and ten's) digits place (the number 99)? 

![](/assets/what_is_binary/table_5.png)

In grade school, we all learned how to "carry the 1" when adding.  The same concept applies to binary.  In decimal, adding 1 to 99 , we reset the right-most (ones) digit to 0, and carry the one to the next digit place.  Since its again a 9, we do the same thing - reset to 0, and add 1 to the next one over, so we get 100.  Here it is in binary:

![](/assets/what_is_binary/table_6.png)

Get it?  Hopefully counting in binary makes decent sense by now.  Step through this next table carefully and try to remember all of these concepts that I've presented thus far.  The left column will count upwards in binary, while the right column will show the decimal equivalent.  

![](/assets/what_is_binary/table_7.png)

##Additional Information: Converting Between Binary and Decimal

Being able to represent any decimal number (and any letter, for that matter) with just two numbers (0 and 1) turns out to be pretty powerful.  We can convert any number to binary, and any letter, which can then be sent across a wire as a signal as something meaningful. We now know how to count in binary, but how about converting something to binary?  It's actually pretty simple once you understand one more thing about how number systems work.  In decimal we know that each digit place has the following names:

![](/assets/what_is_binary/tens.png)

Whereas in binary we have:

![](/assets/what_is_binary/twos.png)

Notice the pattern?  You can apply this pattern to any number system (base 3, base 4, base 16, etc.).  To get the digits place for any number system, you simply take the base you are in (binary = base 2, decimal = base 10) and raise that the *n*th power, where *n* is the digits place you are looking to find (start at 0).     

Let's look at the number 57348 now:
8 is in the 10^0 place (ones)
4 is in the 10^1 place (tens)
3 is in the 10^2 place (hundreds)
7 is in the 10^3 place (thousands)
5 is in the 10^4 place (ten-thousands)

To get that number, what we are really doing is the following:
(8 * 10^0) + (4 * 10^1) + (3 * 10^2) + (7 * 10^3) + (5 * 10^4) = 57348

Let's apply that same logic to a long binary number 1101101
We have a 1 in the ones (2^0), fours (2^2), eights (2^3), thirty-seconds (2^5), and sixty-fourths (2^6) place, and a 0 in the twos (2^1) and sixteenths (2^4) place.  That right there converts the binary number to decimal for us:

(1 * 1) + (1 * 4) + (1 * 8) + (1 * 32) + (1 * 64) + (0 * 2) + (0 * 16) = 119

This should make good sense to you but a few examples are always good.  Try converting the following numbers to decimal using this pattern:


	1. 111001
	2. 101000
	3. 111
	4. 11011

###Why Binary Matters
Everything on the internet and on your computer is at some point converted down into binary and sent over a wire.  There is an entire system for converting numbers, letters, and other symbols to binary called ASCII.  Here is an ASCII Table: 

![ASCII Table](http://upload.wikimedia.org/wikipedia/commons/d/dd/ASCII-Table.svg)

Typically in a given internet application, data is put into a JSON Object (see my post on JSON and networking here), and eventually converted into binary using the concepts in this post as well as an ASCII table.  Binary can be represented as a signal over a wire like so: 

![Binary Signal](http://www.ece.umd.edu/~davis/Image6.gif)

