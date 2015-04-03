---
layout: post
title: Markov and me.
---

Uni has been keeping me busy.
Very busy in fact, but I've still had a bit of time to work on my own stuff.
I talked about Markov chains last time I posted, and I've been doing a bit of work with them recently.

I wanted to make something interesting and a little different, a quick project that had the potential to be turned into something more, so I've created a program that generates new song lyrics based on the lyrics of existing songs.

These new lyrics are written in the style of the original songs, using words found within. The new lyrics are generated using Markov chains, so in some places the new songs feel very familiar, and in others they appear completely new. Most of the time the lyrics don't make sense, but it is fun to read through them and it's very interesting to see what is created.

I split the system into two parts, the Markov generator and the web scraper, both written in Python.

###Markov Chains
Markov chains are a memoryless process that undergo a random transition from one state to the next, where the probability of the next state is entirely dependent on the current state of the system.
When modeled they appear similar to finite state machines, with the their transitions being based on probability as opposed to determinism.

These probabilities are found using a given input source.

###Markov Generator
This following example shows how the generator works.

With an input text of:

    the fox ran over the hill and jumped over the dog

The generator first splits the text into individual words, and then for each finds the word that follows:

    the => fox
    fox => ran
    ran => over
    over => the
    the => hill
    hill => and
    and => jumped
    jumped => over
    over => the
    the => dog

The user then chooses a starting word, and the Markov generation can begin.

With a starting word of `the`, the system finds all of the next possible words:

    fox
    hill
    dog

It then randomly chooses one of these, and appends it to the end of the generated text.
This is a great way of avoiding having to actually calculate probabilities, as multiple occurrences of a word just appears in the list multiple times, resulting in a higher chance of it being randomly picked.
For example, if the phrase `the hill` appears in the input text twice, and `the dog` appears once, then a starting word of `the` would be twice as likely to choose `hill` over`dog`.

Say `hill` is chosen, we now have

    the hill

as our generated text.
The process again repeats, looking at the potential next words for `hill`:

    and

Giving us the generated text of:

    the hill and

After a few more iterations, we end up with

    the hill and jumped over the fox ran over the dog

And as `dog` doesn't have any subsequent words, the generation ends.

Although our generated sentence doesn't make sense, we can see how it has resulted from the input text, and the potential power of Markov chains. With a longer and more complicated input text the generated song will appear even less like the input.

In my lyric system, instead of looking at the potential successors of single words it looks at successors of word pairs. The logic however is the same. The successors when using words pairs look like this:

    the fox => ran
    fox ran => over
    ran over => the
    over the => hill
    the hill => and
    hill and => jumped
    and jumped => over
    jumped over => the
    over the => dog

There were some adjustments I needed to make to accommodate for songs, such as keeping in `\n` symbols so that song structure could be represented.
I also separated individual songs by the symbol `#`, meaning the final words in one song would not lead into the starting words of the next.

###Web Scraper
The web scraper is fairly simple.
Using an artist name as input it first scrapes www.metrolyrics.com for the titles of the songs on the artist's page.
It then takes these song titles and scrapes each individual lyric page for the words, returning a list of words for each song.
This list is then passed on to the Markov generator to create the new song.

The problem with my initial implementation of the scraper was that it took an awfully long time to scrape the lyrics of an artist who has many songs. Take Kanye West for example, who has 85 songs on metro lyrics. The system took almost 2 minutes to scrape all of the lyrics, due to each connection taking around a second.

I improved the system drastically by using Python multiprocessing, allowing concurrent requests to happen.
Although still not as fast as I'd like, it was a major improvement.

###Example Output

__The 1975 - "It takes"__

    It takes a bit more than you
    You're alive, at least as far as I can tell you are
    And so am I, just typically drowned in my room
    And I'm not your savior.
    Wrestle to the ground
    God help me now because
    They're just girls breaking hearts
    Eyes bright, uptight, just girls
    But she can't be found with you
    We get back to my house
    Your hands, my mouth
    Now I just stop myself around you.
    A small town
    Dictating all the people we get around
    What a familiar face.
    Do you wanna find love then you know where the city is

###Future
I have two plans for how I want to extend this project further.

First I would like to make a site that allows a user to type in an artist name and it generate a song for them on the page.
This would require me to learn a Python web framework like Django, which could be quite interesting.

I would also like to build a twitter bot that posts song snippets and takes requests.

~ w
