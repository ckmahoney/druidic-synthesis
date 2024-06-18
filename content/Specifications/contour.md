+++
title = 'Contour'
date = 2024-06-07T18:53:02-04:00
+++

This is conventionally called "amplitude envelope." Traditional amplitude modulation techniques provide a great reference point.   

Here we refine the vocabulary used to describe how loud something is, and how it gets softer or louder over time. A small set of names is selected to represent group of falling, rising, or constant sound. We also describe a contour as **finite** or **infinite**.

- **finite** contours produce amp values in [0, 1] to fixed time d. The signal is 0 valued after d.
- **infinite** contours produce amp values in [0, 1] for all d.

Contours are a function of time scaled to the interval of [0, 1]. All contour methods accept first parameter
- `x` is a number between [0, 1] representing the position in time.

Contours also accept three additional parameters: 

- `k` is an iterator considered to be harmonics; which actually is an index. It has a minimum value of 1 and supports (Nyquest Frequency / Minimum Application Frequency) as its maximum value.
- `c` is the centroid location, describing the vertical position of highest value. Options are "low", "mid", "high". 
    - `low` has strongest amplitudes in early k (e.g. for freq 100 Hertz, k < 15) and weakest amplitudes at highest k
    - `mid` places the highest amplitude in the midrange (e.g. for freq 100 Hertz, 15 <= k <= 32 ) and attenuates the low and high frequencies
    - `high` has weakest ampiltude in early k and highest at max k (e.g. for freq 100 Hertz, k > 32)
- `d` is a free parameter representing duration (in cycles). It is any positive value, and 1 can be considered the "default" state.

## Snap, Burst
finite or infinite
Exponential decay

## Pluck
finite
Quadratic decay

## Constant
infininte
Sustained or "infinite" values

## Grow
finite
Quadratic growth. 

## Surge, Bloom
finite or infinite
Exponential growth. 
Grow convex, bloom concave

We can also define a macro function which mixes from these, enabling smooth amp envelope morphing (from a pluck to a high attack pad for example).

## Infinite

Infinite signals still accept input in the range of [0, 1] as this can always describe the applied result for music composition. In other words, in real life there is no such thing as Infinite Sound. The longest scheduled composition I know of is 639 years due to finish in 2640. 

For druidic synthesis, the infinite producer takes a two steps to create the illusion of infinite variation:
- produce ultralow frequency modulators (may look like noise from a macro point of view)
- pass the interval of [0,1] into an asymptotic function
