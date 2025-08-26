title: Mixxx 2.5.3 Released
authors: Evelynne Veys, Jan Claußen
tags: 2.5.3, release announcement
comments: yes
status: draft

#### Mixxx 2.5.3 Release Announcement: Back to School, back to DVS.

We're proud to announce a new stable release of Mixxx: version 2.5.3, this version not only contains updates and fixes for some issues reported by users, but it also contains improvements to the Digital Vinyl System (DVS) support. These improvements are the result of some bright new insights of developers spinning the wheel instead of sunbathing.
Since these improvements drastically improve the vinyl experience, we have chosen to backport [^1] it to the 2.5 series instead of integrating it in future alpha & beta versions.

Have a nice Mixxx.

[^1]: Back porting is integrating new work into previous releases in order to let those releases profit from the new development.

#### Digital Vinyl System - Timecode System.

An introduction to Digital Vinyl Systems and timecode can be found in [this article on the site of Digital DJ Tips](https://www.digitaldjtips.com/a-beginners-guide-to-digital-vinyl-systems/), and in our own news archive: [How Does Timecode Vinyl Actually Work? (Pt. 1)](https://mixxx.org/news/2021-11-21-dvs-internals-pt1/) and [How Does Timecode Vinyl Actually Work? (Pt. 2)](https://mixxx.org/news/2021-12-22-dvs-internals-pt2/).

To summarize, Mixxx plays the song by following the signal on the timecode media, which gets internally converted to actual timeframes via the LUT.
So the DJ controls the playback of the digital music by manipulating vinyl or CDs.

In order to create a vinyl-like feeling the song needs to follow the signal as closely as possible (low latency, fast responsiveness) for e.g. scratching or backspins. There is no 'standard' signal - each manufacturer uses its own code. The demodulated code from the signal needs to be converted to a usable timecode. This is realized by storing the states of the code in a LUT (lookup table) which is mapped to instants in time in the song. This makes it possible to quickly jump to positions in time during playback.
The conversion from timecode signal to actual timeframe is handled by the [xwax](https://xwax.org/overview.html) library. To detect the pitch of the signals (e.g. the playback speed) xwax uses an [Alpha-Beta Filter](https://en.wikipedia.org/wiki/Alpha_beta_filter).

In [PR #15194](https://github.com/mixxxdj/mixxx/pull/15194) developers have replaced the Alpha-Beta Filter with a more advanced [Kalman-Filter](https://en.wikipedia.org/wiki/Kalman_filter) equivalent. Kalman-Filters are generally used in GPS navigation and weather forecast models.

The use of it in DVS in short: a Kalman-Filter maintains a model of the vinyl to predict the current pitch. This predicted pitch is compared with the noisy crackling input signal. The deviation [^2] determines the trust which is used to incorporate these values into the model for the next prediction. It reuses the current constant velocity model of the original Alpha-Beta Filter.

If the vinyl is, say, spinning at 33 1/3 r/min, that value is used as the initial assumption on which the next prediction is based. If the next measurement returns 40 r/min, the value is not fully trusted, because such a high acceleration is unlikely. This measurement will be used with a low trust. However, if the following measurements detect 40 r/min as well, the filter gradually adjusts to to approximate 40 r/min in further predictions.

By using this model, Mixxx is able to properly and more accurately represent e.g. the pitch control slider on the turntable or CD-player.

Another improvement has been made to the measurements which are fed to the Kalman-Filter [PR 15217](https://github.com/mixxxdj/mixxx/pull/15217). The current model only takes rough measurements of the position on the sine wave. Furthermore these measurements work on the assumption to always be correct. A new check was added to detect if measurements were skipped or larger than assumed. This improves backspins where the deviation of estimation and measurement suddenly become very high.

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
