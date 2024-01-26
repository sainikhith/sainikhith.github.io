---
layout: post
title: Calculating Planet Volumes with Numpy
description: In this post I'm going to explore using Python and Numpy to quickly calculate all the planet volumes in the solar system!
image: "/posts/planets.jpg"
tags: [Python, Numpy, Planets]
---

## introduction
In this post I'm going to explore using Python and Numpy to quickly calculate all the planet volumes in the solar system!  If that works out, I'll try to see how fast it can calculate the volumes of a million fictional planets!

I've created this project to start to explore the use of the Numpy package within Python.  Numpy is a Python library for working with numerical and mathematical data. Here are some key things to know about Numpy:

* Numpy provides an N-dimensional array object called ndarray that allows you to perform vectorized operations on data very efficiently. Arrays in Numpy are fast and memory efficient compared to native Python lists.
* Numpy comes with a large library of high-level mathematical functions that operate on ndarray objects, such as methods for linear algebra, Fourier transforms, statistics, and more. This makes it easy to do scientific computing in Python.
* Numpy arrays have a dtype (data type) that describes the kind of elements it contains, such as floating point numbers, integers, booleans, etc. The dtype allows Numpy to allocate minimal memory and enables fast computations on the array.
* Numpy provides powerful indexing and slicing capabilities for accessing and modifying array data. You can slice Numpy arrays much like Python lists, but assignments also work in-place for fast operations.
* Numpy integrates well with other scientific Python libraries like SciPy, Matplotlib, Pandas, and Scikit-Learn, making it a core foundation for many data science and scientific applications in Python.  We will probably look at some of these in future posts.

So in summary, Numpy is the core library for numerical data and high-performance multi-dimensional array computing in Python. It provides key data structures, array operations, and integrates with other libraries. Numpy is a fundamental package for scientific computing, data analysis, and machine learning in Python.  I'm only really scratching the surface of what Numpy can do here, but you have to start somewhere. 

---
### the planets of the solar system
This is a pretty simple mathematical problem to calculate the volume of a planet if you know the radius, but the reason we've chosen it, is that we want to explore how numpy can do a lot of calculations all at once using it's special data structures, and how quickly it can do them. So we start with a quick Google to grab the radii of all our planets. 

| Planet | Radius (km) |  
|:---|---:|
| Mercury | 2439.7 | 
| Venus | 6051.8 | 
| Earth | 6371 |
| Mars | 3389.7 |
| Jupiter | 69911 |
| Saturn | 58232 |
| Uranus | 25362 |
| Neptune | 24622 |

---
## design
We want to use a Numpy array to hold all the radii, so that we can simultaneously calculate the volumes of all the planets at once.  We'll use the time package as well, to see how long the calculation takes, since parts of this package are written in c under the hood, so it should be fast. 

So, we have our calculation formula in python for the volume of a planet.  We're importing the numpy package, and using it's handy value for pi (&pi;).  
```ruby
import numpy as np

# volume calculation
r = 10
volume = 4/3 * np.pi * r**3
print(volume)

```

We could create a loop and work through an array of radii, to calculate each planet volume like that, but there's a more interesting way to do this with Numpy, and we can calculate them all at once!  Let's see how that works!  We basically create a numpy array of all the radii, and then use that array in our formula!

We'll put a time capture at the start and end of the calculation so we can try and see how fast it is.

---
## test
```ruby
import numpy as np
import time

# array of radii of each planet
radii = np.array([2439.7, 6051.8, 6371, 3389.7, 69911, 58232, 25362, 24622])

start = time.time()
volumes = 4/3 * np.pi * radii**3
print(volumes)
end = time.time()
print(f"calculated in {end - start} seconds")

```

Let's see what happens!

```ruby
>>> [6.08272087e+10 9.28415346e+11 1.08320692e+12 1.63144486e+11
>>> 1.43128181e+15 8.27129915e+14 6.83343557e+13 6.25257040e+13]
>>> calculated in 0.0 seconds
```
Wow! here are all our planets with their volumes, calculated in a tiny fraction of a second, so small we couldn't even measure it.  That's really quite cool.

| Planet | Radius (km) | Volume (km<sup>2</sup>) | 
|:---|---:|---:|
| Mercury | 2439.7 | 6.08272087e+10 |
| Venus | 6051.8 | 9.28415346e+11 |
| Earth | 6371 | 1.08320692e+12 |
| Mars | 3389.7 | 1.63144486e+11 |
| Jupiter | 69911 | 1.43128181e+15 |
| Saturn | 58232 | 8.27129915e+14 |
| Uranus | 25362 | 6.83343557e+13 |
| Neptune | 24622 | 6.25257040e+13 |



Let's give it something a bit more challenging.

Let's now try calling the function with a ficticious solar system full of a million randomly sized planets!
We'll use the Numpy random function to create a random array of different radii for our fictional planets.

```ruby
# 1,000,000 fictional planets!
radii2 = np.random.randint(1, 100000, 1000000)

start = time.time()
volumes = 4/3 * np.pi * radii2**3
print(volumes)
end = time.time()
print(f"calculated in {end - start} seconds")
print(f"length of volumes array is {len(volumes):,d}")

```
So let's see what happens when we run it!

```ruby

>>> [2.48384410e+07 1.39847989e+08 3.95453689e+09 ... 2.22609486e+09
>>> 7.42069736e+06 6.04399282e+06]
>>> calculated in 0.004191875457763672 seconds

calculated in 0.004191875457763672 seconds
length of volumes array is 1,000,000

```
Wow! It just did a million calculations in 0.004 seconds. That was pretty quick! And the resulting array really does have a million entries!

The fact that Numpy arrays have a fixed data type, rather than being able to contain mixed types of objects like Python lists, is one of the reasons it can perform calculations on an entire array virtually simultaneously.  Also, much of the mathematical calculations are done in optimised C code under the hood, and various other languages, with some parts even written in Assembly language.  There is a handy Python API within the Numpy package that allows it to be seamlessly accessed from Python code. 

---
#### random dad joke alert

```ruby
# Q. What kind of music do planets like to listen to? 

# A. Nep-tunes!
```

I hope you've enjoyed this little exploration into Planets, Python and Numpy.  Rest assured we will be exploring Numpy functionality further in later posts.
