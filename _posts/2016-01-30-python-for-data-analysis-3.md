---
layout: default
---

# Part three: Pandas and Scipy

In this third and last post on this series, we're going to look at two additional libraries that are 
extremenly useful for data analysis with Python; Scipy and Pandas. 

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

