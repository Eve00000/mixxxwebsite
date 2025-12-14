title: Mixxx 2.5.4 Released
authors: Evelynne Veys
tags: 2.5.4, release announcement
comments: yes
status: draft

#### Mixxx 2.5.4 Release Announcement: bundling improvements at the end of the year.

We're proud to share our early Christmas present with you: Mixxx version 2.5.4, a new stable release of Mixxx.
This version bundles corrections, improvements and fixes for the issues you reported.  
We'd like to emphasize the importance of reporting issues and bugs and testing nightly builds of the [beta (bugfix)](https://mixxx.org/download/#beta) and [alpha (development)](https://mixxx.org/download/#testing) versions. This quality control ensures Mixxx continues to improve and ensures stress-free end-of-year sets for every Mixxx-DJ and Mixxx-broadcaster.
Thank you for bookmarking our [github](https://github.com/mixxxdj/mixxx/issues), [forum](https://mixxx.discourse.group/) and [zulip](https://mixxx.zulipchat.com/).  
If you want to join our test-panels, leave a note in the appropriate zulip-topic:

* [if you are on macOS](https://mixxx.zulipchat.com/#narrow/channel/267968-testing/topic/group.3A.20macOS_testers)
* [if you are on linux](https://mixxx.zulipchat.com/#narrow/channel/267968-testing/topic/group.3A.20.20linux_testers)
* [if you are on windows](https://mixxx.zulipchat.com/#narrow/channel/267968-testing/topic/group.3A.20windows_testers)

A lot of new users have found their way to Mixxx, all sharing the same passion for music in all forms: tango, techno, experimental electronic, merengue, dubstep, ... For every specific genre DJs are searching for an optimized controller mapping, adapted Auto DJ transitions or effects. Others are still searching their way in the silent darkness, you can help them by sharing your Mixxx experience, your illuminating fire can lead them to Mixxx.  
We welcome all users, we want you to DJ your own way.

Have a nice Mixxx.


#### Highlights: important fixes in 2.5.4

Some examples of improvements and fixes, more fixes have been included to keep everything stable, up and running, and as they were required by operating system updates or Qt improvements.
The complete changelog can be found [here](https://github.com/mixxxdj/mixxx/blob/2.5.4/CHANGELOG.md).


##### Controller Mappings

Mappings were improved and updated for Korg Kaoss DJ, Numark-NS6II, Pioneer DJ CDJ, Reloop Beatmix 2/4, Traktor S4Mk3.


##### Library

* Playlists: polish code for moving selected Tracks [#15454](https://github.com/mixxxdj/mixxx/pull/15454)
* Playlists: disable all modifying actions when locked [#15456](https://github.com/mixxxdj/mixxx/pull/15456)
* Playlists: improve performance when activating a playlist [#15465](https://github.com/mixxxdj/mixxx/pull/15465)
* Track view header: avoid narrow columns after restoring header with hidden columns
  [#15331](https://github.com/mixxxdj/mixxx/pull/15331)
  [#10325](https://github.com/mixxxdj/mixxx/issues/10325)
* Computer: fix "Refresh directory tree" action [#15301](https://github.com/mixxxdj/mixxx/pull/15301)
* Library scanner: make Cancel button work again
  [#15320](https://github.com/mixxxdj/mixxx/pull/15320)
  [#14940](https://github.com/mixxxdj/mixxx/issues/14940)
* Library scanner: fix an issue when verifying remaining tracks [#15346](https://github.com/mixxxdj/mixxx/pull/15346)


##### Preferences

* Library: reset library font/row height when closing without apply [#15455](https://github.com/mixxxdj/mixxx/pull/15455)
* Decks: fix Restore Defaults for rate slider direction and track time display
  [#15443](https://github.com/mixxxdj/mixxx/pull/15443)
  [#15444](https://github.com/mixxxdj/mixxx/pull/15444)
* Sound Hardware: don't allow closing preferences with faulty Sound Hardware config
  [#14576](https://github.com/mixxxdj/mixxx/pull/14576)
  [#6077](https://github.com/mixxxdj/mixxx/issues/6077)


##### Target support

* Update macOS (Intel) runner for building Mixxx to macOS Sequoia 15 [#15391](https://github.com/mixxxdj/mixxx/pull/15391)
* Welcome Ubuntu 26.04 LTS Resolute Raccoon [#15588](https://github.com/mixxxdj/mixxx/pull/15588)
* debian_buildenv.sh: assert for bash
  [#15274](https://github.com/mixxxdj/mixxx/pull/15274)
  [#15273](https://github.com/mixxxdj/mixxx/issues/15273)
* debian_buildenv.sh: use /etc/os-release instead of the obsolete /etc/lsb-release [#15298](https://github.com/mixxxdj/mixxx/pull/15298)
* debian_buildenv.sh: fix for Ubuntu Noble (24.10) [#15292](https://github.com/mixxxdj/mixxx/pull/15292)
* Support building with Qt 6.10: Add Qt6::GuiPrivate
  [#15502](https://github.com/mixxxdj/mixxx/pull/15502)
  [#15485](https://github.com/mixxxdj/mixxx/issues/15485)
* Touchscreen on Windows 11: disable touch -> mouse event translation
  [#15547](https://github.com/mixxxdj/mixxx/pull/15547)
  [#15546](https://github.com/mixxxdj/mixxx/issues/15546)


##### Misc

* Cover Art: Fix a rare issue causing an infinite loop and high CPU load when fetching a cover art from metadata failed unexpectedly
  [#15451](https://github.com/mixxxdj/mixxx/pull/15451)
  [#15199](https://github.com/mixxxdj/mixxx/issues/15199)
* Network Clock: fix writing wrong number of silence after a long underrun [#15400](https://github.com/mixxxdj/mixxx/pull/15400)
* Menu bar: fix showing/hiding via Alt
  [#15352](https://github.com/mixxxdj/mixxx/pull/15352)
  [#15350](https://github.com/mixxxdj/mixxx/issues/15350)
  [#15396](https://github.com/mixxxdj/mixxx/pull/15396)
  [#15385](https://github.com/mixxxdj/mixxx/issues/15385)
* SoundSourceFFmpeg: Fix opening files with non ASCII characters
  [#15700](https://github.com/mixxxdj/mixxx/pull/15700)
  [#15681](https://github.com/mixxxdj/mixxx/issues/15681)
