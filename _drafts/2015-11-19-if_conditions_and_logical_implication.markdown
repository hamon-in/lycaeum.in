---
layout: post
title: "if conditions and logical implication"
date: "2015-11-19 20:11:54 +0530"
---

During one of classes here, I came across an interesting concept which was somewhat hard to explain to the students. This post is as much an attempt to present the idea as it is an attempt to clear my own thoughts on the matter.

### Background

We were writing an implementation of [Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) and using this to explain TDD and some design principles. As is well known, the rules of life to move from one generation to the next are as follows.

1.    Any live cell with fewer than two live neighbours dies.
1.    Any live cell with two or three live neighbours survives.
1.    Any live cell with more than three live neighbours dies.
1.    Any dead cell with exactly three live neighbours becomes a live cell.

The way we went about this was to first write a few tests and functions to create a grid, to make a cell alive and to make a cell dead. After that, we were writing a function `run_generation` to implement the four rules above. We were doing this by writing a test for a rule and then updating the `run_generation` function to implement the rule. The first test looked something like this

{% highlight python %}

    def test_run_gen_rule1():
        """
        If a live cell has less than 2 live neighbours, it dies due to
        loneliness.
        """
        grid = [[1,0,0],  
                [0,0,0], 
                [0,0,0]]
        new_grid = life.run_generation(grid)
        assert new_grid[0][0] == 0

{% endhighlight %}

`0` implies a dead cell and `1` a live one.

This is simple enough. It asserts that given such a condition, the cell in the top left corner should die. The implementation of `run_generation` that made this pass was like so.

{% highlight python %}

    def run_generation(grid):
        new_grid = [[0,0,0], [0,0,0], [0,0,0]]
        return new_grid

{% endhighlight %}

So, this makes the test green and we marked it as such but here's where things get interesting. My claim was that it's not just a broken implementation making the test pass but that it's a complete implementation and which perfectly implements rule 1. And that's where the confusion started.

The questions that came up were things like "Where are we checking that it has fewer than 2 live neighbours?", "Won't it kill the cell even if it has 2 neighbours?" etc.

### Logical implications

There's a notation in classical logic called logical implication. The symbol used is a right arrow (→). It's often read as "implies" so `p → q` is "p implies q". It's also sometimes read as "if p, then q". What does this mean? Logical connective operators like `and`, `or` or `→` are often described using a [truth table](https://en.wikipedia.org/wiki/Truth_table). This is a list of all permutations of the inputs and the resulting outputs. So, a truth table for `a and b` would be something like this


|a    |b    |a and b|
|-----|-----|-------|
|True |True |True   |
|True |False|False  |
|False|True |False  |
|False|False|False  |

This tells us that if `a and b` is `False` in all cases except if both `a` and `b` are True. This is a precise and complete way of saying what `a and b` means. Now, consider the truth table of →.

|a    |b    |a → b|
|-----|-----|-----|
|True |True |True |
|True |False|False|
|False|True |True |
|False|False|True |

This tells us that that the only way in which `a → b` can be `False` is if `a` is `True` *and* `b` is `False`.

The expression is equivalent to `b or (not a)`. Meaning that either `b` should be `True` or `a` should be `False`. This means

  - If `b` is True, then we can be sure that the statement is `True` *regardless of the value of `a`*. 
  - If `a` is False, then we can be sure that the statement is `True` *regardless of the value of `b`*. We say that the statement in this case is [vacuously true](https://en.wikipedia.org/wiki/Vacuous_truth).

If we can guarantee either of these, we can be sure of our implementation. 

I'll stop here lest you accuse me of going all [Smullyan](https://en.wikipedia.org/wiki/Raymond_Smullyan) on you . 

### Our implementation

So, if we have a rule that's of the form `a → b`, we just have to make sure this truth table is satisfied. Now let's look at the rule we're trying to implement.

> Any live cell with fewer than two live neighbours dies.

or stated in the form of a logical implication,

> If a live cell has less than two live neighbours, it should die.

or better

> "a live cell has less than two neighbours" → "it dies".

Now, we're going to try to implement this. A direct translation of the rule would be something like

{% highlight python %}

    if isalive(cell) and neighbours(cell) < 2:
       kill(cell)

{% endhighlight %}

But this is a logical implication `a → b` where `a` is "a live cell has less than two neighbours" and `b` is "it dies".

We can implement `a → b` by just making sure that `b` is `True`. Then, as per the truth table, the statement will be true (ie. our code will be correct) regardless of the value of `a`.

How do we do that? We simply `kill(cell)` and since we start with all cells set to dead (`0`), this is `True`.

{% highlight python %}

    def run_generation(grid):
        new_grid = [[0,0,0], [0,0,0], [0,0,0]]
        return new_grid

{% endhighlight %}


Let's look at this implementation. Can it ever violate the truth table? Well, the only way in which it can is we let `b` become `False` even if `a` is `True`. But, we set everything to dead explicitly so `b` is never `False` hence the final result is always `True`.

## Questions

So, let's answer some questions.

- Where are we checking that it has fewer than 2 live neighbours?
*We don't need to. If `b` is `True`, then the statement is `True` regardless of the value of `a`.*

- Won't it kill the cell even if it has 2 neighbours?
*That's a situation where `a` is `False` and `b` is `True`. If `a` is `False`, then the statement is `True`. This is because we care only about the result **if** `a` is `True`. Otherwise, `b` can be anything.*

- You're just ignoring the if part and making this unconditional. You can't do that to every if condition.
*Actually, I can. If this were the only rule, it would be enough but there are other rules too.*



