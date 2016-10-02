---
layout: default
---

# Part 2

Welcome to the second in the series of posts about Python for Data Analysis. 
In this post we're going to cover Numpy and plotting with Matplotlib.  

# Numerical Analysis with Numpy 

Numpy is probably the most significant numerical computing 
library (module) available for Python. 

It is coded in both Python and C (for speed), providing high level access
to extremely efficieny computational routines. 

# Basic object of Numpy: the Array

One of the most basic building blocks in the Numpy toolkit is the 
Numpy N-dimensional array (`ndarray`), 
which is used for arrays of between 0 and 32 
dimensions (0 meaning a "scalar"). 

## Creating Arrays

`ndarray`s can be created in a number of ways.

### From Python objects

Numpy includes a function called `array` which can be used to 
create arrays from numbers, *lists* of numbers or *tuples* of numbers. 

E.g. 

```py
numpy.array([1,2,3])
numpy.array(7)
```
creates the 1d array and number (scalar):
```
[1 2 3]
7
```

Nested lists/tuples produce n-dimensional arrays:
```py
numpy.array([[1,2], [3,4]])
```
creates the 2d array
```
[[1 2]
 [3 4]]
```

NB: you can create arrays from `dict`s, lists of strings, and other 
data types, but the resulting `ndarray` will contain those objects 
as elements (instead of numbers), and might not behave as you expect!

### Predefined arrays

There are also array creation functions to create specific types of 
arrays: 
```
numpy.zeros((2,2))  # creates  [[ 0 0]
                    #           [ 0 0]]

numpy.ones(3)       # creates [ 1 1 1 ]

numpy.empty(2)      # creates [  6.89901308e-310,   6.89901308e-310]
```
NB: `empty` creates arrays of the right size but doesn't set the values 
which start off with values determined by the previous memory content!

### Random numbers 

`numpy.random` contains a range of random-number generation functions. 

Commonly used examples are
```
numpy.random.rand(d0, d1, ..., dn) # Uniform random values in a given shape, in range 0 to 1
numpy.random.randn(d0, d1,...,dn) # Standard normally distributed numbers (ie mean 0 and standard deviation 1)
```


### Loading data from files

One to two dimensional arrays saved in comma-separated text 
format can be loaded using `numpy.loadtxt`:
```py
arr2d = numpy.loadtxt('filename.csv', delimiter=",")    # The default delimiter is a space!
```
Similarly an array can be saved to the same format:
```py
numpy.savetxt('filename.csv', arr2d_2, delimiter=",")    # The default delimiter is a space!)
```

Numpy also has an internal file format that can save and load 
N-dimensional arrays, 
```py
arrNd = numpy.load('inputfile.npy')
```
and
```py
numpy.save('outputfile.npy', arrNd2)
```

**NOTE:** As usual, _full_ paths are safer than relative paths, which are used 
here for brevity. 

However, these files are generally not usable with other 
non-python programs. 


## Working with `ndarray`s

Once an array has been loaded or created, it can be manipulated using 
either array object member functions, or numpy module functions. 

### Member functions

`ndarray`s are **objects** as covered briefly in the last section, 
and have useful attributes (~ data associated with the array): 

```
ndarray.flags   Information about the memory layout of the array.
ndarray.shape   Tuple of array dimensions.
ndarray.strides Tuple of bytes to step in each dimension when traversing an array.
ndarray.ndim    Number of array dimensions.
ndarray.data    Python buffer object pointing to the start of the arrayâ€™s data.
ndarray.size    Number of elements in the array.
ndarray.itemsize    Length of one array element in bytes.
ndarray.nbytes  Total bytes consumed by the elements of the array.
ndarray.base    Base object if memory is from some other object.

ndarray.dtype   Data-type of the arrayâ€™s elements.


ndarray.T   Same as self.transpose(), except that self is returned if self.ndim < 2.
ndarray.real    The real part of the array.
ndarray.imag    The imaginary part of the array.
ndarray.flat    A 1-D iterator over the array.
ndarray.ctypes  An object to simplify the interaction of the array with the ctypes module.
```

as well as many useful **member-functions** including: 

```
ndarray.reshape : Returns an array containing the same data with a new shape.
ndarray.resize  : Change shape and size of array in-place.
ndarray.transpose (or ndarray.T) : Returns a view of the array with axes transposed.
ndarray.swapaxes : Return a view of the array with axis1 and axis2 interchanged.
ndarray.flatten  : Return a copy of the array collapsed into one dimension.
ndarray.squeeze  : Remove single-dimensional entries from the shape of a.

ndarray.sort    : Sort an array, in-place.
ndarray.argsort : Returns the indices that would sort this array.
ndarray.diagonal: Return specified diagonals.
ndarray.nonzero :  Return the indices of the elements that are non-zero.

ndarray.argmax  : Return indices of the maximum values along the given axis.
ndarray.min : Return the minimum along a given axis.
ndarray.argmin  : Return indices of the minimum values along the given axis of a.
ndarray.ptp : Peak to peak (maximum - minimum) value along a given axis.
ndarray.clip    : Return an array whose values are limited to [min, max].
ndarray.conj : Complex-conjugate all elements.
ndarray.round : Return array with each element rounded to the given number of decimals.
ndarray.trace  : Return the sum along diagonals of the array.
ndarray.sum    : Return the sum of the array elements over the given axis.
ndarray.cumsum  : Return the cumulative sum of the elements along the given axis.
ndarray.mean    : Returns the average of the array elements along given axis.
ndarray.var : Returns the variance of the array elements, along given axis.
ndarray.std : Returns the standard deviation of the array elements along given axis.
ndarray.prod    : Return the product of the array elements over the given axis 
ndarray.cumprod : Return the cumulative product of the elements along the given axis.
ndarray.all : Returns True if all elements evaluate to True.
ndarray.any : Returns True if any of the elements of a evaluate to True.
```
including arithmetic operations on array elements: 
```
+   Add 
-   Subtract 
*   Multiply 
/   Divide 
//  Divide arrays ("floor" division)
%   Modulus operator 
**  Exponent operator (`pow()`)  

# Logic
&   Logical AND
^   Logical XOR
|   Logical OR 
~   Logical Not

# Unary 
-   Negative 
+   Positive 
abs Absolute value
~   Invert 
```
and comparison operators
```
==  Equals
<   Less than 
>   More than 
<=  Less than or equal
>=  More than or equal
!=  Not equal
```

### Non-member functions

Additional functions operating on arrays exist, that the Numpy developers
felt shouldn't be included as member-functions. 
These include

```
# Trigonometric and hyperbolic functions
numpy.sin       : Sine 
numpy.cos       : Cosine
numpy.tan       : Tangent 
# And more trigonometric functions like `arcsin`, `sinh`

# Statistics 
numpy.median    : Calculate the median of an array
numpy.percentile : Return the qth percentile
numpy.cov       : Calculate the covariance matrix
numpy.histogram : Compute the histogram of data (does binning, not plotting!) 

# Differences
numpy.diff      : Elemenet--element difference 
numpy.gradient  : Second-order central difference

# Manipulation 
numpy.concatenate : Join arrays along a given dimension 
numpy.rollaxis  : Roll specified axis backwards until it lies at given position
numpy.hstack    : Horizontally stack arrays
numpy.vstack    : Vertically stack arrays
numpy.repeat    : Repeat elements in an array
numpy.unique    : Find unique elements in an array

# Other
numpy.convolve  : Discrete linear convolution of two one-dimensional sequences
numpy.sqrt      : Square-root of each element
numpy.interp    : 1d linear interpolation

```


### Accessing elements

Array elements can be accessed using the same slicing mechanism as lists; e.g. 
if we have a 1d array assigned to `arr1` containing `[5, 4, 3, 2, 1]`, then 

* `arr1[4]` accesses the 5th element = `1`
* `arr1[2:]` accesses the 3rd element onwards = `[3,2,1]`
* `arr1[-1]` accesses the last element = `1`
* `arr1[-1::-1]` accesses the last element onwards with step -1, = `[1,2,3,4,5]` 
  (i.e. the elements reversed!)

Similarly higher dimensional arrays are accessed using comma-separated slices.  
If we have a 2d array;
```py
a = numpy.array([[ 11, 22],
                 [ 33, 44]])
```
(which we also represent on one line as `[[ 11, 22], [ 33, 44]]`), then

* `a[0]` accesses the first row = `[11, 22]`
* `a[0,0]` accesses the first element of the first row = `11`
* `a[-1,-1]` accesses the last element of the last row = `44`
* `a[:,0]` accesses all elements of the first column = `[11,33]`
* `a[-1::-1]` reverses the row order = `[[33, 44], [11,22]]`
* `a[-1::-1,-1::-1]` reverses both row and column order = `[[ 44, 33], [ 22, 11]]`

The same logic is extended to higher-dimensional arrays. 

#### Using binary arrays (~masks)

In addition to the slice notation, a very useful feature of Numpy is 
**logical indexing**, i.e. using a binary array **which has the same shape 
as the array in question** to access those elements for which the binary array 
is `True`. 

Note that the returned elements are always returned as a flattened array. E.g. 

```py
arr1 = np.array([[ 33, 55], 
                 [ 77, 99]])
mask2 = np.array([[True, False], 
                 [False, True]])
print( arr1[ mask2 ]) 
```
would result in 
```
[33, 99]
```
being printed to the terminal. 

The usefulness of this approach should become apparent as you learn to use Numpy!

### Dealing with NaN and infinite values

Generally speaking, Python handles division by zero as an error; to avoid this 
you would usually need to check whether a denominator is non-zero or not using 
an `if`-block to handle the cases separately. 

Numpy on the other hand, handles such events in a more sensible manner. 

#### Not a Number

NaNs (`numpy.nan`) (abbreviation of "Not a Number") occur when a computation can't 
return a sensible number; for example a Numpy array element `0` when divided 
by `0` will give `nan`. 

They can also be used  can be used to represent missing data.

NaNs can be handled in two ways in Numpy;
* Find and select only non-NaNs, using `numpy.isnan` to identify NaNs
* Use a special Nan-version of the Numpy function you want to perform (if it exists!)

For example, if we want to perform a mean or maximum operation, Numpy offers `nanmean` and 
`nanmax` respectively. 
Additional NaN-handling version of `argmax`, `max`, `min`, `sum`, `argmin`, 
`mean`, `std`, and `var` exist (all by prepending `nan` to the function name). 

When a pre-defined NaN-version of the function you need doesn't exist, you will 
need to select only non-NaN values; e.g. 

```py 
allnumbers = np.array([np.nan, 1, 2, np.nan, np.nan])

# We can get the mean already
print(allnumbers.nanmean())                                 # Prints 1.5 to the terminal 

# But a nan version of median doesn't exist, and if we try 
print(np.median( allnumbers ))                              # Prints nan to the terminal 
# The solution is to use
validnumbers = allnumbers[ ~ np.isnan(allnumbers) ]
print(np.median( validnumbers ))                            # Prints 1.5 to the terminal 
```

#### To Infinity, and beyond...

Similarly, a Numpy array non-zero element divided by `0` gives `inf`, Numpy's representation of infinity. 

However, Numpy does not have any functions that handle infinity in the same way as the nan-functions (i.e. by 
essentially ignoring them). 
Instead, e.g. the mean of an array containing infinity, is infinity (as it should be!). 
If you want to remove infinities from calculations, then this is done in an analogous way 
to the NaNs; 
```py
allnumbers = np.array([np.inf, 5, 6, np.inf])
print(allnumbers.mean())                                # prints inf to the terminal
finite_numbers = allnumbers[ ~np.isinf(allnumbers) ] 
print(finite_numbers.mean())                            # prints 5.5 to the terminal 
```

> #### Tip: `np.isfinite` for both
> 
> `np.isfinite` will return an array that contains `True` everywhere the  
> input array is **neither infinite nor a NaN**.

## Next steps: Visualization helps!

Now that we have a basic understanding of how to create and manipulate 
arrays, it's already starting to become clear that producing numbers alone
makes understanding the data difficult. 

With that in mind then, let's start working on visualization, 
and perform further Numpy analyses there!


# Plotting with Matplotlib

In the last section we were introduced to Numpy and the fact that
it is a numerical library capable of "basic" numerical analyses on Arrays.

We could use Python to analyse data, and then save the result as
comma separated values, which are easily imported into e.g. GraphPad or Excel
for plotting.  

**But why stop using Python there?**

Python has some powerful plotting and visualization libraries, that allow us
to generate professional looking plots in an automated way.

One of the biggest of these libraries is Matplotlib.

Users of Matlab will find that Matplotlib has a familiar syntax.

For example, to plot a 1d array (e.g. stored in variable `arr1d`) 
as a line plot, we can use

```py
import matplotlib.pyplot as plt       
plt.plot(arr1d)
plt.show()
```

> **Reminder: aliases**
>
> Here we used the **alias** (`as`) feature when importing matplotlib to
> save having to type `matplotlib.pyplot` each time we want to access
> the `pyplot` sub-module's plotting functions!

**NOTE**

In the following notes, most of the time when I refer to "`matplotlib`'s X 
function" (where X is changeable), I'm actually referring to the function 
found in `matplotlib.pyplot`, which is `matplotlib`'s main plotting 
submodule. 


## The `show` function

What's going on here?

The `plot` function does just that; it generates a
plot of the input arguments.

However, matplotlib doesn't (by default) **show** any plots until the `show` 
function is called. This is an intended feature:

* To create multiple plots/figures before pausing the script to show them
* To not show any plots/figures, instead using a plot saving function
  - this mode of operation is better suited to non-interactive (batch) processing

It is possible to change this feature so that plots are shown as soon as they 
are created, using `matplotlib`'s `ion` function (~ interactive on). 

## Creating new figures with `figure`

Often we need to show multiple figures; by default calling several plot 
commands, one after the other, will cause each new plot to draw over previous plots. 

To create a new figure for plotting, call 
```py
plt.figure()
# Now we have a new figure that will receive the next plotting command
```

## A short sample of plot types

Matplotlib is good at performing 2d plotting.

As well as the "basic" `plot` command, `matplotlib.pyplot` includes

```
bar             : Bar plot (also barh for horizontal bars)
barb            : 2d field of barbs (used in meteorology)
boxplot         : Box and whisker plot (with median, quartiles)
contour         : Contour plot of 2d data (also contourf - filled version)
errorbar        : Error bar plot
fill_between    : Fills between lower and upper lines
hist            : Histogram (also hist2d for 2 dimensional histogram)
imshow          : Image "plot" (with variety of colour maps for grayscale data)
loglog          : Log-log plot
pie             : Pie chart
polar           : Polar plot
quiver          : Plot a 2d field of arrows (vectors)
scatter         : Scatter plot of x-y data
violinplot      : Violin plot - similar to boxplot but width indicates distribution
```

Rather than copying and pasting content, navigate to the
[Matplotlib Gallery page](http://matplotlib.org/gallery.html)
to view examples of these and more plots.

You'll find examples (including source code!) for most of your
plotting needs there.




### Customizing the figure

Most plotting functions allow you to specify additional **keyword** arguments, 
which determine the plot characteristics. 

For example, a the line plot example may be customized to have a red dashed 
line and square marker symbols by updating our first code snippet to 

```py
plt.plot(arr1d, '--', color="r", markerstyle="s")
```
(or the shorthand `plt.plot(arr1d, "--r")`)

#### Axis labels, legends, etc

Additional figure and axes properties can also be modified. To do so, we
need to access figure/axis respectively. 

To create a new (empty) figure and corresponding figure object, 
use the `figure` function: 

```py
fig1 = plt.figure()
# Now we can modify the figure properties, e.g.
# we can set the figure's width (in inches - intended for printing!)
fig1.set_figwidth(10)
# Then setting the figure's dpi (dots-per-inch) will determine the 
# number of pixels this corresponds to... 
fig1.set_dpi(300)       # I.e. the figure width will be 3000 px!
```
If instead we wish to modify an already created figure object, 
we can either get a figure object by passing it's `number` propery
to the `figure` function e.g.
```py
f = plt.figure(10) # Get figure number 10
```
or more commonly, we get the active figure (usually the last created figure) 
using 
```py
f = plt.gcf()
```


Axes are handled in a similar way; 
```py
ax = plt.axes()
```
creates a default axes in the current figure (creates a figure if none are
present), while 
```py
ax = plt.gca()
```
gets the current (last created) axes object. 

Axes objects give you access to changing background colours, axis colours, 
and many other axes properties. 

Some of the most common ones can be modified for the current axes without 
needing to access the axes object as `matplotlib.pyplot` has convenience 
functions for this e.g. 
```py
plt.title("Aaaaarghh")          # Sets current axes title to "Aaaaarghh"

plt.xlabel("Time (s)")          # Sets current axes xlabel to "Time (s)"
plt.ylabel("Amplitude (arb.)")  # Sets current axes ylabel to "Amplitude (arb.)"

# Set the current axes x-tick locations and labels
plt.yticks( numpy.arange(5), ('Tom', 'Dick', 'Harry', 'Sally', 'Sue') )
```

#### A note on _style_

Matplotlib's default plotting styles are often not considered to be
desirable, with critics citing styles such as the R package ggplot's
default style as much more "professional looking". 

Fortunately, this criticism has not fallen on deaf ears, and while 
it has always been possible to create your own customized style, 
Matplotlib now includes additional styles by default as "style sheets".  

In particular, styles such as **ggplot** are very popular and essentially 
emulate ggplot's style (axes colourings, fonts, etc). 

The style may be changed before starting to plot using e.g. 
```py
plt.style.use('ggplot')
```
A list of available styles may be accessed using
```py
plt.style.available
```
(on the current machine this holds : `['dark_background', 
'fivethirtyeight', 'bmh', 'grayscale', 'ggplot']`)

You may define your own style file, and after placing it in 
a specific folder ( "~/.config/matplotlib/mpl_configdir/stylelib" )
you may access your style in the same way. 

Styles may also be composed together using 
```py
plt.style.use(['ggplot', 'presentation'])
```
where the rules in the **right-most** style will take precedent. 

### 3d plotting

While matplotlib includes basic 3d plotting functionality (via `mplot3d`),
we recommend using a different library if you need to generate
"fancy" looking 3d plots.

A good alternative for such plots isa package called `mayavi`. 

However this is not included with WinPython and the simplest route to installing
it involves installing the Enthough Tool Suite (http://code.enthought.com/projects/).

For the meantime though, if you want to plot 3d data, stick with the `mplot3d`
submodule. 

For example, to generate a 3d surface plot of the 2d data (i.e. the 
"pixel" value would correspond to the height in z), we could use

```py
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.plot_surface(X, Y, Z)
```
where `X`, `Y`, `Z` are 2d data values.  

`X` and `Y` are 2d representations of the axis values which can be 
generated using utility functions, e.g. pulling an example from the 
Matplotlib website, 
 
```
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()
ax = fig.gca(projection='3d')
X = np.arange(-5, 5, 0.25)
Y = np.arange(-5, 5, 0.25)
X, Y = np.meshgrid(X, Y)
R = np.sqrt(X**2 + Y**2)
Z = np.sin(R)
surf = ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap=cm.coolwarm,
                       linewidth=0, antialiased=False)
ax.set_zlim(-1.01, 1.01)

ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

fig.colorbar(surf, shrink=0.5, aspect=5)

plt.show()
```


[![3d plot demo](content/python2016_data/images/surface.png)](http://matplotlib.org/mpl_toolkits/mplot3d/tutorial.html#surface-plots)

## Saving plots

Now that we have an idea of the types of plots that can be generated
using Matplotlib, let's think about saving those plots.

If you call the `show` command, the resulting figure window includes
a basic toolbar for zooming, panning, and saving the plot.

However, as a Pythonista, you will want to automate saving the plot
(and probably forego viewing the figures until the script has batch
processed your 1,000 data files!).

The command to save the currently active figure is
```py
plt.savefig('filename.png')
```

Alternatively, if you store the result of the
`plt.figure()` function in a variable as mentioned above 
you can subsequently use the `savefig` *member-function*
of that figure object to specifically save that figure, even if
it is no longer the active figure; e.g.
```py
fig1 = plt.figure()

#
# Other code that generates more figures
#
# ...
#


fig1.savefig('figure1.pdf')
```
### Note on formats: Raster vs Vector

The filename extension that you specify in the `savefig` function
determines the output type; matplotlib supports

* png   - good for images!   
* jpg   (not recommended except for drafts!)
* pdf   - good for (vector) line plots 
* svg

as well as several others.

The first two formats are known as **raster** formats, which means that the
entire figure is saved as a bunch of pixels.

This means that if you open the
resulting file with an image viewer, and zoom in, eventually the plot will
look pixelated/blocky.

The second two formats are **vector** formats, which means that a line plot
is saved as a collection of x,y points specifying the line start and
end points, text is saved as a location and text data and so on.

If you open a vector format file with e.g. a PDF file viewer, and zoom in,
the lines and text will always have smooth edges and never look blocky.

As a general guideline, you should choose to mainly use vector file
(pdf, eps, or svg) output for plots as they will preserve the full data,
while raster formats will convert the plot data into pixels.

In addition, if you have vector image editing software like
<a href="https://inkscape.org/en/" 
    target="_blank">Inkscape</a> or Adobe Illustrator,
you will be able to open the
vector image and much more easily change line colours, fonts etc.



## Other annotations

We already saw how to add a title and x and y labels; 
below are some more examples of annotations that can be added. 

You could probably guess most of what the functions do (or have guessed
what the function would be called!):

```py
plt.title("I'm a title") # Note single quote inside double quoted string is ok!
plt.xlabel("Time (${\mu}s$)")       # We can include LaTex for symbols etc... 
plt.ylabel("Intensity (arb. units)")

plt.legend("Series 1", "Series 2")

plt.colorbar()      # Show a colour-bar (usually for image data). 

# Add in an arrow
plt.arrow(0, 0, 0.5, 0.5, head_width=0.05, head_length=0.1, fc='k', ec='k')
```

The last command illustrates how as with most
most plotting commands, `arrow` accepts a
large number of additional keyword arguments (aka kwargs) to
customize colours (edge and face), widths, and many more properties.


## Sub-plots

There are two common approaches to creating sub-plots. 

The first is to create individual figures with well thought out sizes (especially 
font sizes!), and then combine then using an external application. 

Matplotlib can also simplify this step for us, by providing subplot functionality.
We could, when creating axes, reposition them ourselves in the figure. 
But this would be tedious, so Matplotlib offers convenience functions to 
automatically lay out axes for us. 

The older of these is the `subplot` command, which lays axes out in a regular 
grid. 

For example 
```py
plt.subplot(221)        # Create an axes object with position and size for 2x2 grid
plt.plot([1,2,3])
plt.title("Axes 1")

plt.subplot(222)    
plt.plot([1,2,3])   
plt.title("Axes 2")

plt.subplot(223)    
plt.plot([1,2,3])
plt.title("Axes 3")

plt.subplot(224)    
plt.plot([1,2,3])
plt.title("Axes 4") 
```

**Note** that the subplot command uses 1-indexing of axes (not 0-indexing!).
The above could be simplified (at least in some scenarios) using a `for`-loop, 
e.g. 

```py
# datasets contains 4 items, each corresponding to a 1d dataset... 
for i, data in enumerate(datasets):
    plt.subplot(2,2,i+1)        
    plt.plot(data)
    plt.title("Data %d"%(i+1))
```

Recently, Matplotlib added a new way of generating subplots, 
which makes it easier to generate non-uniform grids. 

This is the `subplot2grid` function, which is used as follows, 
```py
ax1 = plt.subplot2grid((3,3), (0,0), colspan=3)
ax2 = plt.subplot2grid((3,3), (1,0), colspan=2)
ax3 = plt.subplot2grid((3,3), (1, 2), rowspan=2)
ax4 = plt.subplot2grid((3,3), (2, 0))
ax5 = plt.subplot2grid((3,3), (2, 1))
```
would generate
![grid plot](content/python2016_data/images/grid.png)
(without the labels!)



