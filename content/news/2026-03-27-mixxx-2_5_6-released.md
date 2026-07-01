title: Mixxx 2.5.6 Released
authors: Antoine Colombier
tags: 2.5.6, release announcement
comments: yes
#### Mixxx 2.5.6 Release Announcement
date: 2026-03-27 18:39:58

We're proud to announce a new stable release of Mixxx: version 2.5.6. This version contains updates and fixes for issues, as well as improvements to effects, controller mappings, and overall stability. This should be the last 2.5 release.
Note that this version is the successor of version 2.5.4 because version 2.5.5 has been skipped following an issue in the release workflow.
We'd like to thank all users for their feedback and emphasize once again the importance of testing and reporting. Please join our [team](https://mixxx.org/get-involved/) to help make Mixxx even better.

Enjoy Mixxx!

#### Important updates and fixes in 2.5.6

- The 'not' operator in the search function was not working correctly.
- Rhythmbox imports were not functioning as expected.
- History playlists now allow track file export.
- Performance when restoring large track selections has been improved.
- White Noise and Echo effects have been enhanced, and crackling in QuickEffect has been fixed.
- Controller mappings for Numark Mixtrack 3, Pioneer CDJ-350, Reloop Beatmix 2/4, and Traktor Kontrol Z1 have been updated.
- Scratching with keylock enabled has been fixed.
- Flatpak packaging files have been added for easier installation on Linux.

#### 2.5.6 Changelog

The complete changelog can be found [here](https://github.com/mixxxdj/mixxx/blob/2.5.6/CHANGELOG.md).

##### Library

* Search: fix 'not' operator
  [#15923](https://github.com/mixxxdj/mixxx/pull/15923)
  [#15918](https://github.com/mixxxdj/mixxx/issues/15918)
* Rhythmbox: fix imports
  [#15798](https://github.com/mixxxdj/mixxx/pull/15798)
  [#15770](https://github.com/mixxxdj/mixxx/pull/15770)
* WTrackMenu: warn before opening more than 10 tracks in file browser
  [#15828](https://github.com/mixxxdj/mixxx/pull/15828)
  [#15819](https://github.com/mixxxdj/mixxx/issues/15819)
* Fix "dataChanged() called with an invalid index range" warning
  [#15937](https://github.com/mixxxdj/mixxx/pull/15937)
  [#14610](https://github.com/mixxxdj/mixxx/issues/14610)
* History: allow track file export
  [#16074](https://github.com/mixxxdj/mixxx/pull/16074)
* History: prevent deletion of current history after purging tracks
  [#15991](https://github.com/mixxxdj/mixxx/pull/15991)
* Tracks: improve performance when restoring large track selections
  [#15973](https://github.com/mixxxdj/mixxx/pull/15973)

##### Effects

* White Noise: remove DC offset
  [#15979](https://github.com/mixxxdj/mixxx/pull/15979)
* White Noise: improve gain responds
  [#15949](https://github.com/mixxxdj/mixxx/pull/15949)
* Echo: fix distortion bug
  [#15985](https://github.com/mixxxdj/mixxx/pull/15985)
  [#15835](https://github.com/mixxxdj/mixxx/issues/15835)
* Echo: fix ramping of the send and feedback parameters
  [#16006](https://github.com/mixxxdj/mixxx/pull/16006)
* QuickEffect: fix crackling noise when switching
  [#15796](https://github.com/mixxxdj/mixxx/pull/15796)
  [#15794](https://github.com/mixxxdj/mixxx/issues/15794)
* Glitch: remove unnecessary cast to integer
  [#16068](https://github.com/mixxxdj/mixxx/pull/16068)
* Reverb: fix ramping of the send parameter
  [#16001](https://github.com/mixxxdj/mixxx/pull/16001)

##### Controller Mappings

* Numark Mixtrack 3: update scripts
  [#14180](https://github.com/mixxxdj/mixxx/pull/14180)
* Pioneer CDJ-350: fix incorrect name in controller mapping
  [#15683](https://github.com/mixxxdj/mixxx/pull/15683)
* Reloop Beatmix 2/4: implement shift+jog wheel seek
  [#15575](https://github.com/mixxxdj/mixxx/pull/15575)
  [#12334](https://github.com/mixxxdj/mixxx/issues/12334)
* Traktor Kontrol Z1: fix crossfader cut
  [#14451](https://github.com/mixxxdj/mixxx/pull/14451)
  [#14450](https://github.com/mixxxdj/mixxx/issues/14450)
  [#15945](https://github.com/mixxxdj/mixxx/pull/15945)
* Traktor S4Mk2: check for deck undefined
  [#14445](https://github.com/mixxxdj/mixxx/pull/14445)

##### Engine

* Fix scratching with keylock enabled and mapping using scratch2
  [#15845](https://github.com/mixxxdj/mixxx/pull/15845)
* AudioUnit: fix crash due to off-by-one error in parameter syncing
  [#15919](https://github.com/mixxxdj/mixxx/pull/15919)
* AudioUnit: fix startup crash by loading out-of-process
  [#16106](https://github.com/mixxxdj/mixxx/pull/16106)
* FX units: resolve issue preventing use on all samplers
  [#15971](https://github.com/mixxxdj/mixxx/pull/15971)
  [#15799](https://github.com/mixxxdj/mixxx/issues/15799)
* Fix false positive "First sound has been moved!" warnings log message
  [#16054](https://github.com/mixxxdj/mixxx/pull/16054)
* Beats: fix rare off-by-one beat issue with quantize and sync
  [#13262](https://github.com/mixxxdj/mixxx/pull/13262)
  [#16086](https://github.com/mixxxdj/mixxx/pull/16086)

##### Preferences

* Interface: use main window screen to detect if skin fits
  [#15824](https://github.com/mixxxdj/mixxx/pull/15824)
  [#15823](https://github.com/mixxxdj/mixxx/issues/15823)

##### Skins

* Time widget: make ShowSeconds only show seconds, no extra locale info
  [#15805](https://github.com/mixxxdj/mixxx/pull/15805)
* Search related menu: fix search click trigger
  [#15912](https://github.com/mixxxdj/mixxx/pull/15912)
* Tracks: avoid re-sorting table when purging/hiding tracks
  [#15872](https://github.com/mixxxdj/mixxx/pull/15872)
  [#12565](https://github.com/mixxxdj/mixxx/issues/12565)

##### Target support

* Add Flatpak packaging files
  [#15695](https://github.com/mixxxdj/mixxx/pull/15695)
  [#15922](https://github.com/mixxxdj/mixxx/pull/15922)
  [#15935](https://github.com/mixxxdj/mixxx/pull/15935)
* Fail early if not running from Visual Studio environment
  [#14623](https://github.com/mixxxdj/mixxx/pull/14623)
* Make Debian non-free optional
  [#15895](https://github.com/mixxxdj/mixxx/pull/15895)
* Debian: remove 'qml6-module-qtquick-nativestyle
  [#15771](https://github.com/mixxxdj/mixxx/pull/15771)
* Ubuntu: retire Plucky Puffin 25.04
  [#15926](https://github.com/mixxxdj/mixxx/pull/15926)

##### Miscellaneous

* Fix `mixxx-test` build to find `mad.h` [#15803](https://github.com/mixxxdj/mixxx/pull/15803)
* Num deck streamline
  [#14112](https://github.com/mixxxdj/mixxx/pull/14112)
  [#16009](https://github.com/mixxxdj/mixxx/pull/16009)
