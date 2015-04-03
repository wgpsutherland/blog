---
layout: post
title: Markov and me.
---

Uni has been keeping me busy.
Very busy in fact, but I've still had a bit of time to work on my own stuff.
I talked about Markov chains last time I posted, and I've been doing a bit of work with them.

I wanted to make something interesting and a little different, a quick project that had the potential to be turned into something more.

###Markov Chain Explanation
Markov chains are a memoryless process that undergo a random transition from one state to the next, where the probability of the next state is entirely dependent on the current state of the system.
When modeled they appear similar to finite state machines, with the their transitions being based on probability as opposed to determinism.

The probabilities are found using a given input source.

###Idea
I decided to make a program that takes song lyrics and produces new songs based on them. The generated songs would have a similar feel and style to the existing ones. They would most likely not make any sense.

It was split into two parts, the Markov generator and the web scraper, both written in Python.

###Python Markov Generator
This following example shows how the generator worked.

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
This was a perfect way avoid having to actually calculate probabilities, as multiple occurrences of a word would just appear in the list multiple times, resulting in a higher chance of being randomly picked.
For example, if the phrase `the hill` appeared in the input text twice, and `the dog` appeared once, then a starting word of `the` would be twice as likely to choose `hill` over`dog`.

Say `hill` was chosen, we now have:

    the hill

as our generated text.
The process again repeats, looking at the potential next words for `hill`:

    and

Giving us the generated text of:

    the hill and

After a few more iterations, we end up with

    the hill and jumped over the fox ran over the dog

As `dog` doesn't have any subsequent words, the generation ends.

Although our generated sentence doesn't make sense, we can see how it has resulted from the input text, and the potential power of Markov chains. With a longer and more complicated input text, the generated one will look less like the input, and more unique.

In the lyric system, instead of looking at the potential successors of single words it looks at successors of word pairs. The logic however was the same. The successors would look like this:

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

###Python Web Scraper

###Python Parallelism

###Example - The 1975, "It takes

    It takes a bit more than you
    You're alive, at least as far as I can tell you are
    And so am I, just typically drowned in my room
    And I'm not your savior."
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
