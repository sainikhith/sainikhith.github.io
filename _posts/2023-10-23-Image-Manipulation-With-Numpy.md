---
layout: post
title: Image Manipulation with Numpy
description: In this post I'm going to explore using Python and Numpy to quickly calculate all the planet volumes in the solar system!
image: "/posts/camaro_rainbow_h.jpg"
tags: [Python, Numpy, Images]
---

## introduction
In this post I'm going to explore using Python and Numpy only to manipulate a jpg image.   I'll be using skimage to import the image, and matplotlib as well, to render it so I can see what's happening.  I'll be using the typical naming conventions for these packages, to make it easier for people familiar with them to read the code.

---
Let's start with our basic image file which is a jpeg.

![Image of a yellow camaro car](/img/posts/camaro.jpg)

First thing we need to do is import the jpg image.  For this we will use the skimage function imread. Then we'll have a look at the shape of the image within python.  And as we will see, it is just a Numpy array!

```ruby
import numpy as np
from skimage import io
import matplotlib.pyplot as plt

camaro = io.imread("camaro.jpg")
print(camaro.shape)


>>> (1200, 1600, 3)
```




I hope you've enjoyed this investigation of image manipulation using just the Numpy package and Python.  It's pretty impressive what you can do with just a bit of maths!
