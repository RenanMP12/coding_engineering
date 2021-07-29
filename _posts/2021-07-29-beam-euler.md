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
Next we write the number of elements we want to simulate our problem and pre-allocate the matrices required like stiffness matrix and external load vector. This part of the code we write as
```python

nel    = 4                         # number of elements
nnos   = nel + 1                   # number of nodes
alldof = np.linspace(1,1,2*nnos)   # all degrees of freedom
kg     = np.zeros((2*nnos,2*nnos)) # global stiffness matrix pre-allocation
f = np.zeros((2*nnos, 1))          # external load vector
u = np.zeros((2*nnos, 1))          # displacement vector
coord = np.zeros((nnos, 3))        # coordinate matrix pre-allocation
inci = np.zeros((nel, 6))          # incidence matrix pre-allocation
mask  = np.zeros((2*nnos,2*nnos))  # mask stiffness matrix
maskv = maskv = np.zeros(2*nnos)   # mask load vector

```
# Fourth step:
The following step in our code is set the coordinate and incidence matrices. The first matrix tells us the position of each node in the Cartesian coordinates, and the second matrix tells us the composition of each element (which nodes are attached to each element, **considering a two node element**). One way to write it in Python is
```python

# Coordinate matrix
for i in range(0, nnos):
    coord[i, 0] = i + 1       # node number
    coord[i, 1] = i/L*nel     # node position in X axis
l = coord[1, 1] - coord[0, 1] # element length

# Incidence matrix
for in range(0, nel):
    inci[i,0] = i + 1         # element number
    inci[i,1] = i + 1         # first node
    inci[i,1] = i + 2         # second node
```

# Fifth step:
Next we have to set the boundary conditions of our problem and how to eliminate the rows and columns of the constrained degrees of freedom. In our problem we know that the left side of the beam is clamped and the right side is simply supported. One way to write a matrix with the boundary conditions is
```python

# bc = [node| degree of freedom | value]

```
As we know the Euler-Bernoulli beam has two degrees of freedom (DOF) for each node, where the first DOF is related to the displacement in Y direction and the second DOF is equal to a rotation in Z direction. When the value is set to 0 it means that this DOF is constrained. In our example we can write our code as
```python

bc = np.array([[1, 1, 0],
               [1, 2, 0],
               [nnos, 1, 0]])
```
Now we know which rows and columns shall be removed from the equation system [Kg]{u} = {fg}, we can define the variables *mask* and *maskv* pre-allocated in the third step. The matrix *mask* is the same size of the matrix [Kg] and the vector *maskv* is the same length from the vector {fg}. First we define mask as 
```python

for i in range(0, np.size(bc, 0)):
    if bc[i, 1] == 1:
        mask[2*bc[i, 0] - 2, 2*bc[i, 0] - 2] = 1
    elif bc[i, 1] == 2:
        mask[2*bc[i, 0] - 1, 2*bc[i, 0] - 1] = 1
mask = ma.masked_equal(mask, 1)
mask = ma.mask_rowcols(mask)
mask = (mask == False)

```

If we type **mask.data** the output should be the same presented in the Figure below. The first, second and ninth rows and columns are defined as *False*. 
