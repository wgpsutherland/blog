---
layout: post
title: Getting technical.
---

Now that *Gradients* is at a level that I'm quite happy with, I'm going to be working on performance improvements as opposed to adding new features.
This is essentially going to boil down to reducing unneccessary server requests and making sure the application isn't rendering views when it doesn't need to.

I just wrestled my through learning the *require.js* optimiser and now have 1 single .js file as opposed to about 50 HTML and .js files.
It makes the network tab look a lot cleaner at least!
I'm now reading whether I should be using a CDN or not. 
I currently am - I think it's the right thing to do - but I know that I could reduce the number of server requests by a significant amount if I was to have it all coming from the same place.

I'm also going to work on SEO, though I'm not sure how it's going to work for a single page javascript application... I might try and do it for this blog instead.

Oh, and Happy New Year!
I think this year's going to be a good one.

~ w