+++
title = 'Contrib'
date = 2024-05-30T17:03:16-04:00
draft = true
math = true
+++


A contrib object describes your synth sound. 

We use it to specify the functional assignments for a single instrument, such as a percussive synth; a lead instrument; a bass; and more!

Note that we use the word "instrument" to refer to an acoustic or electronic sound maker, while "synthesizer" is always electronic. 

In this document we assume a sampling rate of 48,000 Hertz. 


# Parameters {#parameters}

Below we describe the 6 parameters required to define a Contrib. Each parameter description includes a list of available values.

1. [Mode](#mode) 
2. [Role](#role)
3. [Register](#register)
4. [Visibility](#visibility)
5. [Energy](#energy) 
6. [Presence](#presence) 

And animations with the [VEP Combination Parameters](#combination-parameters).

## Mode | *Primary element for the synthesizer* {#mode}

`mode`
- `melodic`
- `enharmonic`
- `noise`
___

While it is up to the preset designer to fully implement the elements of each druidic synth, the **Mode** parameter is a macro knob telling which set of harmonics is most important. 

**melodic** synthesizers are the most familiar sounds, containing integer-ratio harmonics to produce for example conventional overtone based waveforms square, sawtooth, or square waves. Each of these waveforms also has its undertone inverse, where we include frequencies based on the integer division of the fundamental (as opposed to integer multiplication). 

**enharmonic** synthesizers need to be handled carefully, as they can easily become noise synths if not well studied. Two good examples of enharmonic synths are "bell" sounds and "snare" sounds. Bells have well defined enharmonic ratios from a fundamental; and percussion instruments like snare drums have a specific set of nodal vibrations. These numbers can be easiliy generated in a gentime context for enharmonic synths.

**noise** synthesizers come in 5 flavors of energy density. Red and Pink noise have higher density at the lower end of the sound spectrum (toward 0 Hertz) while Blue and Violet noise have higher density at the higher end of the spectrum. Equal power noise (aka white noise) has even distribution and might be the sound you think of when you hear the word "noise". 

It is up to the preset to choose the best noise(s) for its intention. For instruments whose primary sound is the noise component, Blue and Equal noise demand more attention. Other instruments may choose to include a blip of Red or Pink noise to add weight without distracting from its own central body.

## Role | *Preset selector* {#role}

`role`
- `kick`
- `perc`
- `hats`
- `bass`
- `chords`
- `lead`
___ 

As of today, druidic synths must be assembled and packaged as "presets" in one of the above roles. Within these roles we can see that there are three percussive elements (**kick**, **perc**, **hats**) and three instrumental elements (**bass**, **chords**, **lead**).

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

For **melodic** synths, register determines whether the synthesizer will contain overtones, undertones, or a combination of both.

While register is more often used in the composition context for describing tones, it can also be applied as a general contrib parameter. When applied to a contrib it means this register is the lowest possible register that the instrument will perform (therefore, setting the maximum height for the synth). 

Naturally, high registers have shorter spectra and low registers are the tallest. 

Most music in the world is recorded with a sampling rate of 48,000 Hertz, providing a maximum reproducable frequency of 24,000 Hertz. The best consumer and commercial amplifiers can reproduce low frequencies down to about ~30 Hertz. We use these values to determine the set of applied safe register values:

This assumes that a register spans all frequencies from \(2^r\) to \(2^r+1\). 

So considering a low register 5, that means we have a baseline value of \(2^5 = 32\) and a maximum value of \(2^6 = 64\).
Most consumer grade sound systems, like your home speakers, probably can't reproduce the 32 hertz frequency. (Well, since it is *you* reading this, maybe it can). But most soundsytems **can** reproduce a 64 hertz tone.

### Music for Human Ears

Related to register, but not quite the same, is the Fletcher-Munsen curve. This curve describes how we humans typically perceive the loudness of different frequencies. Basically, if there are three groups of frequency bands ("low", "mid", and "high"), our ears are most sensitive to the frequencies in the upper mid / lower high range. It's likely that we have extra sensitivity here because these frequencies are the same as the vocal range. This means that by default, these frequencies in the upper mid / lower high range will sound louder to us (even if they have the same decibel value as a corresponding bass instrument).

Since we are synthesizing sound intended to be perceived as well-mixed, we can create a general EQ curve that will attenuate the frequencies to which our ears are most sensitive. Generally this includes most frequencies in registers 11 and 12.

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

**Hidden** parts are constrained to their octave. They may not be animated, and they may even have amplitude reduction. 


## Energy | *Knob for spectral height* {#energy}

`energy`
- `low`
- `medium`
- `high`
___

Additive synthesis is all about creating and managing the spectral content of an instrument's signal. 

The **energy** parameter is a simple method for describing how tall the synthesizer is. That is, the range of the synth's spectral reach.

As an extreme example consider a synth that plays a frequency at 100 Hertz. That means it is a pure sine wave osciallating 100 times per second. Sine waves are a special case and aren't that insteresting for this purpose. 

Let's turn it into a square by adding harmonics at 3, 5, and 7. So the total energy of the instrument comes from its component frequencies 100, 300, 500, 700 making it a **low energy** instrument. 

But that's not the entire range of available harmonics for this square! We can make it even taller by choosing to add more harmonics. The tallest version of this synth has all frequencies up to the Nyquist Frequency. E.g. 100, 300, 500, 700, 900, 1100, 1300, 1500, 1700 ... 19000, 21000, 23000. This is now a **high energy** instrument.

This is a very tall instrument, occupying a lot of our spectral canvas and leaving less room for other instruments to be seen. 

To fix this, now let's make this synth shorter (while still maintaining a square wave). 

Let's impose an artificial limit of 15 harmonics for the synth (e.g. frequencies 100, 300, 500, 700, 900, 1100, 1300, 1500). This is still a square wave, though with literally less energy than the taller version above but also with more energy than our simple 7 harmonic example. Thus, this 15 harmonic version of the square has **medium energy**.

Energy is also determined by the instrument's overall register. A flute is almost always going to be lower energy than  a bass.


## Presence | *Knob for amplitude ADSR* {#presence}

`presence`
- `staccatto`
- `legato`
- `tenuto`
___

Managing amplitude is the most straightforward way to boost or kill a signal. It's the highest level control we have for all instruments and is the original interpretation of "dynamics" in music, adding engagment through crescendo and descrescendo. 

While long form contours like crescendo and descrescendo are a matter of phrasing, we can use the same idea at a much smaller scale: The noteevent. 

Some instruments like the pipe organ or violin can play forever. Others are inherently finite per noteevent like the guitar or piano.

This is the premise of **presence**. It's a simple way to describe the synthesizer's allowed duration. 


## Combination Parameters VEP {#combination-parameters}

Three of the above parameters combine to produce animation effects. 
They are **visibility**, **energy**, and **presence**.  

___
# 
### Visibility + Energy
#
**Visible** and **foreground** contribs activate amplitude modulation. 

Animating amplitude means widening the range of overall amp contours. As an example, rather than having a constant pluck decay, the synthesizer may also include longer held tones as well.

#
### Visibility + Presence
#

**High** or **medium energy** is required to activate bandpass animation. 

Animating Bandpass animation means adding contour to the highpass or lowpass filters for a contrib. 

**Visible** contribs animate on the lowpass filter, while **foreground** contribs animate on the highpass filter. The idea here is that the lowpass animation generally retains the fundamental while the highpass filter may temporarily remove the fundamental. 

The more a part indicates a clear fundamental, the more visible it becomes to our ears.

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
