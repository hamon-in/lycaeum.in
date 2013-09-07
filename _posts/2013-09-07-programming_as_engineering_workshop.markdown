---
layout: post
title: "Programming as Engineering workshop"
date: "2013-09-07 19:59:32 +0530"
---

> Scientists study the world as it is; engineers create the world that has never been.
>     -- Theodore von Karman

I've been doing a series of workshops at colleges in Kerala titled "Programming as Engineering". The general plan is to inspire rather than instruct. To show students some new possibilities and give them some ideas to think about. This post is to talk about it and to solicit feedback.

The general presentation is structured as five separate topics. These are UNIX, Optimisation, Higher order functions, Data structures and small languages. I spent about month digging through books and articles to figure decide on the topics. I didn't want the topics to be too deep nor too shallow. Just enough to get the target audience excited about the possibilities. I also made the topics completely separate so that I can deliver half the talk in about a day at places where there were time constraints.

The first topic is UNIX which I've already [written about here](http://thelycaeum.in/blog/2013/09/03/text_processing_in_unix). It's mostly a UNIX command line 101 kind of thing with one "real world" exercise. It's probably the best received section but that's because of how elegantly powerful the UNIX command line is rather than the presentation itself.

The second is optimisation. This is based on a chapter in [Jon Bentley's "More programming pearls"](http://www.amazon.com/More-Programming-Pearls-Confessions-Coder/dp/0201118890) where he writes a program to print out prime numbers from `1` to `n` and then tries to tune it. The whole exercise is in C which is what students use in their colleges so they can actually relate to it. Also, the question of printing prime numbers is a common exercise in colleges during the first or second year. To see how they can take something like that and tune is squeeze performance out of it is entertaining for them. I use gnuplot and some things to draw graphs of the various program performances so that the students can really "see" what's going on.

The third is Higher order functions. This is adapted from [Anand's excellent "Python practice book"](http://anandology.com/python-practice-book/functional-programming.html#higher-order-functions-decorators). It's serves as a simple introduction to Python as well as a new programming idiom that the students are not familiar with. It's also been quite well received.

The fourth is data structures. This is based on a chapter from [Jon Bentley's "Programming Pearls"](http://netlib.bell-labs.com/cm/cs/pearls/) where he writes a program to do some sorting while abiding by some memory constraints. It's an interesting exercise since students are usually not asked to do things with constraints. Also, they don't think of ways to solve the sorting problem. It's always a "implement a quicksort" type problem for them.

The fifth is small languages where I introduce two small languages. The first is the under appreciated [graphviz](http://www.graphviz.org/). We draw some Debian package dependency graphs and some state machine transition diagrams using `dot`. After that, we write a tiny preprocessor in Python that converts a textual description of an algorithm into a flowchart. The second is [Gnuplot](http://www.gnuplot.info/). We use it to draw graphs of data from the UNIX and optimisation sections.

The whole workshop has been generally quite popular. If you're reading from a college in Kerala and are interested in having me do this at your institute, please drop me a line. Feedback, as always, is welcome. 
