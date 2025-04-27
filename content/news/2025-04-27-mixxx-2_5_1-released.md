title: Mixxx 2.5.1 Released
authors: Evelynne Veys
tags: 2.5.1, release announcement
comments: yes
date: 2025-04-27 12:43:25

#### Mixxx 2.5.1 Release Announcement

We're proud to announce a new stable release of Mixxx, version 2.5.1.
It is recommended that all users update to this new version.
Thanks to your feedback, this release contains a lot of improvements, fixes, new and improved controller-mappings.
We would like to emphasize the importance of testing new versions. You don't have to be a programmer to contribute to Mixxx. Translating, testing new features and checking for possible regression is just as important.
If you would like to contribute to your favorite DJ program, click on [contribute](https://mixxx.org/get-involved/).

Enjoy Mixxx


#### 2.5.1 Changelog
The complete changelog can be found [here](https://github.com/mixxxdj/mixxx/milestone/45)

### Controller Mappings

* Behringer DDM4000 & BCR2000: Update mappings to 2.5
  [#14232](https://github.com/mixxxdj/mixxx/pull/14232)
  [#14349](https://github.com/mixxxdj/mixxx/pull/14349)
* DJ TechTools MIDI Fighter Spectra: Add controller mapping
  [#14559](https://github.com/mixxxdj/mixxx/pull/14559)
* Hercules DJControl Inpulse 300: add toneplay, slicer, and beatmatch functionalities
  [#14051](https://github.com/mixxxdj/mixxx/pull/14051)
  [#14057](https://github.com/mixxxdj/mixxx/pull/14057)
* Hercules DJControl Inpulse 500: New mapping
  [#14491](https://github.com/mixxxdj/mixxx/pull/14491)
  [#14510](https://github.com/mixxxdj/mixxx/pull/14510)
* Hercules DJ Console Mk1: Fix pitch bend buttons [#14447](https://github.com/mixxxdj/mixxx/pull/14447)
* M-Vave SMC-Mixer: Add controller mapping
  [#14411](https://github.com/mixxxdj/mixxx/pull/14411)
  [#14448](https://github.com/mixxxdj/mixxx/pull/14448)
  [#14457](https://github.com/mixxxdj/mixxx/pull/14457)
  [#14458](https://github.com/mixxxdj/mixxx/pull/14458)
* M-Vave SMK-25 II: Piano keyboard mapping
  [#14412](https://github.com/mixxxdj/mixxx/pull/14412)
  [#14484](https://github.com/mixxxdj/mixxx/pull/14484)
* Numark Mixtrack Platinum: Fix VU Meters [#14575](https://github.com/mixxxdj/mixxx/pull/14575)
* Numark NS6II: New mapping [#11075](https://github.com/mixxxdj/mixxx/pull/11075)
* Numark Platinum FX: New mapping [#12872](https://github.com/mixxxdj/mixxx/pull/12872)
* Pioneer-DDJ-SB3: Fixes slip mode and adds missing knob controls [#11307](https://github.com/mixxxdj/mixxx/pull/11307)
* Reloop Digital Jockey 2 IE: New mapping
  [#4614](https://github.com/mixxxdj/mixxx/pull/4614)
  [#14328](https://github.com/mixxxdj/mixxx/pull/14328)
* Traktor S4mk3: Set 4 decks, avoid CO warnings for decks 3/4, eg. VU meter
  [#14249](https://github.com/mixxxdj/mixxx/pull/14249)
* Traktor S4mk3: Smooth xfader curve for Const Power mode
  [#14305](https://github.com/mixxxdj/mixxx/pull/14305)
  [#14329](https://github.com/mixxxdj/mixxx/pull/14329)
  [#14103](https://github.com/mixxxdj/mixxx/issues/14103)
* Traktor S4mk3: stop wheel led blinking when track is over/stopped
  [#14028](https://github.com/mixxxdj/mixxx/pull/14028)
  [#13995](https://github.com/mixxxdj/mixxx/issues/13995)
* Traktor Kontrol S3: Use pitch absolute mode as described in the manual [#14123](https://github.com/mixxxdj/mixxx/pull/14123)
* Stanton SCS.1m/d; Keith McMillen QuNeo; EKS Otus: use `playposition` instead of non-existent `visual_playposition`
  [#14609](https://github.com/mixxxdj/mixxx/pull/14609)
  [#14603](https://github.com/mixxxdj/mixxx/issues/14603)

### Controller Backend

* Controllers: Avoid timer warning on button release [#14323](https://github.com/mixxxdj/mixxx/pull/14323)
* Controller preferences: Fix notify of pending changes when closing preferences [#14234](https://github.com/mixxxdj/mixxx/pull/14234)
  [#14220](https://github.com/mixxxdj/mixxx/issues/14220)
* Controller preferences: Fix broken overwrite dialog ('Save as..' not working) [#14263](https://github.com/mixxxdj/mixxx/pull/14263)
* Controller preferences: Don't break support link texts [#14079](https://github.com/mixxxdj/mixxx/pull/14079)
* Controller preferences: Fix wrong mapping change confirmation request caused by MidiController::makeInputHandler()
  [#14281](https://github.com/mixxxdj/mixxx/pull/14281)
  [#14280](https://github.com/mixxxdj/mixxx/issues/14280)
  [#14292](https://github.com/mixxxdj/mixxx/pull/14292)
* Controller mapping info: Fix cropped description text
  [#14332](https://github.com/mixxxdj/mixxx/pull/14332)
  [#14117](https://github.com/mixxxdj/mixxx/issues/14117)
* MIDI controller learning: Make control box search usable [#14260](https://github.com/mixxxdj/mixxx/pull/14260)
* MIDI controller learning: Don't reload mapping after learn [#14253](https://github.com/mixxxdj/mixxx/pull/14253)
* MIDI controller learning: Correct skin control for mic/aux section [#14221](https://github.com/mixxxdj/mixxx/pull/14221)
* MIDI controller learning: Add more cue controls for samplers
  [#14419](https://github.com/mixxxdj/mixxx/pull/14419)
* MIDI controller learning: Continue after the maximum learning time is over [#14429](https://github.com/mixxxdj/mixxx/pull/14429)
* Allow `midino` 0 in `MidiController::makeInputHandler()
  [#14266](https://github.com/mixxxdj/mixxx/pull/14266)
  [#14265](https://github.com/mixxxdj/mixxx/issues/14265)
* Fix: provide `incomingData` to MIDI sysex mappings
  [#14368](https://github.com/mixxxdj/mixxx/pull/14368)
  [#13133](https://github.com/mixxxdj/mixxx/issues/13133)
* Fix log spam when using Midi for light mapping
  [#14326](https://github.com/mixxxdj/mixxx/issues/14326)
  [#14327](https://github.com/mixxxdj/mixxx/pull/14327)
  [#14333](https://github.com/mixxxdj/mixxx/pull/14333)
  [#14338](https://github.com/mixxxdj/mixxx/pull/14338)
  [#14371](https://github.com/mixxxdj/mixxx/pull/14371)
* Fix for `TypeError` in `midi-components-0.0.js`
  [#14203](https://github.com/mixxxdj/mixxx/pull/14203)
  [#14197](https://github.com/mixxxdj/mixxx/issues/14197)
* Fix crash due to concurrent access in MidiController [#14159](https://github.com/mixxxdj/mixxx/pull/14159)

### Skins

* Deere/LateNight (64 samplers): Bring back library in regular view
  [#14101](https://github.com/mixxxdj/mixxx/pull/14101)
  [#14097](https://github.com/mixxxdj/mixxx/issues/14097)
  [#14700](https://github.com/mixxxdj/mixxx/issues/14700)
* Fix crash when hiding waveforms in Deere
  [#14170](https://github.com/mixxxdj/mixxx/pull/14170)
* Waveform Overview: Abort play pos dragging if cursor is released outside the valid area
  [#13741](https://github.com/mixxxdj/mixxx/pull/13741)
  [#13732](https://github.com/mixxxdj/mixxx/issues/13732)
* Waveform Overview: Also render analysis progress when triggered by track menu or analysis feature [#14150](https://github.com/mixxxdj/mixxx/pull/14150)
* Don't show 'menubar hide' dialog when switching skins [#14254](https://github.com/mixxxdj/mixxx/pull/14254)
* Key Wheel: Move to View menu and make it a floating tool window
  [#14256](https://github.com/mixxxdj/mixxx/pull/14256)
  [#14239](https://github.com/mixxxdj/mixxx/pull/14239)
* Center effect parameter names [#14598](https://github.com/mixxxdj/mixxx/pull/14598)
* Track menu: highlight row when hovering checkbox
  [#14636](https://github.com/mixxxdj/mixxx/pull/14636)
  [#14680](https://github.com/mixxxdj/mixxx/pull/14680)

### Library

* Add Ctrl+Shift+C to copy the content of the selected cell(s) (The Mxxx 2.4 behaviour of Ctrl+C).
  [#14114](https://github.com/mixxxdj/mixxx/pull/14114)
  [#14065](https://github.com/mixxxdj/mixxx/issues/14065)
* Fix MusicBrainz lookup on Windows and macOS [#14216](https://github.com/mixxxdj/mixxx/pull/14216)
* Library scanner: Update cached 'missing' flag when file is redicovered
  [#14250](https://github.com/mixxxdj/mixxx/pull/14250)
* Hidden Tracks: Allow 'load to' via track context manu [#14077](https://github.com/mixxxdj/mixxx/pull/14077)
* Update to libdjinterop 0.24.3 - support for Engine 4.1/4.2
  [#14172](https://github.com/mixxxdj/mixxx/pull/14172)
  [#14289](https://github.com/mixxxdj/mixxx/pull/14289)
* Fix writing metadata via symlink [#13711](https://github.com/mixxxdj/mixxx/pull/13711)
* Library menu: change "Engine DJ Prime" to "Engine DJ"
  [#14248](https://github.com/mixxxdj/mixxx/pull/14248)
  [#14682](https://github.com/mixxxdj/mixxx/pull/14682)
* Fix file extension handling during playlist export [#14381](https://github.com/mixxxdj/mixxx/pull/14381)
* Fix manual key metadata editing in track properties dialog
  [#14022](https://github.com/mixxxdj/mixxx/pull/14022)
  [#14400](https://github.com/mixxxdj/mixxx/issues/14400)
  [#14295](https://github.com/mixxxdj/mixxx/pull/14295)
  [#14294](https://github.com/mixxxdj/mixxx/issues/14294)
* History: Don't allow joining with locked previous playlist
  [#14401](https://github.com/mixxxdj/mixxx/pull/14401)
  [#14399](https://github.com/mixxxdj/mixxx/issues/14399)
* Track info dialog: fixed cover label (max) size [#14418](https://github.com/mixxxdj/mixxx/pull/14418)
* Track Menu: Reset `eject` after moving track file to trash [#14402](https://github.com/mixxxdj/mixxx/pull/14402)
* Fix AutoDJ "Remove Crate" action
  [#14426](https://github.com/mixxxdj/mixxx/pull/14426)
  [#14425](https://github.com/mixxxdj/mixxx/issues/14425)
* Fix scrolling issue with coverart columns visible
  [#13719](https://github.com/mixxxdj/mixxx/pull/13719)
  [#14631](https://github.com/mixxxdj/mixxx/pull/14631)
* Developer Tools: multi-word search, no Tab navigation in controls table [#14474](https://github.com/mixxxdj/mixxx/pull/14474)
* Analyze feature: respect New / All selection when searching
  [#14660](https://github.com/mixxxdj/mixxx/pull/14660)
  [#14659](https://github.com/mixxxdj/mixxx/issues/14659)
* Stop populating Computer library feature when Mixxx should close [#14573](https://github.com/mixxxdj/mixxx/pull/14573)
* Tracks: apply played/missing text color also to selected tracks [#13583](https://github.com/mixxxdj/mixxx/pull/13583)
* Tracks: `show_track_menu` at index position [#14385](https://github.com/mixxxdj/mixxx/pull/14385)
* Search related menu: improve checkbox click UX [#14637](https://github.com/mixxxdj/mixxx/pull/14637)
* Avoid false missing tracks due to db inconsistency
  [#14615](https://github.com/mixxxdj/mixxx/pull/14615)
  [#14513](https://github.com/mixxxdj/mixxx/issues/14513)
* Fix automatic trimming of search bar text
  [#14497](https://github.com/mixxxdj/mixxx/pull/14497)
  [#14486](https://github.com/mixxxdj/mixxx/issues/14486)
* Avoid crash after removing Quick Link
  [#14556](https://github.com/mixxxdj/mixxx/pull/14556)
  [#8270](https://github.com/mixxxdj/mixxx/issues/8270)

### Other Fixes

* Enable R3 time-stretching with Rubberband 4.0.0 API version numbers [#14100](https://github.com/mixxxdj/mixxx/pull/14100)
* Preferences Effects: add Hide/Unhide (move) buttons to Effects tab [#13329](https://github.com/mixxxdj/mixxx/pull/13329)
* Preferences Effects: left/right key in effect lists trigger hide/unhide [#14205](https://github.com/mixxxdj/mixxx/pull/14205)
* Fix beat sync in Flanger effect [#14351](https://github.com/mixxxdj/mixxx/pull/14351)
* Apply talkover ducking after main effects to allow using a compressor effect
  [#13844](https://github.com/mixxxdj/mixxx/pull/13844)
  [#12451](https://github.com/mixxxdj/mixxx/issues/12451)
* Fix sporadic deadlocks when closing Mixxx or changing sound devices
  [#14208](https://github.com/mixxxdj/mixxx/pull/14208)
  [#14055](https://github.com/mixxxdj/mixxx/issues/14055)
* PositionScratchController: Fix loop wrap-around case [#14379](https://github.com/mixxxdj/mixxx/pull/14379)
* Allow seeking to a hotcue during waveform scratching
  [#14357](https://github.com/mixxxdj/mixxx/pull/14357)
  [#13981](https://github.com/mixxxdj/mixxx/issues/13981)
* Reset saved loop when toggling off after switching cue type
  [#14661](https://github.com/mixxxdj/mixxx/pull/14661)
  [#14657](https://github.com/mixxxdj/mixxx/issues/14657)
* Fix leaks from fid_design()
  [#14567](https://github.com/mixxxdj/mixxx/pull/14567)
  [#9470](https://github.com/mixxxdj/mixxx/issues/9470)

### Target support

* Allow to build with git "showSignature = true"
  [#14115](https://github.com/mixxxdj/mixxx/pull/14115)
  [#12997](https://github.com/mixxxdj/mixxx/issues/12997)
* Support building with Qt 6.8/6.9
  [#14080](https://github.com/mixxxdj/mixxx/pull/14080)
  [#14071](https://github.com/mixxxdj/mixxx/issues/14071)
  [#14200](https://github.com/mixxxdj/mixxx/pull/14200)
  [#14204](https://github.com/mixxxdj/mixxx/pull/14204)
  [#14440](https://github.com/mixxxdj/mixxx/pull/14440)
  [#14518](https://github.com/mixxxdj/mixxx/pull/14518)
* Welcome Ubuntu Plucky Puffin; Good bye Mantic Minotaur
  [#14148](https://github.com/mixxxdj/mixxx/pull/14148)
  [#14158](https://github.com/mixxxdj/mixxx/pull/14158)
* Add more translations to Linux desktop file
  [#14153](https://github.com/mixxxdj/mixxx/pull/14153)
  [#14169](https://github.com/mixxxdj/mixxx/pull/14169)
* Debian: recommend qt6-translations-l10n [#14147](https://github.com/mixxxdj/mixxx/pull/14147)
* Update FindFFTW3.cmake to not find version 2
  [#13937](https://github.com/mixxxdj/mixxx/pull/13937)
  [#13931](https://github.com/mixxxdj/mixxx/issues/13931)
* Allow building without tests-tools via new CMake options BUILD_TESTING and BUILD_BENCH
  [#14269](https://github.com/mixxxdj/mixxx/pull/14269)
* Fix and improve "missing env" error message [#14321](https://github.com/mixxxdj/mixxx/pull/14321)
* Qt 6.8: Ensure Mixxx uses "windowsvista" Qt style on Windows [#14228](https://github.com/mixxxdj/mixxx/pull/14228)
* Raise macOS target version to 11 (Qt 6.5 requirement). [#14440](https://github.com/mixxxdj/mixxx/pull/14440)
* Fail early when building on WSL [#14481](https://github.com/mixxxdj/mixxx/pull/14481)
* Remove useless udev rule [#14630](https://github.com/mixxxdj/mixxx/pull/14630)
* Handle new " / " from taglib 2.0
  [#12854](https://github.com/mixxxdj/mixxx/pull/12854)
  [#12790](https://github.com/mixxxdj/mixxx/issues/12790)
