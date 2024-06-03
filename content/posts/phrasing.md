+++
title = 'Phrasing'
date = 2024-06-03T08:52:46-04:00
draft = true
+++

# Change over time

Phrasing describes a few topics. 

Let's make some definitions.

## Clarity


**Clarity** is how easy it is to understand what the content is supposed to be. Photography is an excellent point of comparison. Let's consider a few examples of a poor lighting photo, a photo with high contrast, and a "good" photo:
    - The dark and blurry picture is hard to make out the details
    - The extremeley high contrast image can be easy to identify the contents, but looks unnatural
    - The picture with natural lighting and good subject placement is no-brainer easy to understand
  
For sound, we can relate these pretty easily to our synthesis parameters **swarm**, **sway**, and **degredation.**
We can also choose to conceal parts of the signal by animating its **bandpass**.
Finally, we can make a signal more or less confusing by **shapeshifting** it.

### Amounts

We can put the amount of effect into one of five amounts and two categories:

    - tiny
    - small
    - medium
    - large
    - huge

### Categories
    - destructive
    - modulation

    - **swarm** is a **modulation** effect and relates to *blur* in photography. Other common musical words for **swarm** include *chorus* and *detune.* It is a **modulation** because we use dynamic phase modulation at gentime to produce changes in perceived pitch on a per-harmonic basis.
        - The sound of tiny **swarm** gives a lush, sheer or "shine" kind of effect and is an excellent vocal or lead enhancement.
        - The sound of small **swarm** is a typical *chorus* sound, adding depth to the sound while still keeping a clear focal point.
        - The sound of medium **swarm** is a typical *detune* sound, adding character to the sound while blurring the focal point by a taseful amount.
        - The sound of large **swarm** is an experiemental effect, and is best applied in transitions or dramatic moments.
        - The sound of huge **swarm** is nearly noise as we lose sight of the original signal.

    - **sway** is a **modulation** effect and doesn't exactly have a photography counterpart. The closest might be "swirl" or "bend", where you stretch parts of the image and squish other parts. It is very similar to **swarm** in how it is applied; but uses different time-scale parameters. 
        - The sound of tiny **sway** adds extreme depth and sense of motion without being perceptable. 
        - The sound of small **sway** gives it a faded, or vintage feeling and is common in pop, hip-hop and rock music.
        - The sound of medium **sway** is more pronounced and found in trip-hop, psycedellic, and EDM.
        - The sound of large **sway** is woozy over-the-top carnival. It's more common in hardcore or experimental styles.
        - The sound of huge **sway** is experimental and can be found in hyperpop.

    - **degredation** describes the resolution of the overall signal. This is a **destructive** effect, meaning it is not reversable. In large amounts, it can make the input signal feel more "hollow" or "out of tune" because inner harmonics have been removed. This audio effect is unique to additive synthesis and might not have a point of comparison! The most similar concept is *bit-crushing*, where we drop samples in the *time-domain* to create a distortion effect. For **degredation**, we drop harmonics in the *frequency domain.* 
        - The sound of tiny **degredation** adds a little bit of character to the sound and is an excellent method for adding variety to otherwise static content. 
        - The sound of small **degredation** might be noticable but does not remarkably affect the carrier signal. 
        - The sound of medium **degredation** will remove character from the signal. A sawtooth wave might still crunch but lose its bite.
        - The sound of large **degredation** will remove a lot of character from the signal. A sawtooth wave might become a square wave.
        - The sound of huge **degredation** will drastically affect the sound. It might turn into a plain sine wave.

## Direction

Five principal modes of motion help us describe how we get from point A to point B. 
    - **constant**
    - **increasing**
    - **decreasing**
    - **peaking**
    - **valleying**

## Bandpass

This shares the name of the Arf parameter which it targets: **bandpass.** Bandpass phrasing is most often applied to transitions, entry points, and exits of a composition. 

## Shapeshift

This is a unique parameter for additive synthesis! 

Shapeshift offers the ability to "morph" from one sound into another sound. There's three main options:

    - **constant** which uses the same sound for the lifetime of its melody
    - **two-tone** which begins with tone A and ends on tone B
    - **polytone** which begins with tone A and ends on ...Z

