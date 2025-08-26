title: Mixxx 2.5.3 Released
authors: Evelynne Veys
tags: 2.5.3, release announcement
comments: yes
status: draft

#### Mixxx 2.5.3 Release Announcement: Back to School, back to DVS.

We're proud to announce a new stable release of Mixxx: version 2.5.3, this version not only contains updates and fixes for some issues reported by users, but it also contains a major Digital Vinyl System (DVS) rework. This rework is the outcome of some intensive holiday work and some bright new insights of developers spinning the wheel instead of sunbathing.
Because of the big improvements we have chosen to backport [^1] it to the 2.5 series instead of integrating it in future alfa & beta versions.

Have a nice Mixxx.

[^1]: Back porting is integrating new work into previous releases in order to let those releases profit from the new development.

#### Digital Vinyl System - Timecode System.

A good explanation about Digital Vinyl Systems and Timecode can be found in [this article on the site of Digital DJ Tips](https://www.digitaldjtips.com/a-beginners-guide-to-digital-vinyl-systems/), and in our own Newsarchive: [How Does Timecode Vinyl Actually Work? (Pt. 1)](https://mixxx.org/news/2021-11-21-dvs-internals-pt1/) and [How Does Timecode Vinyl Actually Work? (Pt. 2)](https://mixxx.org/news/2021-12-22-dvs-internals-pt2/).

In short summarized: Mixxx plays the music tracks by following the input signal produced by turntables/CD-players (and other Timecode-mediaplayers) which gets internally converted to actual timeframes by looking up references to timeframes in the musicfile.
So the DJ controls the playback of the digital music by manipulating vinyl or CDs.

In order to create a real vinyl-handling-feeling the produced music needs to follow the inputsignal as close as possible (low latency, fast responsiveness) for eg scratching, spinbacks etc. As there is no 'standard' inputsignal each manufacturer uses its own code. The received inputsignalcode needs to be converted to a usable timecode by using a LUT (Look Up Table), for each manufacturer another LUT needs to be created and used.  
The conversion of the inputsignal to the actual timeframecode is handled by the [XWAX library](https://xwax.org/overview.html). To filter 'impossible' or not 'logical' signals (eg frame 7000 directly after frame 1000) the [alpha-beta filter](https://en.wikipedia.org/wiki/Alpha_beta_filter) was used.

In the rework with [PR #15194](https://github.com/mixxxdj/mixxx/pull/15194) developers have replaced the alpha-beta filter with the [Kalman-filter](https://en.wikipedia.org/wiki/Kalman_filter). Kalman filters are generally used in GPS navigation and weather forecast models.

The use of it in DVS in short: a Kalman filter maintains a model of the vinyl to predict the current pitch. This predicted pitch is compared with the noisy crackling input signal. The deviation [^2] determines the trust which is used to incorporated these values into the model for the next prediction.

An example: The vinyl is likely rotating with 33 1/3 r/min. This is used as a first prediction. If the next measurement is 40 r/min the value is not trust worthy because such a speed-up is unlikely. This measurement will be used with a low trust. If all following values are however at 40 r/min the filter looses trust for the predicted value and slowly adopts 40 r/min for further predictions.

Using this deviation in Mixxx helps to properly and more accurately represent eg the rate-slider on the turntable/CD-player.

Another improvement has been made with the addition of a plausibility check for the time code signal [PR 15217](https://github.com/mixxxdj/mixxx/pull/15217).
This compares the left and right channel of the time code signal to remove noise and is even able to restore the original information. This significantly reduces sticker drift during fast scratch actions. (see [How Does Timecode Vinyl Actually Work? (Pt. 2)](https://mixxx.org/news/2021-12-22-dvs-internals-pt2/) #2)

These changes are major improvements for all DVS users. Too good to wait for Mixxx 2.6.  
Get all Mixxx-ed up and join our [testing-force](https://mixxx.org/get-involved/) for more upcoming features.

[^2]: The Deviation in Kalman filters is called [Innovation](https://en.wikipedia.org/wiki/Innovation_(signal_processing).

### Controller Mappings

Mappings were updated for
* Icon P1-Nano MIDI 1
* Traktor Kontrol S4 Mk3
* Traktor Kontrol S3
* Numark NS6II

#### 2.5.3 Changelog

More fixes have been included to keep everything stable, up and running.
The complete changelog can be found [here](https://github.com/mixxxdj/mixxx/blob/2.5.3/CHANGELOG.md)
