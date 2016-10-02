---
layout: default
---

# Part three: Pandas and Scipy

In this third and last post on this series, we're going to look at two additional libraries that are 
extremenly useful for data analysis with Python; Scipy and Pandas. 


# R-like data analysis with Pandas 

Pandas (`pandas`) provides a high-level interface to working with 
"labeled" or "relational" data. This is in contrast to Numpy that 
deals with raw matrices / arrays, and leaves any tracking of 
"labeling" up to the developer. 

More specifically, pandas is well suited for:

* Tabular data with heterogeneously-typed columns, as in an SQL 
  table or Excel spreadsheet
* Ordered and unordered (not necessarily fixed-frequency) time series data
* Arbitrary matrix data (homogeneously typed or heterogeneous) 
  with row and column labels
* Any other form of observational / statistical data sets. The data actually 
  need not be labeled at all to be placed into a pandas data structure

 

## Series and Data-frames

To deal with these datasets, Pandas uses two main objects: `Series` (1d) and
`DataFrame` (2d). 

R users familiar with the `data.frame` concept will find that the Pandas `DataFrame`
provides the same functionality plus more. 

### Reading data from CSV or Excel files

To create a `DataFrame` object from data in a CSV (Comma Separated Values) file, 
Pandas, provides a simple loading function: 
```py

df = pandas.read_csv('data.csv')
```

Similarly a table of data can be read from an Excel Worksheet using 

```py
df = pandas.read_excel('workbook.xlsx')
```
with optional (**keyword**) arguments like `sheetname`, `header`, `skiprows` to 
handle specifying the sheet to load, where column labels (headers)
should be read from, and how many rows to skip before reading (for tables that 
start part-way down a spreadsheet). 

### Getting and Inspecting data

`DataFrame`s contain utility member functions such as `head`, `tail`, and
`describe` to provide a view of the initial rows, last rows, and a summary 
of the data they contain. 

Specific rows/columns can be queried using column headers, e.g. 
if a `DataFrame` contains the headers `['A', 'B', 'C', 'D']`
then column A can be returned as a 1d `Series` using 

```py 
df['A']
```
Standard Python slice notation may also be used to select rows, e.g. 

```py
df[:3]
```
selects the first 3 rows of the `DataFrame`. 

### Strings 

Pandas can handle strings in tables of data, as well as numerical values. 

For example if we have information such as 
```plain 
Filename,   Cells,  keywords
file1.tif,    120,  normal, rod-shaped
file2.tif,     98,  lysed
file3.tif,     40,  large, rod-shaped
file4.tif,    101,  spotty, rod-shaped
...
```
in an Excel spreadsheet, this could be loaded into a `DataFrame` as 
described above, and subsequently we can access the keywords column 
and query which files contained "rod-shaped" cells:

```py
keywords = pd['keywords']       # Select keywords column as Series
is_rod_shaped = keywords.str.contains("rod-shaped") 
```
Slicing the first few rows would give: 
```py 
print(is_rod_shaped[:3])
```
outputs
```
Filename
file1.tif,   True 
file2.tif,   False
file3.tif,   True 
Name: keywords, dtype: bool
```

Using string methods like `contains` allows us to easily 
incorporate string information from tables of data. 

For further examples of the types of string operations available 
see [here](http://pandas.pydata.org/pandas-docs/stable/text.html) 
(a table of available methods is included at the bottom of the linked 
page). 


### Cleaning messy data

When working with data that has been entered by using third-party software, 
or via manual entry, we often encounter entries that are not readily usable 
within a numerical workflow. 

Examples include numbers that are parsed as strings because of 
mistakes such as accidentally being entered as 10.0.1 (extra "." at the end), and
"NaN" values that are represented using other notation (e.g. "NA", "??"). 

Luckily Pandas import functions like `read_csv` include keyword arguments 
for specifying what `nan` values look like (`na_values`). 

For example, if the data includes "NA" and "?", we would use something like 

```py
df = pandas.read_csv("file1.csv", na_values=["NA", "??"])
```

If we need to deal with erroneous representation of numbers, we can 
first load the data as strings, and then parse the strings into the 
correct format; 

```py
df = pandas.read_csv("file1.csv", na_values=["NA", "??"], 
        dtype={"Column 10":str})
```
specifies that the series labeled "Column 10" should be handled as
strings, and subsequently
```py 
num_dots = df['Column 10'].str.count("\.")      # Need backslash because count treats "." as regex wildcard otherwise!
wrong_strings = df['Column 10'][num_dots > 1]
right_strings = wrong_strings.str[-1::-1].str.replace("\.", "", n=1).str[-1::-1]
df['Column 10'][num_dots > 1] = right_strings
```
The unwieldly looking second-last expression is so long because the string method `replace` 
only acts from left to right, so we needed to sandwich it in the `str[-1::-1]` bits to reverse the 
string and then unreverse it, as we stated above that the first "." was the correct one!

### Timestamps

When working with system timestamps expressed in terns of seconds (e.g. Unix timestamps), e.g. 
```plain 
Filename,   Creation time
data1.xlsx,    1387295797
data2.xlsx,    1387295796
data3.xlsx,    1387295743
data4.xlsx,    1387295743
```

we can easily convert these into `datetime` data types using: 

```py
df['Creation time'] = pandas.to_datetime( df['Creation time'], unit='s')
```
to produce 
```plain
Filename,   Creation time
data1.xlsx,    2013-12-17 15:56:37 
data2.xlsx,    2013-12-17 15:56:36
data3.xlsx,    2013-12-17 15:55:43
data4.xlsx,    2013-12-17 15:55:43
```

We can even now use string-like comparison to compare dates!

```py
df = df[df['Creation time'] > '1970-01-01']
```
(this example selects all rows as they were all created since 1970!). 


### Plotting a column 

Once loaded as described in the previous section, a dataframe can be plotted 
using `pandas.DataFrame` interface to `matplotlib`; 

```py
df.plot()
```
plots each series (column) of the `DataFrame` as a separate line. 

### Saving to CSV or Excel 

Much in the same way as a `DataFrame` be easily loaded from a CSV or Excel 
file, it can be written back just as easily: 

```py 
df.to_csv('foo.csv')
```
**Note**: `to_csv` is a *member*-function of the `DataFrame` object, while 
`read_csv` was a module-level function of the `pandas` module. 



# Scipy : Additional scientific computing modules

Numpy is one of the core modules of the Scipy "ecosystem";
however, additional numerical
routines are packages in the separate `scipy` module.

The sub-modules contained in `scipy` include:

* `scipy.cluster`     : Clustering and cluster analysis
* `scipy.fftpack`     : Fast fourier transform functions
* `scipy.integrate`   : Integration and ODE (Ordinary Differential Equations)
* `scipy.interpolate` : Numerical interpolation including splines and radial basis function interpolation
* `scipy.io`          : Input/Output functions for e.g. Matlab (.mat) files, wav (sound) files, and netcdf.
* `scipy.linalg`      : Linear algebra functions like eigenvalue analysis, decompositoins, and matrix equation solvers
* `scipy.ndimage`     : Image processing functions (works on high dimensional images - hence nd!)
* `scipy.optimize`    : Optimization (minimize, maximize a function wrt variables), curve fitting, and root finding
* `scipy.signal`      : Signal processing (1d) including convolution, filtering and filter design, and spectral analysis
* `scipy.sparse`      : Sparse matrix support (2d)
* `scipy.spatial`     : Spatial analysis related functions
* `scipy.statistical` : Statistical analysis including continuous and discreet distributions and a slew of statistical functions


Now let's introduce their general usage with a sample project.  

### Loading data : `scipy.io`

`scipy.io` contains file reading functionality to load Matlab .mat files,
NetCDF, and wav audio files, amongst others.

We will use the `wavfile` submodule to load our audio file;

```py
import scipy.io.wavfile as scwav
data, rate = scwav.read("audio_sample.wav")
```
Next we can inspect the the loaded data using a plot (plotting just the 
first 500 values),
```py
import matplotlib.pyplot as plt
# Create a "time" axis array; at present there are
# `rate` data-points per second, as rate corresponds to the
# data sample rate.
Nsamples = len(data)
time_axis = np.arange(Nsamples)/rate
plt.plot( time_axis[:500], data[:500])
plt.show()
```

We should see an audio trace.

If you have headphones with you and play the audio, you'll hear a noisy,
but still recongnizable sound-bite.

Let's see if we can improve the sound quality by performing some basic
signal processing!

### Cleaning the data using : `scipy.signal`

There are a host of available 1d filters available in Scipy.
We're going to construct a simple low-pass filter using the
`firwin` function;

```py
import scipy.signal as scsig

# Calculate the Nqyist rate
nyq_rate = rate / 2.
# Set a low cutoff frequency of the filter: 1KHz
cutoff_hz = 1000.0
# Length of the filter (number of coefficients, i.e. the filter order + 1)
numtaps = 29
# Use firwin to create a lowpass FIR filter
fir_coeff = scsig.firwin(numtaps, cutoff_hz/nyq_rate)

# Use lfilter to filter the signal with the FIR filter
filtered = scsig.lfilter(fir_coeff, 1.0, data)

# Write the filtered version into an audio file so we can see
# if we can hear a difference!
scwav.write("filtered.wav", rate, filtered)
```

While "manual" verification by listening to the filtered signal
can be useful, we would probably want to also analyze both the original
signal and the effect the filtering has had.


### Spectral analysis : `scipy.fftpack` and `scipy.signal`

To analyze a 1d filter, we often generate a _periodogram_, which
essentially gives us information about the frequency content of the
signal. The perriodogram itself is a power-spectrum representation
of the Fourier transform of the signal; however, this is not a detailed
course in 1d signal processing, so if you have no idea what that means then
don't worry about it!

For sound signals, the periodogram relates directly to the
pitches of the sounds in the signal.

Let's generate and plot a periodogram using scipy:

```py
plt.figure()
f, Pxx = scsig.periodogram(data.astype(float), rate)
plt.semilogy(f, Pxx)
plt.xlabel('frequency [Hz]')
plt.ylabel('PSD [V**2/Hz]')
plt.show()
plt.figure()
f, Pxx = scsig.periodogram(filtered.astype(float), rate)
plt.semilogy(f, Pxx)
plt.xlabel('frequency [Hz]')
plt.ylabel('PSD [V**2/Hz]')
plt.show()
```

For those wanting more control over the spectral analysis, the `fftpack`
submodule offers access to a range of Fast-Fourier transform related
functions.

Performing the spectral analysis shows us that the signal did indeed
contain a high frequency noise component that the filtering should,
at least to some degree have helped remove.

