title: GSOC 2025 Work Product - Resampling Options for Mixxx
authors: Armaan Chowfin
tags: gsoc, gsoc-2025
comments: yes
date: 2025-08-04 17:42:53

Presenting the latest additions to Mixxx:  
**[issue#9328](https://github.com/mixxxdj/mixxx/issues/9328): Additional interpolation options for scratching**  
**[issue#10611](https://github.com/mixxxdj/mixxx/issues/10611): Custom output samplerates for recording and broadcast**.

The scratching engine now supports a sinc-based resampler with three quality settings, and the original linear resampler is now twice as fast. Additionally, outbound audio is resampled before encoding for recording or streaming. This can improve compatibility with limited or fixed-rate sound cards.

#### Audio Engineering Keywords
**Time and Frequency Domain:**  
**Impulse Response:**



#### Introduction
Mixxx uses the `SoundTouch` and `RubberBand` time-stretching libraries for resampling during a keyLock operation. However, these libraries are unsuitable for scratching due to the fast changing tempo and pitch. Currently a faster, handcrafted linear interpolation algorithm is used - but there have been reports of suboptimal audio quality.  

Digital Signal Processing (DSP) theory tells us that linear interpolation is not ideal, and that a sinc-based resampler will always return interpolated values identical to the original analog signal, under certain theoretical constraints. However, practical implementations of sinc resampling are computationally heavy and generally unsuitable for low-latency realtime software such as Mixxx. Therefore,

* One objective of this GSoC project was to explore the feasibility of using sinc interpolation for scratching. To this end, the `libsamplerate` and `libzita` resample latencies were evaluated.
* Another focus was to investigate and improve the performance of the current linear resampler. Here, we observed that the `libsamplerate` linear interpolator outperformed our own, *reducing per-buffer resample latency from 20µs to 10µs.*

#### Theoretical Foundations
Although of the work during GSOC involved integrating external C and C++ libraries into Mixxx, understanding the interpolation algorithms requires an overview of some foundational ideas in DSP. By the end of this section, I hope to provide a satisfactory answer to why sinc interpolation provides higher quality resampling.

##### Analog-to-Digital and Digital-to-Analog
<div style="border: 1px solid white; padding: 16px; margin-bottom: 24px; border-radius: 6px; line-height: 1.6;">
  <strong>Summary:</strong>
  <ul style="margin-top: 8px; margin-bottom: 0; padding-left: 16px;">
    <li>Every digital signal has a time-domain representation (sample sequence) and frequency-domain representation (replicated spectrum).
    <li>Filters implemented in hardware are called analog filters, those implemented in software are called digital filters.
    <li>In theory, ADC performs periodic sampling by multiplying an analog input signal with a unit impulse train.
    <li>In theory, DAC performs signal reconstruction by applying an analog Low-Pass Filter to the sample sequence. This is implemented via a continuous-time convolution</li>
    <li>In theory, DAC signal reconstruction will be perfect if the original analog signal was Low-Pass filtered to below Nyquist Frequency before ADC sampling.
  </ul>
</div>

An analog signal is a continuous-time, continuous-amplitude function. The **Fourier theorem** states that any analog signal can be viewed as the infinite sum of time-domain sinusoid components of varying frequency and amplitude. The set of components gives the *spectrum* of the signal. A signal's continuous spectrum is generally represented by an Amplitude-Frequency graph.

Its digital counterpart is a discrete-time sequence formed by **sampling** the analog amplitude at uniform time intervals, called sample-periods. In the time domain, sampling is represented as multiplying an analog signal with an impluse train. In practice, time-domain sampling is performed by electrical circuits in an Analog to Digital converter (ADC).

<div style="text-align: center; margin-bottom: 30px;">
  <h5 style="margin-bottom: 10px; font-size: 1.8rem; font-weight:bold;">Periodic Sampling in the Time Domain</h5>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="width: 53%;">
      <img src="{static}/images/news/mixxx-sampling-analog-digital.jpeg" alt="Periodic Sampling 1" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 1:</strong> Mathematical Impulse-Train Sampling</p>
    </div>
    <div style="width: 46%;">
      <img src="{static}/images/news/mixxx-sample-and-hold-circuit.png" alt="Periodic Sampling 2" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 2:</strong> ADC Sample-and-Hold Circuit</p>
    </div>
  </div>
</div>

In the frequency domain, *sampling with any frequency f<sub>s</sub> always creates replicas of the spectrum of the original signal, centered at integer multiples of f<sub>s</sub>*. In the figures below, the left-hand side polygon represents the assumed spectrum of the time-domain signal in Figure 1. Its highest frequency component is B hz, and the range `[-B, B]` is referred to as the **baseband** of the analog signal.

<div style="text-align: center; margin-bottom: 30px;">
  <h5 style="margin-bottom: 10px; font-size: 1.8rem; font-weight:bold;">Periodic Sampling in the Frequency Domain</h5>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="width: 49%;">
      <img src="{static}/images/news/mixxx-spectral-replication-no-overlap.png" alt="Periodic Sampling 1" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 3:</strong> Spectral replication without overlap.</p>
    </div>
    <div style="width: 49%;">
      <img src="{static}/images/news/mixxx-spectral-replication-with-overlap.png" alt="Periodic Sampling 2" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 4:</strong> Spectral replication with overlap</p>
    </div>
  </div>
</div>

The purpose of sampling an analog signal is to create a representation that can be stored and manipulated by digital computers. However, these digital representations are only valuable if they can eventually be perceived in the real world. The process of converting a sampled signal back into a continuous-time analog signal is called **signal reconstruction**. Depending on the relative values of B and f<sub>s</sub>, the spectral replicas created during sampling may overlap in the baseband, resulting in additive distortion. This phenomenon is termed **aliasing** and is perceived only on signal reconstruction.


<!-- A central challenge is that knowledge only of periodic amplitudes (i.e. samples) does not allow us to infer a unique frequency for the original signal. In other words, periodic sampling leads to ambiguity in the frequency domain. -->

<div style="text-align: center; margin-bottom: 30px;">
  <h5 style="margin-bottom: 10px; font-size: 1.8rem; font-weight: bold;">Aliasing in the Time Domain</h5>

  <div style="width: 70%; margin: 0 auto;">
    <img src="{static}/images/news/mixxx-time-domain-aliasing.jpeg" alt="aliasing in time domain" style="width: 100%; height: auto;">
    <p style="font-size: small; margin-top: 5px;">
      <strong>Figure 5:</strong> When a sine wave is sampled below the Nyquist rate, multiple analog sinusoids can fit the same set of samples. The figure shows two possible original signals that are consistent with the sampled data.
    </p>
  </div>
</div>

<!-- Reconstructing via LPF will only return the signal with spectral content within [-fs/2, fs/2] -->

In theory, signal reconstruction is performed by applying an analog low-pass filter to the input sample sequence, to eliminate the spectral replications that were created during sampling. This leaves only the spectral components in the baseband. If f<sub>s</sub> > 2B as in Figure 3, the baseband spectral component after sampling still represents the original spectrum. This criterion is formalized by the **Shannon-Nyquist sampling theorem**, which states that an analog signal must be sampled at at least twice the frequency (called Nyquist rate) of its highest-frequency component to ensure no aliasing upon signal reconstruction.


<div style="text-align: center; margin-bottom: 30px;">
  <h5 style="margin-bottom: 10px; font-size: 1.8rem; font-weight:bold;">Low Pass Filter in the Frequency Domain</h5>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="width: 50%;">
      <img src="{static}/images/news/mixxx-lpf-no-alias.png" alt="Periodic Sampling 1" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 6:</strong> Ideal Low pass (Brick Wall) filter, no aliasing. The reconstruction low pass filter in the DAC assumes that the input meets the Nyquist criterion.</p>
    </div>
    <div style="width: 50%;">
      <img src="{static}/images/news/mixxx-lpf-alias.png" alt="Periodic Sampling 2" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 7:</strong> Failure to meet Nyquist criterion results in aliasing.</p>
    </div>
  </div>
</div>

If f<sub>s</sub> < 2B as in Figure 4, implying that the original signal has some higher-than `Nyquist Rate/2` components, filtering would yield in a distorted baseband due to the "folding-back" of higher frequency sinusoid components. Therefore it is important to pass the analog signal through an analog low-pass filter before it is sampled by the ADC to restrict its frequency content.

The principle behind signal reconstruction is the **continuous-time convolution** process. Here, the fixed impulse response h(t) of a First-Order Hold (FOH) filter stays stationary while the sampled sequence x<sub>flipped</sub> slides over it.

<div style="text-align: center; margin-bottom: 30px;">
  <h5 style="margin-bottom: 10px; font-size: 1.8rem; font-weight:bold;">Low-Pass Filter in the Time Domain</h5>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="width: 90%;">
      <img src="{static}/images/news/mixxx-continuous-convolution.jpeg" alt="convolution" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 10:</strong> Signal Reconstruction  via continuous-time convolution.  At each time step nT, (where T is the sample-period) the sample x<sub>flipped</sub>(n) scales the triangular impulse response h(t-nT), and the reconstructed signal y(n) is the sum of all such shifted, scaled responses.
    </div>
  </div>
</div>

While the FOH filter approximates the analog signal, the reconstruction is not perfect. In fact, the impulse response for perfect reconstruction can be derived mathematically, and it *turns out to be an infinite impulse response in the shape of a sinc function.*

- When convolved with the input sequence, ideal low-pass filtering occurs: Spectral replicas are eliminated, and the analog time-domain signal is reconstructed.


---

##### Sample Rate Conversion
Key idea: Pretend the original analog signal is made of sinc pulses centered on the known samples. Software stores values of sinc kernel at a large (but finite) set of fractional points in (0,1). new sample values are calculated by finding the closest stored value to the theoretical prediction.
sinc interpolation: Take a snapshot of the convolution process that happens during signal reconstruction



While analog filters implement a continuous-time convolution of the input sequence, digital filters can only perform discrete-time convolution.

<div style="text-align: center; margin-bottom: 30px;">
  <h5 style="margin-bottom: 10px; font-size: 1.8rem; font-weight:bold;">Discrete-Time Convolution</h5>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="width: 72%;">
      <img src="{static}/images/news/mixxx-discrete-convolution.jpeg" alt="convolution" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 8:</strong> Convolving a 5-tap impulse response h(k) with a digital signal x(nT), where T is the sample-period. Outputs y(n) are obtained by sliding the input sequence over the stationary coefficients.
    </div>
    <div style="width: 28%;">
      <img src="{static}/images/news/mixxx-sinc-coeffs.jpeg" alt="sinc coeffs" style="width: 100%; height: auto;">
      <p style="font-size: small; margin-top: 5px;"><strong>Figure 9:</strong> 9-tap and 19-tap impulse responses of a lowpass filter. The sinc impulse response is convolved with an input sequence to generate a time-domain output sequence.</p>
    </div>
  </div>
</div>




















#### Background: Analog and Digital Audio
Vibrations of the surrounding air in turn cause the human eardrum to vibrate, and generate a continuous electrical signal. In audio-engineering terms, this electrical signal represents "analog audio", and our ear represents a (biological) "audio interface", i.e. a gateway for audio to enter or exit a processing system.

In the human ear, each cochlear hair cell is tuned to a specific frequency band and converts local mechanical vibrations into discrete neural spikes, encoding amplitude over time. This results in a time-series of electrical events that the brain interprets as sound.

##### Periodic Sampling and Encoding
While an analog signal is represented by its amplitude as continuous function in continuous time, the digital representation of that signal is a time series of amplitude values generated by noting the value of the analog signal at fixed, discrete time intervals.

*The process of generating a digital audio representation from an analog signal is termed **sampling***, and the length of the time interval is termed the "sample-period (its reciprocal - the **sampling rate** - is more commonly used while describing digital audio). Mathematically, sampling is represented as multiplying an analog signal with an impluse train in the time-domain.


In practice, when analog audio from a sound source enters a mic, it is converted to a continuous electrical signal. Finally, a component called the **ADC (Analog to Digital Converter)** records the electrical signal voltage at a fixed sampling rate such as 44.1KHz, 48Khz or 96Khz, to generate a series of amplitude values, i.e. *samples*. Each sample is stored as a fixed-precision floating point number.

The samples are then encoded to a standard digital format (ex. MP3, WAV, AAC, etc.) using well-known algorithms. This allows sampled audio, i.e. music records to be stored on digital hard-drives. The sample sequences are treated as logical *frames* for multichannel audio. An audio frame is an array containing `k` copies of the current sample value, where `k` is the number of output channels (mono:1, stereo:2).

##### Signal Reconstruction and Playback
For an audio record to be played back, there must exist a processing system that understands the original encoding scheme. For vinyl records, we have turntables connected to amplifiers. Moving the stylus along the vinyl grooves generates a continuous electrical signal, which is sent to a speaker. The speaker, being an analog device, responds to the continuous electrical signal by moving its membrane, creating air vibrations that we hear as sound.

To playback digital records, however, we need the right software. The standardization of audio formats ensures that any piece of software that adheres to certain conventions can "decode" and play a digital record. This is one key principle behind the audio playback feature of production-grade software such as VLC, Windows Media Player, Apple Music, Spotify, and even Mixxx.

A second requirement of this playback chain is the accurate reconstruction of the original analog signal from the sampled digital representation. Audio playback software communicates with a digital audio interface that contains a **DAC (Digital to Analog Converter)**, which reconstructs the analog signal and supplies it to the speaker. DAC hardware circuits implement digital reconstruction filters. These filters perform mathematical transformations to the discrete sample sequence to recreate the original analog signal.


#### The Mixxx Audio-Playback Stack
Mixxx exposes two important parameters in the *Sound Hardware Preferences* panel:

- **Sample Rate (Hz)**: Determines the DAC sample rate, i.e., how frequently audio frames are converted to analog signals.
- **Audio Buffer (ms)**: Specifies the total buffer duration, indirectly determining the size of ALSA’s ring buffer.

*Mixxx Sound Hardware Preferences*
![Mixxx Sound Hardware Preferences]({static}/images/news/mixxx-sound-hw-prefs.png)

These parameters together influence the quality of output sound by defining the size in frames of the ALSA ring-buffer for the selected sample-rate:
```
Ring Buffer Size = (Audio Buffer in s) * (Sample Rate in Hz)
```

##### Mixxx Buffering Hierarchy
As audio frames move from Mixxx to the speakers, they pass through three distinct buffering levels:

**Mixxx Buffer** (userspace, heap):  
   Holds outbound frames from the track, possibly post-processing or effects.  
   This buffer is typically larger and acts as the staging area for frames handed off to ALSA.

**ALSA Ring Buffer** (kernel-managed, DMA-mapped):  
   A circular buffer of configurable size (in frames), subdivided into **periods**. Mixxx writes to this buffer in chunks, while the DMA engine drains it in `period_size` frames - a value negotiated between Mixxx and the audio-card driver at initialization. Usually, `period_size = Ring Buffer Size / 2`[^1]. Each time a period is emptied, ALSA triggers a software interrupt which is handled by a userspace callback in a high-priority Mixxx thread. This callback prepares frames and refills the ring-buffer.

**DAC FIFO** (hardware-level):  
   A small first-in-first-out queue that is read at the **DAC sample rate**, typically 44.1 kHz, 48 kHz, or 96kHz. This hardware buffer feeds the analog reconstruction circuitry with upstream frames. For instance, a 96kHz DAC consumes one frame every 1/96k s (≈10.4 us), totaling 96000 frames per second.

*Mixxx, ALSA, DAC buffers*
![Mixxx Buffering Hierarchy]({static}/images/news/mixxx-buffer-heirarchy.jpeg)

 While technically, only `period_size` frames are written to the DAC between callbacks via DMA, Mixxx prepares `Ring Buffer Size` frames in that duration. We can therefore simplify our model by noting that on average, `Ring Buffer Size` frames are written to the DAC every callback.

##### Buffer Underruns
From the `period_size` we compute `period_time = period_size / DAC Sample Rate`. This defines a hard real-time deadline for the userspace audio callback: it must prepare at least `period_size` frames within `period_time` to avoid starving the DAC’s hardware FIFO. This relation also confirms that large ring-buffers and lower DAC sample-rates reduce CPU pressure.

Whether this constraint is met depends on several factors - such as the complexity of audio processing in the real-time thread, OS scheduling latency, memory pressure, etc. Since general-purpose kernels do not provide any real-time guarantees, short `period_time` values can occasionally cause the callback to miss its deadline. The result is a **buffer underrun**, heard as a pop or glitch in playback—unacceptable in live DJ performance.

Unlike typical audio players, Mixxx performs real-time manipulation of audio—mixing, tempo changes, effects, scratching, and more - making the callback workload heavier. This demands low-latency implementations of all audio processing workflows to avoid underruns without compromising quality.

---

#### The Need for Sample-Rate Conversion
While buffer underruns occur due to DAC starvation regardless of the buffer contents, another class of audio distortions is caused arises when the DAC receives the wrong sequence of frames in its FIFO.

***The input sample rate defines how many frames of a digital record represent one second of analog audio***. For example, a digital record sampled from analog at 96kHz stores 96,000 frames for every second of analog sound. ***The DAC sample rate specifies how many outbound frames are consumed per second of real-world (wall-clock) time during playback.***

* If a 96kHz record is played back on a DAC operating at 48 kHz without resampling, only 48k frames are processed each second—meaning less than a full second of the outbound audio is played back per second. This results in an unintended slowdown.
* Conversely, if the DAC sample rate exceeds the input sample rate, more than one second of the original recording is heard every second, creating the perception of sped-up and pitch-shifted playback.

<section style="margin-top: 2rem;">
  <h5 style="text-align: center;">Tempo ramping (sample-rate mismatch, no resampling)</h5>
  <div style="display: flex; gap: 2rem; align-items: flex-start; flex-wrap: wrap; justify-content: center; margin-top: 1rem;">
    <figure style="display: flex; flex-direction: column; align-items: center;">
      <figcaption style="font-weight: bold; margin-bottom: 0.2rem;">Base (96kHz input, 96kHz DAC)</figcaption>
      <audio controls style="width: 250px;">
        <source src="{static}/audio/96in_96out_base-10s.mp3" type="audio/mp3">
        Your browser does not support the audio element.
      </audio>
    </figure>
    <figure style="display: flex; flex-direction: column; align-items: center;">
      <figcaption style="font-weight: bold; margin-bottom: 0.2rem;">Speedup (96kHz input, 192kHz DAC)</figcaption>
      <audio controls style="width: 250px;">
        <source src="{static}/audio/96in_192out_speedup-10s.mp3" type="audio/mp3">
        Your browser does not support the audio element.
      </audio>
    </figure>
    <figure style="display: flex; flex-direction: column; align-items: center;">
      <figcaption style="font-weight: bold; margin-bottom: 0.2rem;">Slowdown (96kHz input, 48kHz DAC)</figcaption>
      <audio controls style="width: 250px;">
        <source src="{static}/audio/96in_48out_slowdown-10s.mp3" type="audio/mp3">
        Your browser does not support the audio element.
      </audio>
    </figure>
  </div>
</section>

In these scenarios, resampling is a corrective procedure that transforms audio sampled at the input sampling rate to match the DAC’s expected output rate.

Resampling may also be used to *induce* tempo ramping. For a record sampled at 96kHz with a DAC also at 96kHz, scaling tempo by a factor of 3 means we want to pass 3x frames to the DAC on each callback than we would during standard playback.

- Without resampling, writing 3x frames per callback in an attempt to increase tempo would overfill the Mixxx-ALSA buffers. In the worst case, the excess frames would be dropped. Either way, the DAC would still consume only 96k frames per second instead of the entire 3 * 96k — nullifying the intended tempo increase.
- For accurate tempo-ramping, we must represent a longer amount of track duration using less frames, while ensuring that there are enough remaining frames for accurate reconstruction. That is, every second, we need to represent 3 * 96k frames using 96k frames only.
- This resample is achieved by a procedure called digital *decimation*, wherein frames are actually removed from a longer sequence before being written to the DAC.
- Conversely, digital *interpolation* is used when increasing track tempo, whereby new frames are generated between true samples using various algorithms.

 This is the reason higher samplerates are preferred in audio editing workflows: there is more room for decimation.

`SoundTouch`, `RubberBand`, `libzita` and `libsamplerate` are examples of open-source C++ libraries that implement standard algorithms to perform time-stretching on streaming data.

---


#### Contributions
**[mixxxPR#15081](https://github.com/mixxxdj/mixxx/pull/15081): Custom samplerates setting for recording.**

This PR introduces an improved user experience in the recording preferences page. No more error messages for incompatible formats. The GUI maintains the necessary format invariants. This PR also introduces `libsamplerate` to the build system along with a base resampler class using the libsamplerate `src_process` API.

**Key Files**

- [dlgprefsrecording.cpp]
- [enginerecord.cpp]
- [recordingmanager.cpp]

---

**[mixxxPR#15160](https://github.com/mixxxdj/mixxx/pull/15160): Custom samplerates setting for broadcasting.**

This PR allows users to choose custom samplerates for each broadcast profile, independently of the engine samplerate.

**Key Files**

- [dlgprefsbroadcast.cpp]
- [shoutconnection.cpp]
- [broadcastmanager.cpp]

Users can now pick custom samplerates for both recording and broadcasting, independent of the engine samplerate.

---

**[mixxxPR#15005](https://github.com/mixxxdj/mixxx/pull/15005): Support for low-latency scratching using the libsamplerate callback API**

This PR implements a resampler class using the libsamplerate Callback API. We observed a reduction in per-buffer resampling latency from 20us to 10us - a 2x improvement over the handcrafted linear interpolator.

**Key Files**

- [enginebuffer.cpp]
- [enginemixer.cpp]
- [enginebufferscalesrc.cpp]
- [dlgprefsound.cpp]

---


### Future Work
- Benchmarking the latency and CPU usage of the various resamplers during scratching.


### Acknowledgements
I thank Daniel, Evelynne, Ronny, and JoergBerg, who have spent considerable time reviewing my PRs and offering assistance anytime I needed it.


### References
[^1] https://0pointer.de/blog/projects/all-about-periods.html
