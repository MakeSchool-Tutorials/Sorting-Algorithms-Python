---
title: "Sorting Real World Data"
slug: sorting-real-world-data
---

# Sorting Real World Data

In this part of the tutorial, we'll be implementing different sorting algorithms to improve a web app for displaying San Francisco civic data sets.

After you've written a few different ways to sort data, you'll compare and analyze their runtime performance to learn more about how, when, and why to use different types of sorting algorithms.

Much of the starter code (the core web app) is already written for you. Your job is to grok (i.e. understand) the existing code and then improve upon it by adding more sorting algorithms.

> [action]
>
Download and setup the starter code: TODO: LINK TO REPO
>
Follow the "Getting Started" instructions in the README to get the app up and running. Once you have it working, explore the codebase to see how it works.

Now that we have the starter code up and running, we can start working on our sorting algorithms.

If you open the `sort.py` file, you'll notice that the `timed_sort()` function accepts an `algorithm` argument for specifying which sorting algorithm to use. At the moment, only one algorithm is available (insertion sort), and so it is set as the default.

### Build Your Own Sorting Routines

Now for the fun part: we're going to build our own implementations of sorting algorithms!

Each sorting function will have the same interface: it will accept a list as its first argument, and a `key` function as its second argument. The `key` function is used to perform some operation on each element in the list before comparison. See the `insertion()` function for an example.

> [action]
>
Implement 5 common sorting algorithms:
>
- bubble
- selection
- heap
- quick
- merge
>
Make sure to rigorously test your implementations to ensure that they work as expected!

> [info]
>
There are lots and lots of resources on sorting algorithms on the web. The [Wikipedia article](https://en.wikipedia.org/wiki/Sorting_algorithm) has a good overview and the site [http://www.sorting-algorithms.com/](http://www.sorting-algorithms.com/) includes helpful animations and brief summaries.

### Use Your Routines in the Project Code

Once you have some (well-tested) sorting algorithms written, it is time to make use of them.

After all, a piece of code is only as valuable as its application.

> [action]
>
Instead of using the `insertion()` sort function in `timed_sort()` by default, use one of your own implementations! Try each one in turn and check the runtimes.

It would be nice if the UI (user interface) allowed users to choose a sorting algorithm along with the column to sort by.

> [action]
>
Add an element to the sorting form to let users select an algorithm to sort by.
>
It can be a simple drop-down `<select>` menu with each algorithm as an `<option>.`
>
In the appropriate route handler function in `app.py`, use the value from the form to tell `timed_sort()` which algorithm to use.

### Fix a Subtle Bug in Sorting Numeric Values

There is a bug in this code. [GASP!] Actually, this should come as no surprise. There are bugs in most code.

In the web app provided, however, there is a subtle bug that makes at least two of the sorting options malfunction.

If you haven't already stumbled upon it, take a closer look at the "Salary Ranges" data set and try sorting by the different column options. Compare what you'd expect to see with the actual results.

> [action]
>
Go through the debugging steps to resolve this issue:
>
- What is the bug?
- Why is it occurring?
- Where in the code is it?
- What would the output look like if the bug were fixed?
- How can you fix the bug?

If you are stumped and can't see it, take a peek at the solution below.

> [solution]
>
The bug is that the "Biweekly High Rate" and "Biweekly Low Rate" are being sorted by their _string_ value, not by their _numeric_ value.
>
In other words, `$1000.00` is listed as being lower than `$40.00` because the string `"4"` is greater than the string `"1"` (in the same way that `"K"` is greater than `"B"`).
>
To fix this bug, use the technique from the previous lesson: make sure that values for this sort are appropriately parsed before being compared.

### Compare and Analyze Performance

With a small library of sorting algorithms to choose from and some decent-sized data sets to experiment with, the final step is to analyze and compare the different algorithms.

Being able to implement an algorithm is only half of the skill. Just as important is knowing how to test and analyze different algorithms so that you can ensure that they work as expected.

With that knowledge you'll be able to know not just _how_ a particular algorithm works, but you'll also learn _why_ and _when_ to use one algorithm or another.

> [action]
>
Pick two of the data sets and a field to sort by on each. Then, sort by those fields with each of the algorithms you implemented. (Make sure that each sorting operation starts with the same initial state in the unsorted data).
>
For each test of the algorithms, record their runtime. Better yet, record the average runtime of at least 3 different executions.
>
Using the results of your experiments, answer the following questions:
>
- Which algorithm is the fastest?
- Do the runtimes recorded correspond to the Big-O complexity of each algorithm?
- What is the initial state of the data and how does that affect the outcomes?

## Where To Go From Here

Once you have completed all of the main goals of the tutorial, use some of these ideas to expand the project and explore the concepts further:

- Calculate shortest distance. 2 of the datasets include geographic coordinates for each record (latitude and longitude). Let users sort the records by proximity from a given location (i.e. closest records come first). This will require some geometry!
- Pick another sorting algorithm (or 2, or 5) and implement it. Then analyze its performance against the others: what are its strengths and weaknesses?
- Add advanced metrics reporting. Collect and display more data about each sort operation, such as: number of comparisons & number of swaps.
- Find another dataset (or 2, or 5) and add it to the app. What happens if you add a much larger dataset, like 10k+ rows? How do the different algorithms handle the added load?
