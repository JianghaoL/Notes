# Introduction

## Signals

Time-domain signal = a signal that describes the change in some phenomenon *over time*.

`Digital` : Discrete
`Real World (analog)` : Continuous

> It is ***possible*** to perfectly represent a continuous signal using discrete points.

## Discrete Signals

`Sampling` : Periodically measure the value of some quantity
`Smaple` : Each one of the measurements

`Sampling Period` : The duration in-between consecutive samples
***X time / Y sample***

## Sampling and Aliasing

> Do not "connect the dot" because information could be lost (you do not know what is between the dots)

Improve the fidelity of discrete signals by *shortening* the gap between samples - by reducing *sampling period*

> Understand  ***precisely*** how often you must sample in order to avoid information loss

`Oversampling` : When sampling more often than is necessary
Oversampling can lead to memory or computational resources being wasted.

`Undersampling` : When sampling too infrequently and lose information

## Sound Waves

`Compression` : The squeezing of the air molecules
`Rarefaction` : The spaciousness between the particles

`Frequency` : The speed of vibration. Measured in `cycles per second = Hz`

## Timbre

`Timbre` : The quality of a sound that is distinct from pitch and intensity

> Timbre is largely determined by the presence or absence of *overtones* or *harmonics*


# Sines and Sampling

## Nyquist-Shannon Sampling Theorem

> **If a signal contains no frequencies than B Hz, it is completely determined by giving its ordinates at a series of points (samples) spaced 1/2B seconds apart.**

Sampling-rate-centric:
> Given a sampling of N Hz, you cannot accurately sample any signal containing frequencies greater than or equal to N/2 Hz.

## Nyquist Frequency

`Nyquist Limit` : Half of the sampling frequency

If passed, the sampled signal becomes a representation of a new wave which begins decrease in frequency.

## Wagon Wheel Effect

> Wheels appearing to spin backwards.

## Sine Wave Aliasing

> **The Sampled Sine Wave Theorem**
> Given a sampling rate of X Hz and an integer N, a sine wave at a frequency of F is indistinguishable from a sine wave at a frequency of `F + (N x X)` after sampling


***Every sampled sine wave has an infinite number of aliases***

# Transforms and Notation

## Transforms

> **The Cartesian to Polar Transform**
>
> Given **(x, y)** in Cartesian representation,
> P can be determined by
> **$$P\space(\arctan(\frac y x), \sqrt(x^2 + y^2))$$**

> **The Polar to Cartesian Transform**
>
> Given P in polar representation,
> (x, y) can be determined by
> **$$P\space(r \times \cos\theta , r \times \sin\theta  )$$**

## The Fourier Transform

> The *Fourier Transform* is a tool for switching between time-based coordinates and frequency-ased coordinates.

The Discrete Fourier Transform takes a time-domain signal and produces a list of phasors which, when summed together, will reproduce the signal.

We can combine or sum phasors by stringing them together into a chain. The first phasor is placed at the origin, and the subsequent phasor is "attached" to the tip of the previous phasor.

> **The mathmatical expression is:**
>
> **$$\sum _{i=1}^{\mathbb{N}}A_i\sin(f_i\times phase)$$**

## The Discrete Fourier Transform

***Classical Notation***

> **$$DFT[k] = \sum _{n=0}^{N-1}x[n]\cdot e^{-\phi i}$$**
> **$$ where\space \phi = k \frac n N 2\pi$$**

**$DFT$** is a list of **N** *complex numbers*. It is the output of the Discrete Fourier Transform. Each complex number in the DFT list encodes the *amplitude* and *phase* of a single phasor.

**$x[n]$** is an arbitrary time-domain signal with a length of **N** samples. It is the input to the Discrete Fourier Transform.

**$e^{-\phi i}$** is a compact way of describing a pair of sine and cosine waves.

**$\phi = k \frac n N 2\pi$** is a way of describing the phase and frequency of a sine wave.

## Euler's Formula

Looking at

**$$e^{-\phi i}$$**
we can choose to represent the points on a *complex* planing using either polar or Cartesian coordinates. When using **$e$**, we use the *polar* form of representation. The relationship between the Cartesian and polar form is given by *Euler's Formula*, which states that

**$$e^{\phi i} = \cos(\phi) + \sin(\phi) i$$**

It establishes the relationship between **$e$** and the unit circle on the complex plane. I states that **$e$ raised to any imaginary nuber will produce a point on the unit circle**. This construction can be reffered to as a *complex* phasor.

## Geometric Interpretation of the DFT

Recall that the classical notation is written as 

**$$DFT[k] = \sum_{n=0}^{N-1}x[n] \cdot e^{-\phi i}$$**

we can perform a subsitution using Euler's formula, where

> **$$ DFT[k] = \sum_{n=0}^{N-1}x[n] \cdot (\cos (\phi) - \sin(\phi)i)$$**

# Inside The DFT

## Correlation

When two signals tend to point in the same direction, we say that  they are *correlated*.

Signals show a *positive* correlation if they tend to increase and decrease together.

Signals show a *negative* correlation if one tends to increase when the other decreases, and vice versa.

Signals are *decorrelated* if there is no discernible relationship between them.

**The *dot product* allows us to measure the amount of correlation between two signals.**

> **A sine and cosine wave at the same frequency are actually decorrelated.**

## Correlation and Contribution

> **Any waveform can be thought of as an aggregation of one or more sine waves.**

We can compute the dot product of a compound waveform with a pure sinusoid. If the resulting dot product is *none-zero*, meaning the two signals are correlated, we know that the sinusoid is present in the compound waveform. The magnitude of the dot product tells us *how much* of the sinusoid is present.

## Correlation With Sine and Cosine

We can improve the aforementioned algorithm by computing *two* dot products instead of one.

We will also compute the dot product with a *cosine* wave. Because a cosine wave is simply a sine with a phase shift of 90 degrees, it ensures that we will always register a non-zero reading on the algorithm no matter what the phase of the incoming signal might be.

Each of the complex numbers in the **$DFT$** output is referred to as a **$bin$**. The first bin is referred to as the **$DC bin$**. The sine and cosine singals for the DC bin are just flat lines, because the **$k$** term in the equation is 0 for the first bin. The DC bin gives us a measurement of the input signal's *average* value.

# Usage and Interpretation of DFT

## Frequency Resolution of the DFT

Each bin in the DFT output corresponds to a particular freequency.

The frequency of the $k^{th}$ bin is given by

**$$k\times (sampling\space rate / N)$$**

where N is the number of samples in the input signal.

Notice that the bins are always evenly distributed across the frequency spectrum from $0\space Hz$ up to the sampling rate. Notice also that the bins are always harmonically related. In other words, all bin frequencies are multiples of the second bin frequency (the bin right after the DC bin). This frequency can be thought of as the *fundamental frequency* of the DFT, calculated as **$(sampling\space rate / N)$** Hz.

To increase the resolution of DFT, we can feed more sample to it.

> The resolution of the DFT output is directly proportional to the length of the input singal we feed it.


## Zero-Padding

> It is possible to interpolate or "fill-in" the output of the DFT by simply appending zeroes to the end of the input signal.

Appending zeros to the end of the signal does *NOT* alter the frequency content of the input sygnal in any undue way, and it merely interpolates the output spectrum.

> **Zero-padding does not *add* any useful information to the signal.**

## Spectral Leakage

> **When any frequency component of the input signal does not *perfectly* match up with an available bin frequency, the energy related to that frequency component will "spill" out into *all* of the other bins.**

The Discrete Fourier Transform gives a ***sampled*** version of the Countinuous Fourier Transform.

When the input signal has integral number of periods, all of the DFT samples will lie at either zero magnitude or on the very tip of the continuous curve's main "bump".

## Windowing

To remove discontinuities, we can *window* the input signal.

We can craft a window signal which tapers off to zero at its ends in order to remove the discontinuities.