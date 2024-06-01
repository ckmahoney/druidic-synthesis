+++
title = 'Arf'
date = 2024-05-30T17:03:16-04:00
draft = true
math = true
+++


An Arf object describes your synth sound. 

{{< audio "demo-sawtooth" >}}

We use it to specify the functional assignments for a single instrument, such as a percussive synth; a lead instrument; a bass; and more!

Note that we use the word "instrument" to refer to an acoustic or electronic sound maker, while "synthesizer" is always electronic. 

In this document we assume an audio sampling rate of 48,000 samples per second. 


## Preface: preset 

This article talks about both "Arf parameters" and "presets" from a developer's point of view. 

These Arf parameters are input to a Druid, aka preset. Druids are complex objects that define functions of k, t, and d (representing the harmonic number, position in the noteevent, and notevent length in cycles). 



# Parameters {#parameters}

Below we describe the 6 parameters required to define an Arf. Each parameter description includes a list of available values.

1. [Mode](#mode) 
2. [Role](#role)
3. [Register](#register)
4. [Visibility](#visibility)
5. [Energy](#energy) 
6. [Presence](#presence) 

Some of these parameters are also applied to synthesizer animation. See [VEP Combination Parameters](#combination-parameters).

## Mode | *Primary element for the synthesizer* {#mode}

`mode`
- `melodic`
- `enharmonic`
- `noise`
___

While it is up to the preset designer to fully implement the elements of each druidic synth, the **Mode** parameter is a macro knob telling which set of harmonics is most important. 

#### Melodic
**melodic** synthesizers are the most familiar sounds. 

These synthesizers contain integer-ratio harmonics from a given fundamental frequency. We use Fourier series representations to produce the conventional overtone waveforms of square, triangle, or sawtooth using multiplication. These waveforms also also have an undertone inverse, using division instead of multiplication. 

Harmonic frequency distortion as a function of frequency g(f) enriches the sound for |g(f)| < 0.005. 

A natural example of harmonic frequency distortion is transverse wave propogation through the wooden body of a piano or guitar.

#### Enharmonic
Natural **enharmonic** sound can be found in mechanical or industratial locations. Think of dropping a steel fork on a cold cement floor. The sound of metal striking cement activates a complex collection of frequencies through the steel body and tines of the fork.

Two common **enharmonic** sounds are "bell" and "snare." Bells have well defined enharmonic ratios from a fundamental; and percussion with a circular drumhead like snare, tom tom, or timpani can be modelled by the modes of membrane vibraiton. 

#### Noise
**noise** synthesizers come in 5 flavors of energy density. Red and Pink noise have higher density at the lower end of the sound spectrum (toward 0 Hertz) while Blue and Violet noise have higher density at the higher end of the spectrum. Equal power noise (aka white noise) has even distribution and might be the sound you think of when you hear the word "noise." 

The druidic preset selects which noise components are most appriorate for the given **energy** and **presence**.


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

Generally speaking you should design druidic presets on these guidelines:

- The tallest roles are **kick** and **bass**, because these are associated with low registers
- The shortest role is **hats**  because hi hat sounds are associated with the highest registers
- **bass** and **lead** are monophonic, while **chords** is polyphonic.
- Percussive elements **kick**, **perc**, and **hats** generally have Staccatto presence, while **bass**, **chords**, and **lead** use any presence.
- Percussive elements are least likely to have a **melodic** mode while instrumental elements are most likely to have a **melodic** mode.

**chords** is a special case because it is the only instrument designed generally for polyphony. As such, it is often shorter because it is expected to contain more simultaneous notes (and therefore more overall energy).

**lead** is also a special case in that it is designed for high visibility and energy, regardless of register.

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

A **register** spans all frequencies from \(2^r\) to \(2^r+1\). 

For **melodic** synths, register determines whether the synthesizer will contain overtones, undertones, or a combination of both.

The provided **register** is the lowest octave where the instrument will perform. This implicitly sets the maximum spectral height for the synthesizer.

Naturally, high **registers** (like 13 or 14) have shorter spectra and low **registers** (like 4 or 5) are the tallest. 

Most music in the world is recorded with a sampling rate of 48,000 Hertz, providing a maximum reproducable frequency of 24,000 Hertz. The best consumer and commercial amplifiers can reproduce low frequencies down to about ~30 Hertz. We use these values to determine the set of applied safe register values, which explains why we don't find options for *1*, *2*, *3*, *4*, or anything higher than **13**. 


So considering a low register **5**, that means we have a baseline value of \(2^5 = 32\) and a maximum value of \(2^6 = 64\).
Most consumer grade sound systems, like your home speakers, probably can't reproduce the 32 Hertz frequency. (Well, since it is *you* reading this, maybe it can). But most soundsytems *can* reproduce a 64 Hertz frequency.

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
___

Not all instruments are presented equally. Some are featured front and center, while others provide a supporting role. 
Without synthesis, the primary mechanism we have to manage the presence of a part is by playing it more softly or loudly; or run it through an analog filter.

Applying a gentime frequency filter to our additive synthesizer provides an additional layer of focal point management.

A **visible** part has no filtering: We want to see it all.

Parts in the **foreground** may have some additional cutoff applied, or animation on their bandpass.

**Background** parts have stricter bandpass settings and may have animation on bandpass.

**Hidden** parts are constrained to their octave. They may not be animated.


## Energy | *Knob for spectral height* {#energy}

`energy`
- `low`
- `medium`
- `high`
___

The primary goal of additive synthesis is to create and manage the spectral content of an synthesizer's signal. 

The **energy** parameter is a simple method for describing how tall the synthesizer is. That is, the range of the synthesizer's spectral reach.

As an extreme example consider a synth that plays a frequency at 100 Hertz. That means it is a pure sine wave of 1 component and has the lowest possible **energy** (which is coverd by the **low** value).  

A more interesting waveform is the square wave.
Let's turn the sine wave into a square by adding harmonics at ratio 3, 5, and 7. Now the total energy of the instrument comes from its 7 component 

> frequencies: 100, 300, 500, 700 

making it a **low energy** instrument. 

You might be wondering why stop at 7? Exactly! That's not the entire range of available harmonics for this square! We can make it even taller by choosing to add more harmonics. 

The tallest version of this synth has all odd numbered multiples of the fundamental (100) up to the Nyquist Frequency (24000). 
> frequencies: 100, 300, 500, 700, 900, 1100, 1300, 1500, 1700 ... 19000, 21000, 23000. 

This is now a **high energy** instrument.

This is a very tall instrument, occupying a lot of our spectral canvas and leaving less room for other instruments to be seen. 

To fix this, now let's make this synth shorter (while still maintaining a square wave). 

Let's impose an artificial limit of 15 harmonics for the synth
>  frequencies: 100, 300, 500, 700, 900, 1100, 1300, 1500

This is still a square wave, though with literally less energy than the taller version above; but also with more energy than our earlier 7 harmonic example. Thus, this 15 harmonic version of the square has **medium energy**.

**Energy** is also determined by the Arf's **register**. A flute (**register 10**) is almost always going to be have computed energy than a bass (**register 6**).


## Presence | *Knob for amplitude ADSR* {#presence}

`presence`
- `staccatto`
- `legato`
- `tenuto`
___

Managing amplitude is the most straightforward way to boost or kill a signal. 

It's the highest level control we have for all instruments and is the original interpretation of "dynamics" in music: Adding engagment through crescendo and descrescendo. 

While long form contours like crescendo and descrescendo are a matter of phrasing, we can use the same idea at a much smaller scale: The noteevent. 

Some instruments like the pipe organ or violin can play forever. A noteevent may last the entire composition for these instruments. Other instruments are inherently finite, like the pluck of a guitar or strike of a piano.

This is the premise of **presence**. It's a simple way to describe the synthesizer's allowed duration. 

**Staccatto** sounds are unambiguously finite. They never occupy the entire duration of the noteevent's length.

**Tenuto** events tend toward note completion, but also do not occupy the entire noteevent's length. 

**Legato** events always fill the entire noteevent's duration. It is common for the majority of the signal to fade away at the tail. This is remarkably different than **staccattoo** and **tenuto** which have already "exited" by this point in time.

An *important observation* is that none of the note events extend beyond the duration. This is *contrary* to a popular concept of "release" in traditional ADSR.

With druid synthesis you are guaranteed that the body of the sound is rendered for the exact duration requested.


## Combination Parameters VEP {#combination-parameters}

Three of the above parameters combine to produce animation effects. 
They are **visibility**, **energy**, and **presence**.  

___
# 
### Visibility + Energy
#
**Visible** and **foreground** arfs with **medium** or **high** **energy** activate amplitude modulation. 

Animating amplitude means expanding the range amplitude contours. As an example, rather than having a constant pluck decay, the synthesizer may also include longer held tones as well.

#
### Visibility + Presence
#

**Legato** or **tenuto energy** is required to activate bandpass animation. 

Bandpass animation is adding contour to the highpass or lowpass filters for a noteevent. 

**Visible** arfs animate on the lowpass filter, while **foreground** arfs animate on the highpass filter. 

For overtones, 
1. The lowpass filter animation via **visible** retains its fundamental, adding motion without affecting perceived fundamental.
2. The highpass filter animation via **foreground** removes its fundamental, adding motion and weakining the signal.

For undertones, the filter banks are reversed. 

1. Highpass filter animation via **visible** retains the fundamental, adding motion without affecting perceived fundamental.
2. Lowpass filter animation via **foreground** removes the fundamental, adding motion and weakining the signal.




#
### Energy + Presence
#

This combo has two specific cases for handling "sidechain compression" (also known as "ducking"):

1. **High energy** and **staccatto presence** describes a part that should be the source of ducking.
2. **Low energy** and **legato presence** describes a part that should be a target of ducking.

For a conventional example, consider a dance track where the kick drum controls dynamic compression for the ambient aspects of the track. 

1. The kick has a **high energy** and **staccatto presence** value.
2. The pad synth has a **low energy** and **tenuto presence** value.

Therefore, the kick drum has no dynamic attenuation while the pad reduces its amplitude when the kick is performing.
