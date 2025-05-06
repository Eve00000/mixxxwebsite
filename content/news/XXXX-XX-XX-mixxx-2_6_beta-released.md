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
