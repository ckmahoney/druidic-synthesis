+++
title = 'Element'
date = 2024-06-02T08:29:46-04:00
+++

Elementary components of sound.


Consider the periodic table of elements. It describes, in so few parameters, the entire range of known and predicted elements that comprise our world. We can use just a few numbers, like the number of protons and electroncs, to describe emergent properties that aries from certain combinations.

The same concept applies to a **druidic element**.

## Mode

The primary mode for this element. See [mode] for mor details.

## Muls, Amps, Phss

Short for "multipliers," "amplitudes," and "phases." These three vectors are implicitly paired with an arbitrary frequency.

Together they define the "baseline" sound, or "principal" sound of the **druidic element**.

## Expr

`(amp_expr, freq_expr, phase_expr)`

This is a collection of three static envelopes to apply to each of the muls, amps, and phases (respectively). This set defines the principal glide, ADSR, and fuzz contours of the **druidic element**.

## Modders

`(amp_modulators, freq_modulators, phase_modulators)`

This is a complex collection of dynamic callbacks where your fantasies run free. 

Like **expr**, there is opportunity to mutate any of the three fundamental parameters (amplitude, frequency, and phase) per multiplier.

## hplp

This pair of filter banks describes the contour of allowed frequencies. 

The first element represents the **highpass** values. Entries in this vector describe minimum allowed frequency value.

The second element represents the **lowpass** values.
Entries in this vector describe maximum allowed frequency value.