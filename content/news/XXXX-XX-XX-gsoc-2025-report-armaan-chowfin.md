title: GSoC 2025 Work Product - Resampling Options for Mixxx
authors: Armaan Chowfin
status: draft
tags: gsoc, gsoc-2025
comments: yes

<!-- ## Introduction


## Motivation
 -->


## Background
#### Emulated Vinyl Scratching
**Vinyl scratching causes a sudden acceleration or deceleration in playback speed of the loaded track(s)**, creating the audible scratching sound. On a turntable, scratching is performed by moving the stylus by hand - causing it to follow grooves in the vinyl that correspond to the analog audio waveform. In this case, no distortions are generated. 

All DJ software, Mixxx included, must offer the ability to emulate vinyl scratching when tracks are stored digitally. Here, a MIDI controller's jog wheels can be spun to emulate the motion of the turntable stylus during scratching. However, since digitally stored audio is not a perfect representation of the analog waveform (i.e. it is a series of amplitude values sampled from the analog waveform at a fixed sample rate), *the software implementation of **playback speed change** is also imperfect*. More specifically, a resample of the input audio file is required. Sub-optimal resampling occasionally leads to unwanted, audible distortions during scratching.

At present, the resample during scratching uses a fast, handcrafted linear interpolation algorithm. Mixxx uses the SoundTouch and RubberBand libraries to perform general audio time-stretching. These libraries implement additional interpolation algorithms, but as of June 2025, Mixxx has no support for using these while scratching. Therefore, the project goal is defined:
- To determine whether alternative interpolation algorithms result in a *noticable reduction in scratching artifacts, or a latency improvement over linear interpolation*, **providing quantitative supporting evidence** in the form of C++ unit tests under varying scratching scenarios.
#### Audio Playback in Mixxx


## Implementation
I implemented resampling options to the Mixxx audio playback path through two branches:
- A longstanding issue requesting custom resample rates during recording and broadcast (issue #)
- Individual branches implementing pluggable C++17 resampler backends built on the libzita and libsamplerate libraries.


#### The libsamplerate and libzita libraries

