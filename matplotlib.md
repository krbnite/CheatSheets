https://matplotlib.org/gallery.html

## Simple Example
```python
# If in Jupyter Notebook or Console, uncomment
#%matplotlib inline

import numpy as np
from matplotlib import pyplot as plt

mu, sigma = 0,1
data = mu + sigma * np.random.randn(1000)

# histogram
n, bins, patches = plt.hist(data, 25, normed=1, facecolor='b', alpha=0.63)

# Annotate, etc
plt.xlabel('Thing of Interest')
plt.ylabel('Probability')
plt.title('Probability of a Thing of Interest')
plt.text(-2, 0.025, rf'$\mu={mu}, \sigma={sigma}$')
plt.axis([x0,xf,y0,yf])
plt.grid(True)
plt.show()
```

## Colors
* b: blue
* etc

## Symbols
* solid line: -
* dashed line: --
* dot-dash line: -.
* dotted line: :
* triangles
* etc

## Building Multiple Figures/Plots
Sometimes you need to build figures inside functions or have a few figures you are working on
at once... In this case, it would be nice to treat a figure as an object that you can add attributes to.  Well,
you're in luck!  That is simple to do.

```
fig1 = plt.figure("Figure 1")

# Work on some subplots in fig1
nrows=2; ncols=3
sub1 = fig1.add_subplot(nrows, ncols, 1) # position 1
plt.plot(...)  # put something there
sub3 = fig1.add_subplot(nrows, ncols, 3) # position 3
plt.hist(...)  # put something there
sub5 = fig1.add_subplot(nrows, ncols, 5) # position 5
plt.plot(...)  # put something there
```

## Multiple Lines, Single Plot
```
plt.plot(
    time, var1, 'go',
    time, var2, 'b--',
    time, var3, '^k'
)
plt.xlabel('Time')
plt.ylabel('Variables of Interest')
plt.title('Time Evolution of Variables of Interest')
plt.show()
```


## Tick Marks & Tick Labels

## Grids

## Annotations
