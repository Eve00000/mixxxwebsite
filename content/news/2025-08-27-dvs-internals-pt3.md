title: "How Does Timecode Vinyl Actually Work? (Pt. 3)"
authors: Jan Claußen
tags: traktor, timecode, dvs, vinyl control
math: yes
date: 2025-08-27 11:54:12

Since its release in 2011, the **Traktor Control Vinyl MK2** has sparked
curiosity among digital DJs and audio developers alike. Its timecode format
stands apart from Serato’s, which we explored in the previous posts. With the
MK2 system, Native Instruments introduced a more advanced timecode that boosts
resolution and accuracy by applying advanced cryptographic techniques.

In this post, we’ll break down how it works at a basic level and how Mixxx is processing the signal.

---

## Recap: How Serato Timecode Works

Serato’s timecode is built around a
[Linear Feedback Shift Register](https://en.wikipedia.org/wiki/Linear-feedback_shift_register),
modulated onto a 1 kHz carrier using
[amplitude modulation (AM)](https://en.wikipedia.org/wiki/Amplitude_modulation) - a legacy
technique from radio transmission.

The demodulation process is relatively simple: when one stereo channel crosses
the x-axis, the other hits a peak. If that peak exceeds a certain threshold,
the system reads it as a **1**, if not, it’s a **0**.

![Serato Timecode Signal]({static}/images/news/serato-control-cd.svg)

We covered this in more detail in
[DVS Internals Pt. 1]({filename}/news/2021-11-21-dvs-internals-pt1.md)
and [Pt. 2]({filename}/news/2021-12-22-dvs-internals-pt2.md).

---

## The Traktor MK2 Signal

Below is a signal that resembles what you’ll find on the Traktor MK2
Control vinyl/CD, which has been specifically generated for this blog post by
using a [Raised-Cosine Filter](https://de.wikipedia.org/wiki/Raised-Cosine-Filter) to
modulate a random sequence onto the carrier.

The carrier wave operates at **2500 Hz**, a significant increase from Serato’s
**1000 Hz**.

> **Advantage:** The higher carrier frequency allows for 2.5× greater resolution.

![Offset-modulated Signal]({static}/images/news/traktor-mk2-signal.svg)

Upon inspection, this waveform clearly doesn’t use amplitude modulation - the
amplitude remains constant. Instead, it appears to be **offset-modulated**,
where the signal is shifted vertically from the x-axis. This is a non-standard
technique not commonly used in typical modulation schemes.

On the original vinyl version (not shown here due to copyright), the offset
can be so large that the signal floats entirely above the x-axis for multiple
cycles - making zero-crossing detection impossible.

Even when that doesn’t happen, the offset causes the time interval $\Delta t$
between zero-crossings to become irregular, introducing audible pitch flutter.

To decode the signal, we must solve:

1. How to filter the signal to enable pitch detection
2. How to demodulate this non-standard modulation
3. How to decipher the code that is modulated onto the carrier

---

## Pitch Detection

> **Note:** If you're unfamiliar with pitch detection in DVS systems, revisit
> [DVS Internals Pt. 1]({filename}/news/2021-11-21-dvs-internals-pt1.md).

Our goal is to produce a signal that oscillates evenly around the x-axis. This
filtered waveform can then be processed by the existing pitch detection algorithm.

A simple discrete derivative operation achieves this:

$$
y[n] = x[n] - x[n-1] \tag{1}
$$

$\text{where:}$<br>
$\text{- x[n]: Input sample}$<br>
$\text{- x[n-1]: Delayed input sample}$<br>
$\text{- y[n]: Difference of both values}$<br>
<br>

When applied to the offset-modulated signal, we get:

![Traktor MK2 signal with derivative]({static}/images/news/traktor-mk2-signal-with-derivative.svg)

The resulting waveform oscillates cleanly around zero, which is ideal for
analysis. It also makes it easier to pinpoint the half-cycle peaks needed for
bit detection.

---

## Demodulation Techniques

To extract bits from the signal, we detect the zero-crossings and sample the
amplitude of the sinusoid at those moments.

![Traktor MK2 signal with
zero-crossings]({static}/images/news/traktor-mk2-signal-with-zero-crossings.svg)

You may notice that the derivative’s zero-crossings don’t align perfectly with
the original peaks. That’s due to a delay introduced by the filter. Smoothing
the signal first, then compensating for the delay (e.g., by selecting
$x[n-3]$), yields better results.

For greater accuracy, one could analyze the phase response $\phi(\omega)$,
which shows how filter delay varies with input frequency - but for this use case,
a fixed delay works well enough.

The filtered signal can cross the x-axis in two directions-positive to
negative or vice versa. Based on the direction, we determine which half-cycle
contains the encoded bit. In this example we only use the upper half-cycle for
decoding, but you can also decode the signal from the lower half-cycle.

![Traktor MK2 signal with readings]({static}/images/news/traktor-mk2-signal-with-readings.svg)

Demodulation is then as simple as applying a threshold: amplitudes above it
are **1**, and below it are **0**.

![Traktor MK2 signal with timecodes]({static}/images/news/traktor-mk2-signal-with-timecodes.svg)

On actual vinyl, the physical behavior of the needle causes the offset to
decay over time, because the needle slowly drifts back to the middle. This decay complicates bit extraction.

To compensate, we analyze the **slope** between subsequent readings by
reusing the derivative equation in $\text{(1)}$.

$$slope[n] = reading[n] - reading[n-1]$$

$\text{where:}$<br>
$\text{- x[n]: Current reading}$<br>
$\text{- x[n-1]: Last reading}$<br>
$\text{- y[n]: Difference of both values}$<br>
<br>

We then decode a positive slope to toggle the bit to **1** and a negative
slope to toggle the bit to **0**.

![Traktor MK2 signal with indicated slope]({static}/images/news/traktor-mk2-signal-with-slope.png)

This method helps isolate the encoded signal from the floating zero line
caused by mechanical drift.

---

## The Code

> **Note:** A deeper explanation of LFSRs can be found in [DVS Internals Pt. 2]({filename}/news/2021-12-22-dvs-internals-pt2.md)

Interestingly, the Traktor MK2 system also uses a [Linear Feedback Shift Register](https://en.wikipedia.org/wiki/Linear-feedback_shift_register) - but with different properties. While Serato’s LFSR has a
**20-bit** length, Traktor’s uses a **110-bit** register with a minimum
run length of two symbols. The generator polynomial of the the LFSR was found
by using the [Berlekamp-Massey algorithm](https://en.wikipedia.org/wiki/Berlekamp%E2%80%93Massey_algorithm).

The number of unique states an LFSR can generate is:

$$n_{max} = 2^m -1$$

Hence for the Serato timecode

$$n_{serato} = 2^{20} -1 = 1\,048\,575$$

and for the Traktor MK2 timecode

$$n_{mk2} = 2^{110} -1 = 1.298 \cdot 10^{33} = 1\,298\,074\,214\,633\,706\,907\,132\,624\,082\,305\,023$$

That’s an astronomically high number-far beyond what’s required for this
application.

But how many states are actually needed?  With a 2500 Hz carrier, you get 2500 bits per second.<br>

For 12 minutes of timecode:

$$12 \text{ min} \cdot 60  = 720 \text{ s}$$
$$720 \text{ s} \cdot 2500 \text{ states/s} = 1\,800\,000 \text{ states}$$

which exceeds the maximum state range of Serato’s 20-bit LFSR by far.

However, a downside appears: each 110-bit state must be stored in 128 bits (4
× 32-bit integers).

So for the A-side with 12 minutes:

$$1\,800\,000 \text{ states} \cdot 128 \text{ bit} = 230\,400\,000 \text{ bit} = 28\,800\,000 \text{ byte} = 27.46 \text{ MB}$$

And for a 25-minute CD:

$$4\,500\,000 \text{ states} \cdot 128 \text{ bit} = 576\,000\,000 \text{ bit} = 72\,000\,000 \text{ byte} = 68.66 \text{ MB}$$

> **Disadvantage:** The memory footprint is large-even a single side of timecode can exceed 27 MB.

This makes storing a full lookup table impractical in production software.

It's important to point out that the current implementation is naive, because it treats the
Traktor MK2 code as if it were Serato code. Since Mark Hills designed the xwax
library, which is used by vinyl control in Mixxx, for exactly this style of
timecode, changes would have to be made to make the decoder more modular.

Nonetheless, the current technique works and it represents the current state
of the decoder in Mixxx.

---

## Conclusion

Fortunately, there are mathematical methods to reduce the memory requirements.
This requires diving deeper into the crypthographic theory.

First tests show that this can possibly be achieved by applying a fixed tap
pattern (e.g. every 5th bit) to a 110-bit LFSR window - a form of structured
decimation or undersampling. This collapses the sequence into a 22-bit
[Gold code](https://en.wikipedia.org/wiki/Gold_code), whose two sequences alternate.
The implementation of this technique is far more complex and not completed
yet.

We’ll explore those strategies in the next part of this series.
