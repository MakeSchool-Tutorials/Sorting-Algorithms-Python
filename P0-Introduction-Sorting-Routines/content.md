---
title: "Introduction to Sorting Routines"
slug: introduction-sorting-routines
---

# Sorting Routines

When programming with collections of data, one of the most common operations performed is _sorting_. Just think of all the ways in which you use sorting in everyday software:

- Emails listed by date received
- Music albums shown alphabetically by title
- Address book contacts sorted by last or first name
- Facebook posts arranged by most recent post date
- To do items displayed in priority order

In each of these examples and in countless more, some collection of data is sorted so as to provide the viewer with a more useful representation of that data. After all, the computer doesn't really care how data is sorted - it's all the same data. But when we get into questions of how that data should be rendered or otherwise operated upon, sorting becomes a crucial factor.

### Principles of Sorting

Luckily, sorting is an intuitive task for anyone past the age of 2 or 3. If given the set of numbers 3, 2, 8, 4, 10, it wouldn't take us much longer than a few seconds to sort them from lowest to highest. Easy peasy.

It is so easy, in fact, that we are likely unaware of exactly _how_ our brains can perform this task. Which is exactly the kind of awareness we need to develop in order to write a basic sorting algorithm. Without a clear understanding of the precise steps involved in sorting, we will not be able to write a program that can do this work for us.

> [action]
>
Consider how you would _encode_ the mental steps for sorting a small list of numbers into a series of specific instructions that you could recite over the phone to a friend who does not know how to sort anything. Write your instructions in plain English.

When you're finished you should have a list of instructions that looks something like this:

1. Find the lowest number in the list (we'll call this the _source_ list)
2. Put this number at the beginning of a new list (call this our _sorted_ list)
3. Find the next-lowest number in the source list
4. Put this number after the last number in the sorted list
5. Repeat steps 3 and 4 until all the numbers from the source list have been moved to the sorted list

Did you write something similar? If your instructions are more vague than the instructions listed above, try to be more specific.

We'll call this our "basic human sorting algorithm", and we can write a program that implements this algorithm. Let's use Python to create a function called `human_sort()` that takes a list of numbers and returns the numbers sorted (from smallest to largest) in a list. For example,

	source_nums = [3, 2, 8, 4, 10]
	sorted_nums = human_sort(source_nums)
	sorted_nums == [2, 3, 4, 8, 10] # => true

> [action]
>
Write a `human_sort()` function in Python that implements the "basic human sorting algorithm" you created. You are _not_ allowed to use any of Python's built-in sorting functions (what would the fun in that be?)

When you're done, take a look at the implementation below to see another example of how _pseudocode_ can be translated into real computer code.

> [solution]
>
	def human_sort(orig):
	    source = orig[:]  # create shallow copy
	    sorted = []       # init sorted array
	    source_len = len(source)
	    for _ in range(source_len):         # repeat for every elem
	        min_idx = 0
	        for j in range(len(source)):    # find min in source
	            compare = source[min_idx]   # elem to compare against
	            current = source[j]         # current elem in iteration
	            if current < compare:
	                min_idx = j
	        sorted.append(source[min_idx])  # add min to sorted
	        del source[min_idx]             # remove from source
	    return sorted

## Beyond the Basics

Congratulations! You've written a real, honest-to-goodness sorting algorithm. Granted, it isn't super efficient, but it is a start. Let's think about what a more advanced sorting algorithm would look like.

In the wild, sorting is rarely as simple as "sort these 5 integers from lowest to highest". To start with, the data we want to sort is often made of _compound values_, things like structs or dictionaries (or even more arrays!).

For example, say we have the following data set (truncated for readability) of flight information stored as a list of dictionaries:

	[
	  { 'origin': 'SFO', 'dest': 'JFK', 'duration': 5.5, 'price': '$249', 'airline': 'JetBlue' },
	  { 'origin': 'SFO', 'dest': 'LGA', 'duration': 7.75, 'price': '$201', 'airline': 'Spirit' },
	  { 'origin': 'OAK', 'dest': 'LGA', 'duration': 5.83, 'price': '$225', 'airline': 'Delta' },
	  { 'origin': 'OAK', 'dest': 'NWK', 'duration': 8.13, 'price': '$189', 'airline': 'United' },
	  { 'origin': 'OAK', 'dest': 'JFK', 'duration': 5.32, 'price': '$244', 'airline': 'Virgin America' }
	]

If you were given instructions to "sort this data", what would you do? Hopefully, you'd as for better instructions, because there is no single, straightforward way to "sort" this data!

In order to sort a data set like the one above, we need to know which attribute to sort _by_. We could _sort by_ duration (low-high or high-low), or we could sort by airline (alphabetical descending or ascending), or we could sort by price, or... you get the idea.

Here's an example interface for a `sort_by()` function that could sort the above list by duration (ascending lowest to highest):

	by_duration = sort_by(flights, 'duration', Order.asc)
	by_duration[0]['duration'] == 5.32 # => true

Or we could sort by destination (alphabetically, Z-A):

	by_dest = sort_by(flights, 'dest', Order.desc)
	by_dest[0]['dest'] == 'NWK' # => true

Note the use of `Order.asc` and `Order.desc` above. Those aren't built into Python, they are examples of an [enumeration](https://docs.python.org/3/library/enum.html). For the above code to work, we'd have to define an `Order` enumeration like this:

	from enum import Enum
	class Order(Enum):
	    asc = 1
	    desc = -1

The value you give to each enumeration member is up to you. In the example implementation below, only the names of each member are used.

> [action]
>
Write a `sort_by()` function following the examples above.
>
It should have three parameters:
- `list`: a list of dictionaries
- `property`: property to sort by
- `order`: a member of the `Order` enumerator (you must define this)
>
Make sure to test your function with different inputs to ensure that it works under all reasonable conditions!

When you've got a working version of `sort_by()`, compare your code to the example below.

> [solution]
>
	def sort_by(orig, property, order):
	    source = orig[:]  # create shallow copy
	    sorted = []       # init sorted array
	    source_len = len(source)
	    for _ in range(source_len):
	        idx = 0
	        for j in range(len(source)):
	            compare = source[idx][property] # elem to compare against
	            current = source[j][property]   # current elem in iteration
	            if order is Order.asc and current < compare:
	                idx = j
	            elif order is Order.desc and current > compare:
	                idx = j
	        sorted.append(source[idx])
	        del source[idx]
	    return sorted

Before moving on, there is one other case we must consider. What if we want to sort by a _computed value_, i.e. a value that is calculated from the data, but is not actually a part of the data?

A common example of this kind of problem is the proximity question. If I have a list of Indian restaurants with their geographic coordinates (latitude and longitude), how would I answer the question "find the 10 closest restaurants to my current location?" Assuming we have a latitude and longitude value for our current position, we would want to sort the restaurants by their distance (ascending) from our current location. Of course, the data set would not store distance from our present location (nor should it), it only stores the restaurant's coordinates. The distance must be computed.

In the flight data from above, we can find another use case for sorting computed values. How would we sort by price? The prices are stored as strings, but we'd want to sort them by their numeric value. So we would need to parse them first, and the sort them.

With the power of Python's lambdas, we could write a `sort_with()` function that looks like this:

	by_price = sort_with(flights, key=lambda flight: parse_price(flight['price']))
	by_price[0]['price'] == '$189' # => true

Assuming that we have a function `parse_price()` implemented like this:

	def parse_price(price):
	    without_dollar = price.replace('$', '')
	    price_as_float = float(without_dollar)
	    return price_as_float

> [action]
>
Write a `sort_with()` function that implements the interface shown above. It should take a list of items to sort and a `key` function to call on each item before sorting.

When you've written a working `sort_with()` function, compare your code to the example below.

> [solution]
>
	def sort_with(orig, key):
	    source = orig[:]  # create shallow copy
	    sorted = []       # init sorted array
	    source_len = len(source)
	    for _ in range(source_len):         # repeat for every elem
	        min_idx = 0
	        for j in range(len(source)):    # find min in source
	            compare = source[min_idx]   # elem to compare against
	            current = source[j]         # current elem in iteration
	            if key(current) < key(compare): # compare computed values
	                min_idx = j
	        sorted.append(source[min_idx])  # add min to sorted
	        del source[min_idx]             # remove from source
	    return sorted

## Varieties of Sorting Algorithms

When we encounter sortable data as software users, we usually expect it to be highly performant. If we visit a flight searching site like Hipmunk or Kayak, we expect to be able to choose different sorting options and have the view update rapidly and accurately. The same goes for any other list-based application with sorting capabilities: performance affects user experience.

Optimizing the performance of our sorting operations requires more advanced algorithms. Luckily, computers have been around for a few years now, and a lot of smart people have thought of highly efficient sorting algorithms.

As with so many things, though, there is no "one size fits all" solution. There are a whole family of different sorting algorithms, each with their own strengths and weaknesses.

> [action]
>
Explore the sorting algorithms listed on http://www.sorting-algorithms.com. Try to understand how each one works.
>
What are the optimal conditions for each? Which conditions produce the worst runtimes? Does your "basic human sorting algorithm" look like any of the examples listed here?

In the rest of this tutorial, we'll be implementing several of the most common sorting algorithms to better understand how they work and how they compare to each other. While you will likely not have to write your own sorting algorithm for production code (you will almost always want to use a library), knowing how to write and analyze them is a fundamental computer science skill.
