---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/primes_image.jpeg"
tags: [Python, Primes]
---

## introduction
In this post I'm going to run through creating a function in Python that can quickly find all the Prime numbers below a given value.  For example, if I passed the function a value of 100, it would find all the prime numbers below 100.

I've created this project to demonstrate the use of some basic Python functionality in a simple mathematical problem.  There are a few good reasons for using Python to solve mathematical problems like this:  

* Python has simple and clean syntax that makes it easy to write and understand code. This is important for a math-heavy algorithm like prime number calculation.
* It has built-in big number support through the 'decimal' module, which allows me to work with large numbers without worry of overflow. This is crucial for prime calculations.
* Python has libraries that provide highly optimised functions and data structures for numerical computing, which can really speed up the processing.
* It's easy to write Python code in a functional style, with higher-order functions like 'filter', 'map', etc. This lends itself well to a prime checking algorithm.
* Python code can be written and tested interactively using the REPL. That would allow me to prototype and debug each part of the algorithm easily.
* Python can later be compiled to native machine code using tools like Cython or Numba if I need to optimize for speed. So I can start simple and optimize later if needed.

Of course, many other languages like C, Rust, Julia etc would also work. However, I find Python strikes a nice balance between simplicity, available libraries, and performance for this type of maths-focused code. The main thing is using a language that makes the algorithm clear and maintainable.

---
### what is a prime number?
A prime number is a whole number greater than 1 that can only be divided evenly by 1 and itself. By evenly, I mean to produce a whole number. For example, 5 is a prime number because it can only be divided evenly by 1 and 5. It cannot be divided evenly by any other whole numbers.

Some key things about prime numbers:

* They have exactly 2 factors - 1 and the number itself. For example, the only factors of 5 are 1 and 5.
2 is the only even prime number. All other prime numbers are odd.
* There is no highest prime number. We can keep finding larger and larger prime numbers forever.
* Prime numbers get more spread out as the numbers get larger. For small numbers there are lots of primes, but for very big numbers you have to look farther to find the next prime.
* Prime numbers are important in many areas of maths and encryption. But at a basic level, they are just numbers that can only be evenly divided by themselves and 1.

So in simple terms, a prime number is a lone wolf - it can't be evenly split into smaller whole number groups. It's only divisible by itself and 1. This makes primes the atoms of the math world. 

---
#### random dad joke alert

```ruby
# Q. What do prime numbers and stoners have in common?  

# A. They get more spaced out the higher they get...
```


Ok, Let's get into our design.

---
## design
First let's think about the variables and data types we want to use.  We need to start with an integer to hold the limit we are checking up to.  Let's call that primes_limit.

```ruby
primes_limit = 20
```

We know the smallest true Prime number is 2.  We now need a list of all numbers that we need to check which will be every integer between 2 and our upper bound which in this case was 20. We use *primes_limit + 1* as the range logic we want to use for this is not inclusive of the upper limit.

Instead of using a list, though, we're going to try to use a set.  The reason for this is that sets have some special functions that will allow us to eliminate non-primes during our search, and massively speed it up. We are going to look at how fast we can calculate our primes. 

```ruby
range_limit = primes_limit+1
numbers_to_check = set(range(2, range_limit))
```

We also need a place where we can store any primes we discover.  A list will be fine for this job.

```ruby
primes_list = []
```

We're going to use a while loop to iterate through our set and check for primes, but before we construct that I find it can be valuable to think through the logic first.  

So, we have our set of numbers (called numbers_to_check to check all integers between 2 and 20. Let's extract the first number from that set that we want to check as to whether it's a prime. When we check the value we're going to check if it is a prime...if it is, we're going to add it to our list called primes_list...if it isn't a prime we don't want to keep it.

There is a method which will remove an element from a list or set and provide that value to us, and that method is called *pop*

```ruby
print(numbers_to_check)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```
If we use pop, and assign this to the object called **prime** it will *pop* the first element from the set out of **numbers_to_check**, and into **prime**

```ruby
prime = numbers_to_check.pop()
print(prime)
>>> 2
print(numbers_to_check)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

Now, we know that the very first value in our range is going to be a prime, as there is nothing smaller than it so therefore nothing else could possible divide evenly into it.  As we know it's a prime, let's add it to our list of primes.

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

Now we're going to do a special trick using set functionality to check our remaining numbers_to_check for non-primes. For the prime number we just checked (in this first case it was the number 2) we want to generate all the multiples of that up to our upper range (in our case, 20).

We're going to use a set rather than a list, because it allows us some special functionality that we'll use in a minute, which is the magic of this approach.

```ruby
multiples = set(range(prime*2, primes_limit+1, prime))
```

Remember that when creating a range the syntax is range(start, stop, step). For the starting point - we don't need our number as that has already been added as a prime, so let's start our range of multiples at 2 * our number as that is the first multiple, in our case, our number is 2 so the first multiple will be 4. If the number we were checking was 3 then the first multiple would be 6 - and so on.

For the stopping point of our range - we specify that we want our range to go up to 20, so we use **primes_limit+1** to specify that we want 20 to be included.

Now, the **step** is key here.  We want multiples of our number, so we want to increment in steps *of our* number so we can put in **prime** here

Lets have a look at our list of multiples...

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

The next part is the magic I spoke about earlier, we're using the special set functionality **difference_update** which removes any values from our number range that are multiples of the number we just checked. The reason we're doing this is because if a number is a multiple of anything other than 1 or itself then it is **not a prime number** and can remove it from the list to be checked.  Hopefully this will give us a much faster process than checking each number in the original list!

Before we apply the **difference_update**, let's look at our two sets.

```ruby
print(numbers_to_check)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

**difference_update** works in a way that will update one set to only include the values that are *different* from those in a second set

To use this, we put our initial set and then apply the difference update with our multiples:

```ruby
numbers_to_check.difference_update(multiples)
print(numbers_to_check)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```

When we look at our number range now, all values that were also present in the multiples set have been removed as we *know* they were not primes.

This is very cool!  We've made a massive reduction to the pool of numbers that need to be tested so this is really efficient. It also means the smallest number in our range *is definitely a prime number* as we know nothing smaller than it divides into it, and this means we can run all that logic again in a loop until we run out of numbers to check.

---
## solution version 1
Here is the code version 1, with a while loop doing the hard work of updated the number list and extracting primes until the list is empty.
we are also going to use the time functionality in python to time our function on various limits to see how fast our algorithm is!

```ruby
import time

# we'll create a function that accepts the primes limit as the upper bound of primes to find.  We will give it a default of 20.
def find_primes_under(primes_limit=20):
    start = time.time()
    # range limit, which we need because the range function we use in a minute excludes the upper bound, and we want to be inclusive.
    range_limit = primes_limit+1

    # number range to be checked, between 2, which we know is the first true prime, and the limit requested.
    numbers_to_check = set(range(2, range_limit))

    # primes that we have found go in here.
    primes_list = []

    # while there are still numbers left in the numbers_to_check set, we go round the loop
    while numbers_to_check:
        # we know the lowest number from the numbers to check set is a prime
        prime = numbers_to_check.pop()
        primes_list.append(prime)

        # range(start, stop, step) - we use this nice feature to find multiples of our prime
        # and remove them from our set of numbers remaining to check
        multiples = set(range(prime*2, range_limit, prime))
        numbers_to_check.difference_update(multiples)

    # We calculate some summary details to report, before returning the list of primes that we found.
    # We also report the time the calculation took.
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    end = time.time()
    print(
        f"There are {prime_count} prime numbers between 0 and {primes_limit}.")
    print(f"The largest prime is {largest_prime}.")
    print(f"The calculation took {end - start} seconds.")

    return primes_list
```
---
# testing version 1
Let's call the function and print the primes_list to have a look at what we found!

```ruby
primes_list = find_primes_under()
print(primes_list)
```

So this is what we get.

```ruby
There are 8 prime numbers between 0 and 20.
The largest prime is 19.
The calculation took 0.0 seconds.

[2, 3, 5, 7, 11, 13, 17, 19]
```

Let's now try calling the function with some bigger numbers!

```ruby
primes_list = find_primes_under(100)
print(primes_list)

There are 25 prime numbers between 0 and 100.
The largest prime is 97.
The calculation took 0.0 seconds.
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]

```
Still pretty quick! Let's give it a bit more of a challenge, and try to find primes up to 100000

```ruby
primes_list = find_primes_under(100000)

There are 9592 prime numbers between 0 and 100000.
The largest prime is 99991.
The calculation took 0.03126120567321777 seconds.

```
Awesome! It found 99991 prime numbers between 2 and 100,000 in a tiny fraction of a second! 
Let's go for something really large, say a million...

```ruby
find_primes_under(1000000)

There are 78498 prime numbers between 0 and 1000000.
The largest prime is 999983.
The calculation took 0.3613283634185791 seconds.
```

That is pretty cool, and super quick!  

---
## houston, we have a problem
But wait!  While this is all working perfectly on my machine, and passed every test, we actually have a problem with the python code, and this is one that could cause another developer, or future-me a massive headache!  

*The Symptoms*
We have reports that, while most of the time everything works perfectly, occasionally people are getting the wrong results!  Prime numbers are now and again being missed, and sometimes the list of returned 'Primes' contains the odd rogue number which is not a prime at all! What is going on?

I try, but cannot replicate it on my machine. I run tests, I debug through the code. It's all working great for me!  So to solve this intermittent problem we need to look again at the code in our function, and question our assumptions.

---

### using pop() on a set in python

In the real world - we can't rely on using the pop() method on a Set because pop() will simply take the first item in the set, and a set in python is un-ordered by definition.  That means, pop() could randomly select any value from the set.  In practice, at least on my machine with my setup, it is usually extracting the lowest element of the set.  However, we cannot rely on this.  

The items are stored internally with some order, but this internal order is determined by the hash code of the key (which is what allows retrieval to be so fast). 

This hashing method means that we can't 100% rely on it successfully getting the lowest value. In very rare cases, the hash provides a value that is not the lowest.  And this usually results in a rogue non-prime value being added to our list of primes!

Even though here, we're just coding up something for fun - it is most definitely a useful thing to note when using Sets and pop() in Python in the future!  In fact this applies to other functions that for whatever reason are allowed on Sets, but they do not work reliably because of the unordered nature of them.  

These include min() and max(), and this I feel is worse than the pop() situation, since the name of these two functions implies that they will find the minimum or maximum value of a Set, and Python allows you to call them on a Set, but they suffer from exactly the same problem!  The min function will not always find you the minimum value, and the max function will not always find you the maximum value of an un-ordered set!

The simplest solution to force the minimum value to be used is to replace the line...

```ruby
prime = numbers_to_check.pop()
```

...with the lines...

```ruby
prime = min(sorted(numbers_to_check))
numbers_to_check.remove(prime)
```
## solution version 2
Here is the new function:

```ruby
def find_primes_under(primes_limit=20):
    start = time.time()
    # upper bound
    range_limit = primes_limit + 1

    # number range to be checked
    numbers_to_check_set = set(range(2, range_limit))
    numbers_to_check_list = list(numbers_to_check_set)

    # primes that we have found
    primes_list = []

    # while there are still items in the numbers_to_check set
    while numbers_to_check_set:
        # we know the lowest number from the numbers to check set is a prime
        # even though we can call min on a set, we need to sort it first before it will reliably give us the minimum!!!
        prime = min(sorted(numbers_to_check_list))
        numbers_to_check_set.remove(prime)
        primes_list.append(prime)

        # range(start, stop, step) - we use this to find multiples of our prime
        multiples = set(range(prime*2, range_limit, prime))

        # here we remove them from our set of numbers remaining to check
        numbers_to_check_set.difference_update(multiples)
        # and now we need to update the list so we can reliably find the min
        numbers_to_check_list = list(numbers_to_check_set)

    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    end = time.time()
    print(
        f"There are {prime_count} prime numbers between 0 and {primes_limit}.")
    print(f"The largest prime is {largest_prime}.")
    print(f"The calculation took {end - start} seconds.")

    return primes_list
```

When we make this change, we see a massive decrease in performance.  Our test for 100,000 now takes over 5 seconds!

```ruby
There are 9592 prime numbers between 0 and 100000.
The largest prime is 99991.
The calculation took 5.072026491165161 seconds.
```

However, because we have to sort the list for each iteration of the loop in order to get the minimum value, it's a lot slower than what we saw with pop()!

## solution version 3
After further research we find there is such a thing as an OrderedSet in the ordered_set package.  You have to install it with pip install ordered_set before you can use it. 

Let's try this and see what happens.  Here is the code:

```ruby
import time
from ordered_set import OrderedSet

def find_primes_under(primes_limit=20):
    start = time.time()
    # upper bound
    range_limit = primes_limit + 1

    # number range to be checked
    numbers_to_check = OrderedSet(range(2, range_limit))

    # primes that we have found
    primes_list = []

    # while there are still items in the numbers_to_check set
    while numbers_to_check:
        # Using an ordered set we should be able to pop them.
        prime = numbers_to_check.pop(0)
        primes_list.append(prime)

        # range(start, stop, step) - we use this to find multiples of our prime
        multiples = set(range(prime*2, range_limit, prime))

        # here we remove them from our set of numbers remaining to check
        numbers_to_check.difference_update(multiples)

    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    end = time.time()
    print(
        f"There are {prime_count} prime numbers between 0 and {primes_limit}.")
    print(f"The largest prime is {largest_prime}.")
    print(f"The calculation took {end - start} seconds.")

    return primes_list

```

Unfortunately, although using this new type lets us go back to the old pop approach, it was still no quicker than using the sorted option above, in my tests.  In fact it was slower!  7 seconds for 100,000 primes.  Not impressed.

```ruby
primes_list = find_primes_under(100000)

There are 9592 prime numbers between 0 and 100000.
The largest prime is 99991.
The calculation took 7.056451082229614 seconds.
```

## solution version 4
So, lets go back to the drawing board, and to the simple built in types, and think through the logic again.  There's got to be a simple solution using what we know.

We want the ordered nature of a list, and we want to be able to quickly eliminate multiples of our primes using the set functionality.  So, let's try using both types, and use the right tool for each job.

```ruby
def find_primes_under(primes_limit=20):
    start = time.time()
    # upper bound
    range_limit = primes_limit + 1

    # number range to be checked - we need both a list and a set for
    # this experiment
    numbers_to_check_list = list(range(2, range_limit))
    numbers_to_check_excluding_multiples_set = set(numbers_to_check_list)

    # primes that we have found
    primes_list = []

    # while there are still items in the numbers_to_check set
    while numbers_to_check_excluding_multiples_set:
        # we know the lowest number from the numbers to check set is a prime
        # so we pop the first number from the list, and then check if we
        # have eliminated it from our set check
        prime = numbers_to_check_list.pop(0)
        if prime not in numbers_to_check_excluding_multiples_set:
            # we don't need to check this, it's been excluded already
            continue
        else:
            primes_list.append(prime)
            numbers_to_check_excluding_multiples_set.remove(prime)

        # range(start, stop, step) - we use this to find multiples of our prime
        # within the specified range
        multiples = set(range(prime*2, range_limit, prime))

        # here we remove them from our set of numbers remaining to check
        numbers_to_check_excluding_multiples_set.difference_update(multiples)

    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    end = time.time()
    print(
        f"There are {prime_count} prime numbers between 0 and {primes_limit}.")
    print(f"The largest prime is {largest_prime}.")
    print(f"The calculation took {end - start} seconds.")

    return primes_list
```


```ruby
primes_list = find_primes_under(100000)

There are 9592 prime numbers between 0 and 100000.
The largest prime is 99991.
The calculation took 0.9255471229553223 seconds.

primes_list = find_primes_under(1000000)

There are 78498 prime numbers between 0 and 1000000.
The largest prime is 999983.
The calculation took 120.53264737129211 seconds.
```

Ok, I think that's probably as good as it's going to get, for tonight at least!  100,000 primes in just under a second, and a million primes in 2 minutes, and no unreliable behaviour!

A million primes is still a bit slow at a couple of minutes! and it's nothing like the sub-second performance results of our original code where were just popping off the set each time and hoping it would pick the right one.  But you can't have everything in software.  

I hope you've enjoyed this little exploration into Python.  