title: Mixxx 2.6 beta Released
authors: Evelynne Veys
tags: 2.6 beta, release announcement start  CI
comments: yes
status: draft

#### Mixxx 2.6 beta Release Announcement

We're proud to announce a new beta release of Mixxx, version 2.6.
This is a beta release containing a lot of new features, we offer this release in order to get it tested by the most possible users.
We look forward to the feedback of all users.

Mixxx needs testers for the new 2.6 beta release.
If you would like to contribute to your favorite DJ program (translate, add a mapping, add a feature, test) [Get Involved](https://mixxx.org/get-involved/).

Enjoy Mixxx 2.6 beta

#### STEMS STEMS STEMS

To explain what STEMS are all about, we take a sentence from the introduction of this post, a regular text and convert it to a STEMS-text.
In this example we extract 4 TEXT-STEMS that represent each a particular part of that sentence that only result in the original text when they are combined they result.

In the Text stems example here you can play with the mute and effect buttons.
With the MUTE button you can make the Channel disappear, with the Effect button you can add an effect to the Channel.
To see the original text each Channel needs to be un-muted and without effect.

<div class="stemdemo">
  <style>
    body {
      font-family: Raleway;
      padding: 20px;
    }
    .channel {
      margin-bottom: 10px;
    }
    .stemdemo {
      padding:1em;
      border:1px #555 solid;
      border-radius:8px;
      margin: 1em;
    }
    .channel button {
      margin-right: 10px;
    }
    .channel button:hover, .channel button:active {
      border-color: unset;
    }
    .channel button[state='true'] {
      color: var(--accent-color-hover);
    }
    #output {
      font-family: monospace;
      white-space: pre-wrap;
      font-size: 28px;
      background-color: black;
      padding: 0.5em;
    }
    .c1 { color: #009E73; }
    .c2 { color: #D55E00; }
    .c3 { color: #CC79A7; }
    .c4 { color: #56B4E9; }

    #output.c1_mute .c1 { opacity: 0.2; }
    #output.c2_mute .c2 { opacity: 0.2; }
    #output.c3_mute .c3 { opacity: 0.2; }
    #output.c4_mute .c4 { opacity: 0.2; }

    #output.c1_fx .c1 { filter: blur(2.5px); }
    #output.c2_fx .c2 { filter: blur(2.5px); }
    #output.c3_fx .c3 { filter: blur(2.5px); }
    #output.c4_fx .c4 { filter: blur(2.5px); }

    #output span { transition: all 500ms ease; }
  </style>

  <div class="channel" data-channel="1">
    <button class="mute">Mute</button>
    <button class="fx">Effect</button>
    Channel&nbsp;1&nbsp;Consonants&nbsp;+&nbsp;punctuation
  </div>
  <div class="channel" data-channel="2">
    <button class="mute">Mute</button>
    <button class="fx">Effect</button>
    Channel&nbsp;2&nbsp;All&nbsp;'a'
  </div>
  <div class="channel" data-channel="3">
    <button class="mute">Mute</button>
    <button class="fx">Effect</button>
    Channel&nbsp;3&nbsp;All&nbsp;'e'
  </div>
  <div class="channel" data-channel="4">
    <button class="mute">Mute</button>
    <button class="fx">Effect</button>
    Channel&nbsp;4&nbsp;other&nbsp;vowels
  </div>

  <div id="output"></div>

  <script>
    const baseText = "Mixxx needs testers for the new 2.6 beta release.";

    function getCharChannel(char) {
      const lower = char.toLowerCase();
      if (lower === 'a') return 2;
      if (lower === 'e') return 3;
      if (/[aeiou]/.test(lower)) return 4;
      return 1;
    }

    const canvas = document.getElementById('output');

    document.querySelectorAll('div.channel button').forEach(
      b => b.addEventListener("click",
        function toggle() {
          const state = this.getAttribute("state") !== "true";
          const channel = this.parentElement.getAttribute("data-channel")
          if (state) {
            canvas.classList.add(`c${channel}_${this.className}`);
          } else {
            canvas.classList.remove(`c${channel}_${this.className}`);
          }
          this.setAttribute("state", state);
        }
      )
    );

    canvas.innerHTML = '';
    for (let i = 0; i < baseText.length; i++) {
      const char = baseText[i];
      const channel = getCharChannel(char);

      canvas.innerHTML += `<span class="c${channel}">${char}</span>`;
    }
  </script>
</div>

Wouldn't it be nice if we could do this tricks with music?
This possibility exists and is called STEMS. Stems are audiotracks that contain 4 stems-tracks: Drums, Bass, Melody (or other) and Vocals (or Vox).

#### STEMS in Mixxx

With Mixxx starting with this 2.6 beta release you can control the Volume and Effects for each of these Stem-tracks independently.
At the moment you need to create the STEM-Tracks with 3rd party tools, but once added to the Mixxx-library you can control them like
- create an a cappela: Play Drums, Bass and Other "Normal" + mute the Vocals
- create a drum pattern: Play Drums normally, mute Bass, Other and Vocals
- remix a track: set the volume of the Drums on 100%, the bass on 90%, the other on 50% and the vocals on 60%
- add effects to the vocal only: set the volume of all stems to a value of your choice + add reverb effect only to the vocals.

Mixxx wouldn't be Mixxx if these controls weren't available for your controller, so they arthis.
To control the Stems you need for each channel, depending on your choice
- 4 buttons if you want to use the 'mute' functions
- 4 encoders to control the Volume
- 4 buttons to activate the effect
- 4 encoders to control the effect level.

Find more info about Stems in Mixxx in [Antoine Colombier's GSoC Article](https://mixxx.org/news/2024-08-26-stem-mixing/)

@Video(https://www.youtube.com/watch?v=oEJuvVFSbnE)


#### CUES CUES CUES

What if you wanted to add an extra cue, that logically should be between the 2nd and 3rd cue you created? In 2.6 beta this problem is solved. You can move a cue to another pôsition without the need to re-create the cue. It's also possible to rearrange all cues according to their position in the track.


![Drop a hotcue on another position]({static}/images/news/Mixxx_2_6_beta_drop_hotcue_on_other_position.gif)

You can also use a menu action to re-order the hot cues on their position in the track.


![Drop a hotcue on another position]({static}/images/news/Mixxx_2_6_beta_menu_reorder_hotcues.gif)

Before cues played as long as you pressed them, to continue the playing you needed to press the play button. That's solved too: press the cue, drag & drop it on the play button and ... tada the track keeps playing.


#### 2.6 beta Changelog
The complete changelog can be found [here](https://github.com/mixxxdj/mixxx/blob/2.6-beta/CHANGELOG.md)

### STEM file support

* Add simple support for STEM files [#13044](https://github.com/mixxxdj/mixxx/pull/13044)
* Add stem controls [#13086](https://github.com/mixxxdj/mixxx/pull/13086)
* Add analyser support for stem [#13106](https://github.com/mixxxdj/mixxx/pull/13106)
* Add quick effect support on stem [#13123](https://github.com/mixxxdj/mixxx/pull/13123)
* Add advanced stem loading COs [#13268](https://github.com/mixxxdj/mixxx/pull/13268)
* Multithreaded Rubberband
  [#13143](https://github.com/mixxxdj/mixxx/pull/13143)
  [#13649](https://github.com/mixxxdj/mixxx/pull/13649)
* Add support for stem in the engine
  [#13070](https://github.com/mixxxdj/mixxx/pull/13070)
  [#14244](https://github.com/mixxxdj/mixxx/pull/14244)
* Add stem files to the taglib lookup table [#13612](https://github.com/mixxxdj/mixxx/pull/13612)
* Stem controls for LateNight
  [#13537](https://github.com/mixxxdj/mixxx/pull/13537)
  [#14745](https://github.com/mixxxdj/mixxx/pull/14745)
* Fix: make "stem_group,mute" a powerwindow button
  [#13751](https://github.com/mixxxdj/mixxx/pull/13751)
  [#13749](https://github.com/mixxxdj/mixxx/issues/13749)
* Stem control test fix [#13960](https://github.com/mixxxdj/mixxx/pull/13960)
* Solves problem with special characters in path to stems [#13784](https://github.com/mixxxdj/mixxx/pull/13784)
* Enable FFmpeg (free) on Windows. [#14695](https://github.com/mixxxdj/mixxx/pull/14695)
* FFmpeg: Use internal aac decoder. If not available give a hint. [#14645](https://github.com/mixxxdj/mixxx/pull/14645)
* Fix build with -DSTEM=OFF [#13948](https://github.com/mixxxdj/mixxx/pull/13948)
* Fix warning when building without STEM support [#14551](https://github.com/mixxxdj/mixxx/pull/14551)
* Fix: exclude stem samples for QML waveform [#13655](https://github.com/mixxxdj/mixxx/pull/13655)
* Fix: use generic way to get the group on stem waveform renderer [#14291](https://github.com/mixxxdj/mixxx/pull/14291)

### Library

* Add color coding for Key column [#13390](https://github.com/mixxxdj/mixxx/pull/13390)
* Add Key Color Palettes [#13497](https://github.com/mixxxdj/mixxx/pull/13497)
* Add overview column with small waveform [#14140](https://github.com/mixxxdj/mixxx/pull/14140)
* Add a rebindable keyboard shortcut for editing items as a replacement for F2 [#13148](https://github.com/mixxxdj/mixxx/pull/13148)
* CmdlineArgs: Add `--rescan-library` for rescanning on startup [#13661](https://github.com/mixxxdj/mixxx/pull/13661)
* Add case-insensitive parsing for Lancelot key notation [#14318](https://github.com/mixxxdj/mixxx/pull/14318)
* iTunes: Add iOS importer using the Media Player framework [#12690](https://github.com/mixxxdj/mixxx/pull/12690)
* Add Shuffle action to track table header menu [#13392](https://github.com/mixxxdj/mixxx/pull/13392)
* Library scan: log summary and show popup
  [#13427](https://github.com/mixxxdj/mixxx/pull/13427)
  [#10720](https://github.com/mixxxdj/mixxx/issues/10720)
* Search: add BPM lock filter `bpm:locked`
  [#14590](https://github.com/mixxxdj/mixxx/pull/14590)
  [#14583](https://github.com/mixxxdj/mixxx/issues/14583)
* Track menu, purge: allow to hide further success popups in the current session [#13807](https://github.com/mixxxdj/mixxx/pull/13807)
* Track Info dialogs: move metadata buttons below color picker [#13632](https://github.com/mixxxdj/mixxx/pull/13632)
* Track File Export: add 'Apply to all' checkbox, remove ".. All" buttons [#13614](https://github.com/mixxxdj/mixxx/pull/13614)
* Fix: restore BPM and Bitrate column width [#13571](https://github.com/mixxxdj/mixxx/pull/13571)
* Elide key text from the right [#13475](https://github.com/mixxxdj/mixxx/pull/13475)
* Playlists: add 'Unlock all' and 'Delete all unlocked' menu actions
  [#14091](https://github.com/mixxxdj/mixxx/pull/14091)
  [#8960](https://github.com/mixxxdj/mixxx/issues/8960)

### Effects

* Compressor effect: Adjust Makeup Time constant calculation [#13261](https://github.com/mixxxdj/mixxx/pull/13261)
[#13237](https://github.com/mixxxdj/mixxx/issues/13237)
* Fix: prevent quickFX model out of bound [#13668](https://github.com/mixxxdj/mixxx/pull/13668)

### Waveforms

* Simplify waveform combobox in preferences
  [#13220](https://github.com/mixxxdj/mixxx/issues/13220)
  [#6428](https://github.com/mixxxdj/mixxx/issues/6428)
  [#13226](https://github.com/mixxxdj/mixxx/issues/13226)
* Add minute markers on horizontal waveform overview
  [#13401](https://github.com/mixxxdj/mixxx/pull/13401)
  [#5843](https://github.com/mixxxdj/mixxx/issues/5843)
  [#13648](https://github.com/mixxxdj/mixxx/pull/13648)
  [#13489](https://github.com/mixxxdj/mixxx/pull/13489)
* Add slip waveform to Textured/'High details' type [#14039](https://github.com/mixxxdj/mixxx/pull/14039)
* Disable textured waveforms when using OpenGL ES
  [#13381](https://github.com/mixxxdj/mixxx/pull/13381)
  [#13380](https://github.com/mixxxdj/mixxx/issues/13380)
* Waveform Overview: Scale by ReplayGain
  [#14309](https://github.com/mixxxdj/mixxx/pull/14309)
  [#14331](https://github.com/mixxxdj/mixxx/pull/14331)
* feat: improve screen rendering framework [#13737](https://github.com/mixxxdj/mixxx/pull/13737)
* Rendergraph: Add rendergraph library and use if for waveform rendering
  [#14007](https://github.com/mixxxdj/mixxx/pull/14007)
  [#14021](https://github.com/mixxxdj/mixxx/pull/14021)
  [#14191](https://github.com/mixxxdj/mixxx/pull/14191)
  [#14185](https://github.com/mixxxdj/mixxx/pull/14185)
  [#14188](https://github.com/mixxxdj/mixxx/pull/14188)
  [#14192](https://github.com/mixxxdj/mixxx/pull/14192)
  [#14190](https://github.com/mixxxdj/mixxx/pull/14190)
  [#14186](https://github.com/mixxxdj/mixxx/pull/14186)
  [#14189](https://github.com/mixxxdj/mixxx/pull/14189)
  [#14187](https://github.com/mixxxdj/mixxx/pull/14187)
  [#13470](https://github.com/mixxxdj/mixxx/pull/13470)
  [#14461](https://github.com/mixxxdj/mixxx/pull/14461)
  [#14726](https://github.com/mixxxdj/mixxx/pull/14726)
  [#14706](https://github.com/mixxxdj/mixxx/issues/14706)
* Improve apperrance of marks on the waveforms [#13969](https://github.com/mixxxdj/mixxx/pull/13969)
* ControllerRenderingEngine: Patch out unavailable APIs when using GL ES [#13382](https://github.com/mixxxdj/mixxx/pull/13382)
* Fix high details waveforms wrapping around after visual index 65K [#13491](https://github.com/mixxxdj/mixxx/pull/13491)
* Fix: support for new WaveformData struct in shaders
  [#13474](https://github.com/mixxxdj/mixxx/pull/13474)
  [#13472](https://github.com/mixxxdj/mixxx/issues/13472)
* Fix: remove scaleSignal in waveform analyzer [#13416](https://github.com/mixxxdj/mixxx/pull/13416)
* Fix: prevent double free on DigitsRenderer [#13859](https://github.com/mixxxdj/mixxx/pull/13859)
* Fix: waveform overview seeking
  [#13947](https://github.com/mixxxdj/mixxx/pull/13947)
  [#13946](https://github.com/mixxxdj/mixxx/issues/13946)
* Fix invalid slip render marker [#13422](https://github.com/mixxxdj/mixxx/pull/13422)
* Fix waveform marker image alignment
  [#14656](https://github.com/mixxxdj/mixxx/pull/14656)
  [#14037](https://github.com/mixxxdj/mixxx/issues/14037)

### Auto-DJ

* Add transition mode 'Skip Silence, Start with Xfader centered' [#13628](https://github.com/mixxxdj/mixxx/pull/13628)
* Add crossafder recenter option when turning off (default off)
  [#13303](https://github.com/mixxxdj/mixxx/pull/13303)
  [#11571](https://github.com/mixxxdj/mixxx/issues/11571)
* Add context menu action for enabling/disabling the Auto DJ [#13593](https://github.com/mixxxdj/mixxx/pull/13593)

### Controller Mappings

* Behringer DDM4000 & BCR2000: Remove XML input declarations from mapping [#14285](https://github.com/mixxxdj/mixxx/pull/14285)
* Hercules DJ Control Starlight: Add EffectChain superknob control [#14126](https://github.com/mixxxdj/mixxx/pull/14126)
* Numark Mixtrack 3: Update scripts [#14193](https://github.com/mixxxdj/mixxx/pull/14193)
* Traktor S3: Small updates and fixes [#14340](https://github.com/mixxxdj/mixxx/pull/14340)

### Controller Backend

* Add screen renderer to support controllers with a screen
  [#11407](https://github.com/mixxxdj/mixxx/pull/11407)
  [#13334](https://github.com/mixxxdj/mixxx/pull/13334)
* Deprecate `lodash.mixxx.js`, and `script.deepMerge` [#13460](https://github.com/mixxxdj/mixxx/pull/13460)
* Add New CO "beats_translate_half" to move beatgrid a half beat
  [#14279](https://github.com/mixxxdj/mixxx/pull/14279)
  [#10811](https://github.com/mixxxdj/mixxx/issues/10811)
* Settings: Add a file and color controller setting types [#13669](https://github.com/mixxxdj/mixxx/pull/13669)
* Allow to enable MIDI Through Port in non-developer sessions [#13909](https://github.com/mixxxdj/mixxx/pull/13909)
* Refactor: modernize softtakeover code [#13553](https://github.com/mixxxdj/mixxx/pull/13553)
* document `ScriptConnection` readonly properties & slight cleanup [#13630](https://github.com/mixxxdj/mixxx/pull/13630)
* Modernize Hid/Bulk Lists [#13622](https://github.com/mixxxdj/mixxx/pull/13622)
* Prevent deadlock with BULK transfer and reduce log noise [#13735](https://github.com/mixxxdj/mixxx/pull/13735)
* Expose convertCharset convenience function to controllers
  [#13935](https://github.com/mixxxdj/mixxx/pull/13935)
  [#14108](https://github.com/mixxxdj/mixxx/pull/14108)
* Add HID error message upon failed open [#14184](https://github.com/mixxxdj/mixxx/pull/14184)
* Remove boilerplate and duplication in controller setting definition [#13920](https://github.com/mixxxdj/mixxx/pull/13920)
* Allow feedback on every release of a `powerWindow` button [#14335](https://github.com/mixxxdj/mixxx/pull/14335)
* Controller Settings: Improve click event filter [#14355](https://github.com/mixxxdj/mixxx/pull/14355)
* Controller Settings: Add a collapsible group box [#14324](https://github.com/mixxxdj/mixxx/pull/14324)
* Fix: Don't return in JogWheelBasic on deck absent in option
  [#13425](https://github.com/mixxxdj/mixxx/pull/13425)
  [#14106](https://github.com/mixxxdj/mixxx/pull/14106)

### Engine

* Fix: sync rate using the current BPM instead of the file one
  [#13671](https://github.com/mixxxdj/mixxx/pull/13671)
  [#12738](https://github.com/mixxxdj/mixxx/issues/12738)
* Sync: prefer playing inaudible decks over stopped non-sync decks [#14580](https://github.com/mixxxdj/mixxx/pull/14580)
* Fix: prevent null CO access when cloning sampler or preview [#13740](https://github.com/mixxxdj/mixxx/pull/13740)
* Use correct detected channel count on CoreAudio [#14372](https://github.com/mixxxdj/mixxx/pull/14372)

### Preferences

* Waveforms: Group options, adjust tabstops, reorder ui file [#13615](https://github.com/mixxxdj/mixxx/pull/13615)
* Controllers: Make extended controller information available for device selection
* Controllers: Reorganize content into tabs
* Mixer: Show 'real' crossfader configuration [#14124](https://github.com/mixxxdj/mixxx/pull/14124)
* Mixer: Fix crossader graph [#13848](https://github.com/mixxxdj/mixxx/pull/13848)
  [#13896](https://github.com/mixxxdj/mixxx/pull/13896)
  [#14006](https://github.com/mixxxdj/mixxx/pull/14006)
  [#14354](https://github.com/mixxxdj/mixxx/pull/14354)
* Effects: Left/Right key in effect lists trigger hide/unhide [#14205](https://github.com/mixxxdj/mixxx/pull/14205)
* Sound Hardware: Open with sprecific I/O tab selected [#14346](https://github.com/mixxxdj/mixxx/pull/14346)
* Sound Hardware: Don't set m_settingsModified in update slots [#13450](https://github.com/mixxxdj/mixxx/pull/13450)
* Library, Track Search: Fix accidental use of wrong preference controls [#13592](https://github.com/mixxxdj/mixxx/pull/13592)

### Skins

* Allow swapping hotcues via dragging and dropping hotcue buttons
  [#13394](https://github.com/mixxxdj/mixxx/pull/13394)
  [#14367](https://github.com/mixxxdj/mixxx/pull/14367)
* Add controls to order Hotcues by position in the track
  [#13808](https://github.com/mixxxdj/mixxx/pull/13808)
  [#14423](https://github.com/mixxxdj/mixxx/pull/14423)
* Drop Hotcue onto Play button to latch `play`
  [#14179](https://github.com/mixxxdj/mixxx/pull/14179)
  [#14178](https://github.com/mixxxdj/mixxx/pull/14178)
* Always show tooltips if Ctrl key is pressed [#14078](https://github.com/mixxxdj/mixxx/pull/14078)
* Update waveforms_container.xml [#13501](https://github.com/mixxxdj/mixxx/pull/13501)
* LegacySkinParser: Short-circuit if template fails to open [#13488](https://github.com/mixxxdj/mixxx/pull/13488)
* Tooltips: Fix cue mode setting location [#14045](https://github.com/mixxxdj/mixxx/pull/14045)

### Experimental Features

* SoundManagerIOS: Remove unsupported/redundant options [#13487](https://github.com/mixxxdj/mixxx/pull/13487)
* ControllerRenderingEngine: Disable BGRA when targeting Wasm [#13502](https://github.com/mixxxdj/mixxx/pull/13502)
* BaseTrackTableModel: Disable inline track editing on iOS [#13494](https://github.com/mixxxdj/mixxx/pull/13494)
* Set QQuickStyle to "basic" [#13696](https://github.com/mixxxdj/mixxx/pull/13696)
  [#13600](https://github.com/mixxxdj/mixxx/issues/13600)
* Fix: trigger QML waveform slot at init [#13736](https://github.com/mixxxdj/mixxx/pull/13736)
* CoreServices: Default to `~/Music` as a music directory on WASM and iOS [#13498](https://github.com/mixxxdj/mixxx/pull/13498)
* CMakeLists: Disable `QTlsBackendOpenSSLPlugin` on iOS [#14375](https://github.com/mixxxdj/mixxx/pull/14375)

### Target support

* Sound preferences: Add missing ifdefs for building without Rubberband [#13577](https://github.com/mixxxdj/mixxx/pull/13577)
* Update Linux-GitHub runner to Ubuntu 24.04.01 LTS
  [#13781](https://github.com/mixxxdj/mixxx/pull/13781)
  [#13880](https://github.com/mixxxdj/mixxx/pull/13880)
* Debiam: Add missing qt6-declarative-private-dev and qt6-base-private-dev package
  [#13904](https://github.com/mixxxdj/mixxx/pull/13904)
* RPM: Add missing deps [#14183](https://github.com/mixxxdj/mixxx/pull/14183)
* Show translator file path in debug message [#14209](https://github.com/mixxxdj/mixxx/pull/14209)
* Building without tests-tools [#14268](https://github.com/mixxxdj/mixxx/pull/14268)
* Remove unmaintained shell.nix [#14300](https://github.com/mixxxdj/mixxx/pull/14300)
* Add QGLES2 option for UNIX [#14489](https://github.com/mixxxdj/mixxx/pull/14489)
* Don't set GL_BGRA if QT_OPENGL_ES_2 [#14488](https://github.com/mixxxdj/mixxx/pull/14488)
* Windows and macOS: Update to Qt 6.8.3 (requires MSVC 2022) [#14655](https://github.com/mixxxdj/mixxx/pull/14655)

### Misc Refactorings

* Refactor/shrink modernize scopedtimer [#13258](https://github.com/mixxxdj/mixxx/pull/13258)
* Improve use of parented_ptr [#13411](https://github.com/mixxxdj/mixxx/pull/13411)
* Pre-allocate memory in basetrackcache to avoid multiple reallocations [#13368](https://github.com/mixxxdj/mixxx/pull/13368)
* Bump actions/checkout from 4.1.6 to 4.1.7 [#13386](https://github.com/mixxxdj/mixxx/pull/13386)
* Bump actions/checkout from 4.1.7 to 4.2.0 [#13713](https://github.com/mixxxdj/mixxx/pull/13713)
* Bump actions/checkout from 4.2.0 to 4.2.1 [#13726](https://github.com/mixxxdj/mixxx/pull/13726)
* Bump actions/checkout from 4.2.1 to 4.2.2 [#13810](https://github.com/mixxxdj/mixxx/pull/13810)
* Bump azure/trusted-signing-action from 0.3.20 to 0.4.0 [#13500](https://github.com/mixxxdj/mixxx/pull/13500)
* Bump azure/trusted-signing-action from 0.4.0 to 0.5.0 [#13809](https://github.com/mixxxdj/mixxx/pull/13809)
* Bump actions/upload-artifact from 4.3.4 to 4.3.5 [#13539](https://github.com/mixxxdj/mixxx/pull/13539)
* Bump actions/upload-artifact from 4.3.5 to 4.3.6 [#13562](https://github.com/mixxxdj/mixxx/pull/13562)
* Bump actions/upload-artifact from 4.3.6 to 4.4.0 [#13621](https://github.com/mixxxdj/mixxx/pull/13621)
* Bump actions/upload-artifact from 4.4.0 to 4.4.1 [#13725](https://github.com/mixxxdj/mixxx/pull/13725)
* Bump actions/upload-artifact from 4.4.1 to 4.4.3 [#13765](https://github.com/mixxxdj/mixxx/pull/13765)
* Bump coverallsapp/github-action from 2.3.0 to 2.3.1 [#13766](https://github.com/mixxxdj/mixxx/pull/13766)
* Bump coverallsapp/github-action from 2.3.1 to 2.3.3 [#13793](https://github.com/mixxxdj/mixxx/pull/13793)
* Bump coverallsapp/github-action from 2.3.3 to 2.3.4 [#13811](https://github.com/mixxxdj/mixxx/pull/13811)
* Bump actions/upload-artifact from 4.3.3 to 4.6.0 [#14167](https://github.com/mixxxdj/mixxx/pull/14167)
* Bump azure/trusted-signing-action from 0.5.0 to 0.5.1 [#14168](https://github.com/mixxxdj/mixxx/pull/14168)
* Bump coverallsapp/github-action from 2.3.4 to 2.3.6 [#14246](https://github.com/mixxxdj/mixxx/pull/14246)
* Bump actions/upload-artifact from 4.6.0 to 4.6.1 [#14406](https://github.com/mixxxdj/mixxx/pull/14406)
* chore: update the donate button label [#13353](https://github.com/mixxxdj/mixxx/pull/13353)
* WPixmapStore: Change getPixmapNoCache to std::unique_ptr and further optimizations [#13369](https://github.com/mixxxdj/mixxx/pull/13369)
* Remove unused setSVG and hash functionality from pixmapsource [#13423](https://github.com/mixxxdj/mixxx/pull/13423)
* Remove FAQ from Readme.md [#13453](https://github.com/mixxxdj/mixxx/pull/13453)
* [#13452](https://github.com/mixxxdj/mixxx/pull/13452)
* Make Paintable::DrawMode an enum class [#13424](https://github.com/mixxxdj/mixxx/pull/13424)
* Paintable cleanup [#13435](https://github.com/mixxxdj/mixxx/pull/13435)
* hash clean up [#13458](https://github.com/mixxxdj/mixxx/pull/13458)
* BaseTrackTableModel: Fix `-Wimplicit-fallthrough` warning on GCC 14.1.1 [#13505](https://github.com/mixxxdj/mixxx/pull/13505)
* Refactor: fix trivial cpp coreguideline violations [#13552](https://github.com/mixxxdj/mixxx/pull/13552)
* Refactor `EngineMixer`  [#13568](https://github.com/mixxxdj/mixxx/pull/13568)
* more `ControlDoublePrivate` optimization [#13581](https://github.com/mixxxdj/mixxx/pull/13581)
* Modernize `ControlValueAtomic`  [#13574](https://github.com/mixxxdj/mixxx/pull/13574)
* Optimize control code [#13354](https://github.com/mixxxdj/mixxx/pull/13354)
* Fix some minor code issue [#13586](https://github.com/mixxxdj/mixxx/pull/13586)
* Static initialization order fix [#13594](https://github.com/mixxxdj/mixxx/pull/13594)
* Remove referenceholder [#13240](https://github.com/mixxxdj/mixxx/pull/13240)
* chore: add note about ConfigKey naming convention [#13658](https://github.com/mixxxdj/mixxx/pull/13658)
* refactor: split out `AutoFileReloader` from `QmlAutoReload`
  [#13607](https://github.com/mixxxdj/mixxx/pull/13607)
  [#13756](https://github.com/mixxxdj/mixxx/pull/13756)
  [#13755](https://github.com/mixxxdj/mixxx/issues/13755)
* Fix Clazy v1.12 errors in main [#13770](https://github.com/mixxxdj/mixxx/pull/13770)
* Code cleanup in SidebarModel and WLibrarySidebar [#13816](https://github.com/mixxxdj/mixxx/pull/13816)
* Refactor `MovingInterquartileMean` [#13730](https://github.com/mixxxdj/mixxx/pull/13730)
* Improved comments in enginecontrol and use of std::size_t for bufferSize across the codebase [#13819](https://github.com/mixxxdj/mixxx/pull/13819)
* Refactor: use higher-level `std::span` based logic [#13654](https://github.com/mixxxdj/mixxx/pull/13654)
* VSyncThread: tsan fix pll vars data race [#13873](https://github.com/mixxxdj/mixxx/pull/13873)
* Control Indicator: Use atomic to fix tsan detected data race condition of blink value [#13875](https://github.com/mixxxdj/mixxx/pull/13875)
* Fix undefined behaviour of infinity() [#13884](https://github.com/mixxxdj/mixxx/pull/13884)
* Use atomic for m_bWakeScheduler, protect m_bQuit with mutex [#13898](https://github.com/mixxxdj/mixxx/pull/13898)
* Refactor `ValueTransformer` and `WBaseWidget` [#13853](https://github.com/mixxxdj/mixxx/pull/13853)
* Avoid data race on m_pStream [#13899](https://github.com/mixxxdj/mixxx/pull/13899)
* Cleanup and deprecate more `util/` classes
  [#13687](https://github.com/mixxxdj/mixxx/pull/13687)
  [#13968](https://github.com/mixxxdj/mixxx/pull/13968)
  [#13965](https://github.com/mixxxdj/mixxx/issues/13965)
  [#14107](https://github.com/mixxxdj/mixxx/pull/14107)
  [#14095](https://github.com/mixxxdj/mixxx/issues/14095)
  [#14087](https://github.com/mixxxdj/mixxx/pull/14087)
  [#14086](https://github.com/mixxxdj/mixxx/issues/14086)
* Github CI(clang-format): Indent Objective-C blocks with 4 spaces [#13503](https://github.com/mixxxdj/mixxx/pull/13503)
* Github CI(pre-commit): Add cmake-lint hook [#13932](https://github.com/mixxxdj/mixxx/pull/13932)
* Github CI(labeler): Add `developer experience` issue label [#14343](https://github.com/mixxxdj/mixxx/pull/14343)
* Github CI(labeler): add Dev Tools to `developer experience` [#14475](https://github.com/mixxxdj/mixxx/pull/14475)
* Refactor: remove samplew_autogen.h
  [#13988](https://github.com/mixxxdj/mixxx/pull/13988)
  [#14005](https://github.com/mixxxdj/mixxx/pull/14005)
* Fix clang-tidy complain [#14029](https://github.com/mixxxdj/mixxx/pull/14029)
* Github CI(dependabot): Open PRs against 2.5 branch instead of main [#14060](https://github.com/mixxxdj/mixxx/pull/14060)
* Happy New Year 2025! [#14098](https://github.com/mixxxdj/mixxx/pull/14098)
* Fix warning in Auto DJ test  [#14102](https://github.com/mixxxdj/mixxx/pull/14102)
* Fix: Add `QT_VERSION_CHECK`ed `QCheckBox::checkStateChanged` handlers [#14104](https://github.com/mixxxdj/mixxx/pull/14104)
* Remove warning introduced in [#13339](https://github.com/mixxxdj/mixxx/pull/13339) [#14109](https://github.com/mixxxdj/mixxx/pull/14109)
* Fix wrong access to ENV var MIXXX_VCPKG_ROOT instead of CMake setting MIXXX_VCPKG_ROOT [#14146](https://github.com/mixxxdj/mixxx/pull/14146)
* WOverview: remove unused coefficients [#14145](https://github.com/mixxxdj/mixxx/pull/14145)
* Fix missing initialization in Rotary() [#14176](https://github.com/mixxxdj/mixxx/pull/14176)
* WPushButton: remove obsolete focusOutEvent() [#14177](https://github.com/mixxxdj/mixxx/pull/14177)
* Fix memory leak and use parented_ptr in WTrackMenu [#14199](https://github.com/mixxxdj/mixxx/pull/14199)
* Fix Clazy warning in main [#14241](https://github.com/mixxxdj/mixxx/pull/14241)
* Fix recently introduced clazy warnings [#14336](https://github.com/mixxxdj/mixxx/pull/14336)
* Fix calculation of m_resourcePath in the testing case [#14110](https://github.com/mixxxdj/mixxx/pull/14110)
* Add borrowable_ptr, a threadsafe callback solution [#1713](https://github.com/mixxxdj/mixxx/pull/1713)
* Include QtConcurrentRun [#14303](https://github.com/mixxxdj/mixxx/pull/14303)
* Optimized tooltip generation in WBaseWidget [#13952](https://github.com/mixxxdj/mixxx/pull/13952)
* Github CI(pre-commit): qsscheck.py -> added utf-8 in open() [#14320](https://github.com/mixxxdj/mixxx/pull/14320)
* Small grammar fix for comment in `BpmControl::slotUpdateRateSlider` [#14344](https://github.com/mixxxdj/mixxx/pull/14344)
* Add links to important guidelines to CONTRIBUTING.md [#14342](https://github.com/mixxxdj/mixxx/pull/14342)
* Log Test fixes and refactoring.  [#14111](https://github.com/mixxxdj/mixxx/pull/14111)
* chore: lint CMakeLists.txt [#14369](https://github.com/mixxxdj/mixxx/pull/14369)
* RateControl/Position ScratchController: use std::unique_ptr, PollingControlProxy etc. [#14058](https://github.com/mixxxdj/mixxx/pull/14058)
* Controller preferences: Cond-compile out HID settings when building without HID [#14376](https://github.com/mixxxdj/mixxx/pull/14376)
* Add missing space to engine controller API documentation [#14384](https://github.com/mixxxdj/mixxx/pull/14384)
* Fix prettier pre commit [#14416](https://github.com/mixxxdj/mixxx/pull/14416)
* Use std::shared_ptr in  controller settings to fix memory leak [#14413](https://github.com/mixxxdj/mixxx/pull/14413)
* clean up README.md [#14471](https://github.com/mixxxdj/mixxx/pull/14471)
* Fix type safety warnings [#14613](https://github.com/mixxxdj/mixxx/pull/14613)
* CMake: Join project() with enable_language() [#14577](https://github.com/mixxxdj/mixxx/pull/14577)
* Scenegraph: condition to QML=ON  [#14487](https://github.com/mixxxdj/mixxx/pull/14487)
* Fix building with Qt 6.9 [#14678](https://github.com/mixxxdj/mixxx/pull/14678)
* Fix: import proper QtQml.Models module instead of qmllabs [#14675](https://github.com/mixxxdj/mixxx/pull/14675)
* qmlwaveform: Fix moc in Qt 6.9.0 [#14649](https://github.com/mixxxdj/mixxx/pull/14649)
