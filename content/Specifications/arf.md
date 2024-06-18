+++
title = 'Arf'
date = 2024-05-30T17:03:16-04:00
math = true
+++




## Preface: preset 

This article discusses "Arf parameters" and "druidic presets" from a developer's perspective. 

Arf parameters serve as inputs to a druid preset, which are complex objects that define the relative contribution of functions of k, t, and d (representing the harmonic number, position in the note event, and note event length in cycles).


# Parameters {#parameters}

These are the parameters and their options. The related subsection describes the parameter in greater detail.

1. [Mode](#mode) `melodic` | `enharmonic` | `noise`
2. [Role](#role) `kick` | `perc` | `hats` | `bass` | `chords` | `lead`
3. [Register](#register) `4` | `5` | `6` | `7` | `8` | `9` | `10` | `11` | `12` | `13`
4. [Visibility](#visibility) `visible` | `foreground` | `background` | `hidden`
5. [Energy](#energy) `low` | `medium` | `high`
6. [Presence](#presence) `staccato` | `legato` | `tenuto`


Some of these parameters are also applied to synthesizer animation. See [VEP Combination Parameters](#combination-parameters).

## Mode | *Primary element for the preset* {#mode}

`mode`
- `melodic`
- `enharmonic`
- `noise`
___

The **mode** parameter indicates which set of harmonics is prioritized by the preset designer.

#### Melodic
**melodic** synthesizers are the most familiar sounds. 

These synthesizers contain integer-ratio harmonics from a given fundamental frequency. We use Fourier series representations to produce the conventional overtone waveforms of square, triangle, or sawtooth using multiplication. These waveforms also have an undertone inverse, using division instead of multiplication. 

Harmonic frequency distortion, when |g(f)| < 0.005, enriches the sound. 

An example of this kind of distortion is how sound travels through the wooden body of a piano or guitar, interacting with transverse waves.

#### Enharmonic
Natural **enharmonic** sound can be found in mechanical or industrial settings. Think of dropping a steel fork on a cold cement floor. The sound of metal striking cement activates a complex collection of frequencies through the steel body and tines of the fork.

Two common **enharmonic** sounds are "bell" and "snare." Bells have well-defined enharmonic ratios from a fundamental, and percussion with a circular drumhead like snare, tom tom, or timpani can be modeled by the modes of membrane vibration. 

#### Noise
**noise** synthesizers come in 5 flavors of energy density. Red and Pink noise have higher density at the lower end of the sound spectrum (toward 0 Hertz) while Blue and Violet noise have higher density at the higher end of the spectrum. Equal power noise (aka white noise) has even distribution and might be the sound you think of when you hear the word "noise." 

The druidic preset selects the appropriate noise components based on the **energy** and **presence** parameters.

## Role | *druidic preset selector* {#role}

`role` values:
- `kick`
- `perc`
- `hats`
- `bass`
- `chords`
- `lead`
___ 

Within these roles we can see that there are three percussive elements (**kick**, **perc**, **hats**) and three instrumental elements (**bass**, **chords**, **lead**).

Design druidic presets based on these guidelines:

- **Kick** and **bass** roles are tallest, associated with low registers.
- **Hats** role is shortest, associated with high registers.
- **Bass** and **lead** are monophonic, while **chords** are polyphonic.
- Percussive elements (**kick**, **perc**, **hats**) generally have staccato presence, while instrumental elements (**bass**, **chords**, **lead**) can have any presence.
- Percussive elements are less likely to have a **melodic** mode, whereas instrumental elements are more likely.


**chords** is a special case because it is the only instrument designed generally for polyphony. As such, it is often shorter (with respect to spectral content) because it is expected to contain more simultaneous notes, and therefore more overall energy.

**lead** is also a special case in that it is designed with high visibility and energy, regardless of register.

## Register | *Vertical placement in the spectrogram* {#register}

`register`
- `4`
- `5`
- `6`
- `7`
- `8`
- `9`
- `10`
- `11`
- `12`
- `13`
___

A **register** covers all frequencies from \(2^r\) to \(2^r+1\), determining the lowest and highest frequencies an instrument can produce. 

For **melodic** synths, register determines whether the synthesizer will contain overtones, undertones, or a combination of both.

The provided **register** is the lowest octave where the instrument will perform, setting the maximum spectral height for the synthesizer.

High **registers** (like 13 or 14) have shorter spectra, while low **registers** (like 4 or 5) are taller.

Most music is recorded with a sampling rate of 48,000 samples per second providing a maximum reproducible frequency of 24,000 Hertz. The best consumer and commercial amplifiers can reproduce low frequencies down to about 30 Hertz. We use these values (30, 24,000) to determine the set of available **register** values. 



Considering a low register of **5**, that means we have a baseline value of \(2^5 = 32\) and a maximum value of \(2^6 = 64\).


Most consumer-grade sound systems, like your home speakers, probably can't reproduce the 32 Hertz frequency. (Well, since it is *you* reading this, maybe it can.) But most sound systems *can* reproduce a 64 Hertz frequency. 

When you do the same calculation at **register 4** you'll find that **register 4** includes some frequencies that we might not be able to reproduce. 

This explains why we don't find **register** options for *1*, *2*, *3*, *4*, or anything higher than **13**. 

<!-- ### Music for Human Ears -->
<!-- 
Related to **register**, but not quite the same, is the Fletcher-Munsen curve. This curve describes how we humans typically perceive the loudness of different frequencies. Basically, if there are three groups of frequency bands ("low", "mid", and "high"), our ears are most sensitive to the frequencies in the upper mid / lower high range. It's likely that we have extra sensitivity here because these frequencies are the same as the vocal range. This means that by default, these frequencies in the upper mid / lower high range will sound louder to us (even if they have the same decibel value as a corresponding bass instrument).

Since we are synthesizing sound intended to be perceived as well-mixed, we can create a general EQ curve that will attenuate the frequencies to which our ears are most sensitive. Generally this includes most frequencies in registers 11 and 12.
 -->

## Visibility | *Knob for bandpass filter* {#visibility}

`visibility`
- `visible`
- `foreground`
- `background`
- `hidden`

*Note* that **visibility** is the name of the parameter, which includes an option **visible**.
___

Not all instruments are presented equally. 

Some are featured front and center, while others provide a supporting role. 
Without synthesis, the primary way we have to manage the presentation of a part is by playing it more softly or loudly, or run it through an analog filter.

Applying a gentime frequency filter to our additive synthesizer provides an additional layer of **visibility** management.

A **visible** part has no filtering: We want to see it all.

Parts in the **foreground** may have an artificially shortened height or animation on bandpass.

**Background** parts have stricter bandpass settings and may have animation on bandpass.

**Hidden** parts are constrained to their octave. They may not have bandpass animation.


## Energy | *Knob for spectral height* {#energy}

`energy`
- `low`
- `medium`
- `high`
___

The goal of additive synthesis is to manage the spectral content of a synthesizer's signal. 

The **energy** parameter is a simple method for describing how tall the synthesizer is. That is, the range of the synthesizer's spectral reach.

#### Low

As an extreme example, consider a synth that plays a frequency at 100 Hertz. That means it is a pure sine wave of one component and has the lowest possible **energy** (which is covered by the **low** value).  

A more interesting waveform is the square wave.
Let's turn the sine wave into a square by adding harmonics at ratio 3, 5, and 7. Now the total energy of the instrument comes from its 7 components:

> frequencies: 100, 300, 500, 700 

making it a **low energy** instrument. 

You might be wondering why stop at 7? Exactly! That's not the entire range of available harmonics for this square! We can make it even taller by choosing to add more harmonics. 

#### High

The tallest version of this synth has all odd-numbered multiples of the fundamental (100) up to the Nyquist Frequency (24,000). 
> frequencies: 100, 300, 500, 700, 900, 1100, 1300, 1500, 1700 ... 19000, 21000, 23000. 

This is now a **high energy** instrument.

This is a very tall instrument, occupying a lot of our spectral canvas and leaving less room for other instruments to be seen. 

To address this, let's make this synth shorter while still maintaining a square wave.

#### Medium

Let's impose an artificial limit of 15 harmonics for the synth:
>  frequencies: 100, 300, 500, 700, 900, 1100, 1300, 1500

This is still a square wave. It has less energy than the taller version above, but also has more energy than our earlier 7 harmonic example. Thus, this 15 harmonic version of the square has **medium energy**.

**Energy** is also determined by the Arf's **register**. A flute (**register 10**) is almost always going to have less computed energy than a bass (**register 6**).


## Presence | *Knob for amplitude ADSR* {#presence}

`presence`
- `staccato`
- `legato`
- `tenuto`
___

Managing amplitude is the most straightforward way to boost or kill a signal. 

It's the highest level control we have for all instruments and is the original interpretation of "dynamics" in music: Adding interest through crescendo and decrescendo. 

While long-form contours like crescendo and decrescendo are a matter of phrasing, we can use the same idea at a much smaller scale: The note event. 

Some instruments like the pipe organ or violin can play forever. A note event may last the entire composition for these instruments. Other instruments are inherently finite, like the pluck of a guitar or strike of a piano.

This is the premise of **presence**. It's a simple way to describe the synthesizer's allowed duration. 

- **Staccato** sounds are finite and never occupy the entire duration of the note event. They are short or plucky.
- **Tenuto** events have a hearty body, but do not fill the entire duration. Instead, they fade out or exit at 50-80% completion.
- **Legato** events always fill the entire duration, with the signal usually fading away at the tail, unlike **staccato** and **tenuto** which fade earlier.

An *important observation* is that none of the note events extend beyond the duration. This is *contrary* to the popular concept of "release" in traditional ADSR.

With druidic synthesis, you are guaranteed that the body of sound rendered will produce a sample for the exact duration requested. The tail end of the waveform may include either silence or a fading signal. 


## Combination Parameters VEP {#combination-parameters}

Three of the above parameters combine to produce animation effects. 
They are **visibility**, **energy**, and **presence**.  

___
# 
### Visibility + Energy
#
**Visible** and **foreground** arfs with **medium** or **high** **energy** activate amplitude modulation. 

Animating amplitude means expanding the range of amplitude contours. As an example, rather than having a constant pluck decay, the synthesizer may also include longer-held tones.

#
### Visibility + Presence
#

**Legato** or **tenuto energy** is required to activate bandpass animation. 

Bandpass animation is adding contour to the highpass or lowpass filters for a note event. 

**Visible** arfs animate on the lowpass filter, while **foreground** arfs animate on the highpass filter. 

For *overtones*, 
- The lowpass filter animation via **visible** retains its fundamental, adding motion without affecting perceived fundamental.
- The highpass filter animation via **foreground** removes its fundamental, adding motion and weakening the signal.

For *undertones*, the filter banks are reversed. 

- Highpass filter animation via **visible** retains the fundamental, adding motion without affecting perceived fundamental.
- Lowpass filter animation via **foreground** removes the fundamental, adding motion and weakening the signal.




#
### Energy + Presence
#

This combo has two specific cases for handling "sidechain compression" (also known as "ducking"):

- **High energy** and **staccato presence** describes a part that should be the source of ducking.
- **Low energy** and **legato presence** describes a part that should be a target of ducking.

For a conventional example, consider a dance track where the kick drum controls dynamic compression for the ambient aspects of the track. 

- The kick has a **high energy** and **staccato presence** value.
- The pad synth has a **low energy** and **tenuto presence** value.

Therefore, the kick drum has no dynamic attenuation while the pad reduces its amplitude when the kick is performing.
