---
layout: post
title: A matter of security.
---

This post is a summary of my attempts at making *Gradients* more secure after I tried breaking it.
I have waited a while before posting it as I needed to actually fix the security errors before I talked about them.

###The Challenge

I set myself the challenge of trying to break *Gradients*, to see where it's vulnerabilities were.

I decided to try and see if I could fool the system into thinking I was another user, without the need for knowing their password (or even their username).
I also attempted to access the server through curl requests, meaning that the website and subsequent login could be bypassed entirely.

###Structure of Gradients

*Gradients* currently stores user IDs as auto-incrementing integers.
This means that it is very simple for an attacker to a) guess what a valid user ID is and b) find many subsequent user IDs once it has found a single valid one.
I plan on changing the user ID to be a UUID, as although this would simply be security through obscurity, it would provide some advantages:

Unless the attacker was somehow able to gain access to the database, guessing 1 user ID would not give them any indication as to what any others are.

*Gradients* also didn't do any server side validation.
It simply checked client-side whether the user page trying to be accessed was the same as the user stored in the cookie.
If it was, access was granted.

I knew due to this that getting access to information I wasn't meant to see would be very easy.

###The Attack

Through it being so easy to guess what a valid user ID was and the fact that a cookie storing a user ID was the only way access to a page was determined, I simply had to type the following into the developer's console until I hit gold:

    document.cookie="user_id=x"

Going through different values of x and reloading the page allowed me to access the pages of every single user in the system.
This was obviously not good.

###The Response

The ease of which I was able to gain access to anyone's account prompted me to research heavily into web security.
I knew that I would need to:

* Introduce server side validation for each request.
* Store an encrypted access token as a cookie that relies on a secret hidden on the server to be decrypted.
* Have an expiration for the access token.

======

* write the middleware validation so that each request goes through it
* setting up the access token creation on login and storing it as a cookie
* overriding backbone sync so that every ajax call adds the cookies to the header
* how I rewrote the url sending mechanism that prevents a valid user from accessing info about another user through curl requests 

~ w