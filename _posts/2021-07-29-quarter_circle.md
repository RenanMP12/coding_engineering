---
layout: post
title: "Computing properties of a quarter circle"
---

This is an example of how to compute some properties of a circular section like area, inertia and its centroid, using finite element method.

<img src="{{ site.github.url }}/assets/img/circular_geometry.png" alt="Home" width="50%">

# First step
The first step when programming with Python is always import the libraries we will use. In this example two libraries are required
```python

# Libraries
import numpy as np
import matplotlib.pyplot as plt

```

# Second step

Next we define the number of nodes to approximate the circular section and the coordinate matrix, where we store the number of each node and its position in the Cartesian coordinates. In this matrix the first node will be always at the origin, while the others will be equally distributed on the perimeter of the quarter circle. This code can be written as

```python

nnos = 11                  # number of nodes
nel  = nnos - 2            # number of elements
coord = np.zeros((nnos,3))  # coordinate matrix pre-allocation
coord[0][0] = 1    
theta = (np.pi/2)/(nnos - 2) # theta value   
for i in range(0, nnos - 1):
   coord[i + 1][0] = i + 2          # Number of node
   coord[i + 1][1] = np.cos(theta*i) # X coordinate
   coord[i + 1][2] = np.sin(theta*i) # Y coordinate

# Scatter 
fig = plt.figure()
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.scatter(coord[:,1], coord[:,2], marker = '+', c = 'black')
plt.xlabel('x', fontsize = 'xx-large')
plt.ylabel('y', fontsize = 'xx-large')

```

<img src="{{ site.github.url }}/assets/img/scatter.png" alt="Home" width="50%">

# Third step

The following step is defining the elements. This definition is carried in the incidence matrix, where we establish the set of nodes which constitute each element. We can notice in the Figure below that all elements share the node at the origin, therefore we can write the incidence matrix as

```python

# incidence matrix pre-allocation
inci = np.zeros((nel, 3))

for i in range(0, nel):
   #central node
   inci[i][0] = 1
   # second node
   inci[i][1] = i + 2
   # third node
   inci[i][2] = i + 3

```
The simplest way to plot these set of elements is

```python

fig, ax = plt.subplots()
plt.xlim([0,1])
plt.ylim([0,1])
ax.fill(coord[:, 1], coord[:,2], 'b')

```
This Python code above plots all elements in blue as illustrated in the first Figure. If you want to plot each element separately as illustrated in the Figure below, you should write the following code
```python
fig, ax = plt.subplots()
 plt.xlim([0,1])
 plt.ylim([0,1])
 for i in range(0, nel):
    # set of nodes
    node1 = inci[i][0]
    node2 = inci[i][1]
    node3 = inci[i][2]

    # x-axis position
    x1 = coord[int(node1) - 1][1]
    x2 = coord[int(node2) - 1][1]
    x3 = coord[int(node3) - 1][1]
    x  = np.array([x1, x2, x3])

    # y-axis position
    y1 = coord[int(node1) - 1][2]
    y2 = coord[int(node2) - 1][2]
    y3 = coord[int(node3) - 1][2]
    y  = np.array([y1, y2, y3])

    if i%2 == 0:
       ax.fill(x, y,'k',alpha=0.3)
    else:
       ax.fill(x, y,'b',alpha=0.3)

```


# Fourth step
