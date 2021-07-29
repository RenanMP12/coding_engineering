---
layout: post
title: "Programming a 2D Euler–Bernoulli beam with Python"
---

# First step:

First step in our code is import all libraries we need. In this code three libraries are required: *numpy* to compute the beam stiffness matrices, load and displacement vectors, respectively, *numpy mask* to set the boundary conditions of our problem and *matplotlib* to plot our result, a.k.a post-processing. In order to import these libraries we write the following commands
```python

# Libraries
import numpy as np                
import numpy.ma as ma
import matplotlib.pyplot as plt

```

# Second step:

The following step is writing the properties of our beam like length, load amplitude, Young's modulus and so on. In this problem the beam length is 1 meter, the load amplitude is 100 Newtons/meter, the Moment of Inertia has 100 kg.m², and the Elastic Modulus worths 210 GPa. We write these information in our code as
```python

# Beam properties
 L = 1             # beam length in meters
 q = 100           # load in newtons/meter
 I = 100           # moment of inertia in kg.m2
 E = 210e9         # young's modulus 

```
# Third step:

# Fourth step:

