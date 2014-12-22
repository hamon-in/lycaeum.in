---
layout: post
title: "Non linear functions"
date: "2014-12-22 23:34:35 +0530"
---

I conducted a session on [non linear functions](http://thelycaeum.in/blog/2014/12/10/public_session_on_non_linear_functions/) recently. This is a post describing what we talked about. I had some worries about this since it was not on a purely programming topic and it involved some preparation and tools which I wasn't used to. However, the two hour presentation went quite well and I'd like to summarise it here since I am planing a "part 2" soon.

Firstly, the whole presentation involves a number of graphs and plots. I recently used [ipython notebook](http://ipython.org/notebook.html) for a corporate python training event and was very happy with the tool. I wanted to use the same thing here and use the plotting functionality of matplotlib to show all the graphics. However, due to a schedule crunch, I wasn't able to learn the API well enough to do what I wanted. I opted to [write the tools I needed](https://github.com/TheLycaeum/non-linear-functions) by myself using PyGame. This took a few hours on a train (one of my favourite environments to get work done). I was using the book [chaos, fractals and dynamics](http://math.bu.edu/people/bob/books.html) which is a high school level book on the topic. I first read it during my early college years and it was the first book outside of my syllabus that I actually worked through from beginning to end.

The first thing that we discussed is iteration. This is the idea of repeating a function on an initial value (called the `seed`). So, if the function is f(x) = x+1 and the seed is 2, our values would be


    f(2) = 3
    f(f(2)) = 4
    f(f(f(2))) = 5
    .
    .
    .

The series of numbers 3, 4, 5 is called the `orbit` of 2 under the function `f`. The behaviour of functions under iteration for different seed points is what we're going to look at.

Firstly, there are some points for which the value doesn't change under iteration. e.g. The orbit of 0 under f(x) = x<sup>2</sup> will be a series of 0s. 0 is what is called a `fixed point` for this function. We can write a small program to display the orbit a number under a function like so


     def iterate(seed, iters):
         while iters:
            print seed
            seed = seed ** 2
            iters -= 1


You can try running this for 0 and see that it really is a fixed point. An interesting example is how `0.739` is a fixed point for the function `cos(x)` (where x is expressed in radians).

The second interesting thing you can see in an orbit is what's called a set of `periodic points`. This is, for example `-1` under f(x) = 1/x. The orbit will look like this -1, 1, -1, 1, -1, 1, ... -1 is a periodic point with `prime period` 2. This means that you'll get back to -1 after 2*n iterations for any value of n greater than 0.

The next thing that's interesting is points that are eventually fixed and eventually periodic. For example, -1 is eventually fixed under f(x) = x<sup>2</sup>. This means, that the orbit will reach a fixed point and stay there. There are points that will hit a periodic point and then stay there.

This gives us some ground work. Then we take our first non linear function, the [logistic map](https://en.wikipedia.org/wiki/Logistic_map) and then use it to see how things behave. The function is simply


    f(x) = cx(1-x)

where `c` is a constant that we have to choose.

It's often used as a simple model for populations where the population (x) is expressed as fraction of the total number o f individuals that can exist in a fixed area. This means that x will vary between 0 and 1 and `c` is a constant that describes the environmental conditions of the situations.

Now, if we look at the orbits of various numbers under this function for different values of c, we will see this behaviour.

If `c` is 0.5 or 0.8 0 becomes a fixed point.
If `c` is 1.0, 0 is still a fixed point but it takes a lot of time for an orbit to reach there.
If `c` is 1.5, 1/3 is a fixed point
If `c` is 2, 1/2 is a fixed point
If `c` is 3.2, we will encounter a cycle of period 2.
If `c` is 3.5, we will hit a cycle of period 4.
If `c` is 3.55, we will hit a cycle of period 8.
If `c` is 4, the orbits of various numbers almost look like a series of random numbers.
If `c` is 5, all orbits seem to tend to infinity.

The [linear_plot.py](https://github.com/TheLycaeum/non-linear-functions/blob/master/linear_plot.py) program can be used to show the orbit of a point under a function on the number line. If, for example, one runs it like so `python linear_plot.py --function='3.5*x*(1-x)' --start 0.5` (which means iterate the point `0.5` under the function `3.5*x*(1-x)`. The resulting output looks something like this.

![Logistic function plot for c=3.5](/img/logistic-3.5.png)

Hovering on top of a gray dot will give you the value of the point. You can experiment with the various values of `c` which we're mentioned above to get an idea of how this works.

Then we went on to discuss graphic analysis of functions. This is a simple method to understand the behaviour of a function using just a first degree plot. The basic method is as follows. We draw the function in a two dimensional graph and draw a line across the graph intersecting the origin. This line will be a plot of function `f(x) = x`. Now, to get the orbit of a point `x` under a function `g`, we first mark x on the X axis and then move up or down to find `g(x)`. This gives us the point `(x, g(x))`. Now, we move left or right until we intersect the line `f(x)=x`. This intersection point will be `(g(x), g(x))`. We then move up or down to again intersect `g` so that we get `(g(x), g(g(x)))`. We can repeat this to see how functions behave. This has been done in in the [graph.py](https://github.com/TheLycaeum/non-linear-functions/blob/master/graph.py) program. An example output is shown below when we run it like so `python graph.py --seed=0.00001 --iters=1000 --winsize=700x700 --function='3.2*x*(1-x)' --start=0 --stop=1`.

![Graphical analysis for logistic function with c=3.2](/img/graph-3.2.png)

You can see the periodic orbit when you see the animation happen.

We then discussed how this behaviour can be plotted and analysed in various ways and wound up. I'll probably be doing a part two of the same presentation where we talk about bifurcations and bifurcation plots, move the iteration to the complex plane and then enter the world of fractals.

If you're interested in following my presentations, do stay in touch using one of the methods mentioned at http://thelycaeum.in/contact.html.

Also, I'm conducting a repeat of my mentoring course. If you want details or to sign up, please head on to the [courses](http://thelycaeum.in/courses.html) page. 
