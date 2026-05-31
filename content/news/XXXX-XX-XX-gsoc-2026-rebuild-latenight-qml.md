title: "GSoC 2026: Rebuilding the LateNight Theme in QML"
authors: Ayush Sah (arsenic)
status: draft
tags: gsoc, gsoc-2026, development, UI, QML, LateNight
comments: yes
summary: Introducing the Google Summer of Code 2026 project to port and rebuild Mixxx's beloved LateNight skin as a native, modern, and highly-performant QML theme.

Hi everyone! I am Ayush Sah, and I am thrilled to share that I will be working with Mixxx for Google Summer of Code (GSoC) 2026. Over the next few months, my project will focus on one of the most exciting shifts in Mixxx’s user interface history: **rebuilding the iconic LateNight theme in native QML**.

As Mixxx paves the way toward the landmark 3.0 release, transitioning its user interface to the ["New UI"](https://mixxx.org/news/2025-08-06-qml-project/) is a core priority. Built on [Qt Quick / QML](https://en.wikipedia.org/wiki/QML), this shift unlocks modern aesthetics, fluid layout customization, and vector-based scaling. My goal is to ensure that LateNight, arguably one of Mixxx's most popular skins, is fully ported to QML with perfect visual and functional parity, serving as a clean blueprint for future community-made QML themes.

---

## Why LateNight & Why QML?

For years, **LateNight** has been a favorite for DJs performing in low-light environments. Its clean, dark layout, compact decks, and highly functional stacked waveforms make it both practical and visually appealing. However, the legacy skin system is driven by complex XML and QSS (Qt Style Sheets), which can be difficult to maintain, lack native fluid animations, and do not scale gracefully on modern High-DPI screens or mobile/touchscreen devices.

By porting LateNight to native QML, we aim to achieve:

1. **Modern Typography & Pixel-Perfect Scaling**: Dynamic scaling across 4K displays, retina screens, and touchscreen laptops.
2. **Hardware-Accelerated Rendering**: Leveraging modern, native graphics APIs, specifically **Apple Metal** on macOS and **Microsoft DirectX** on Windows, instead of legacy OpenGL. This keeps the CPU free to focus entirely on real-time audio processing, while the GPU handles UI and waveform rendering.
3. **Theme Extensibility**: Standardized, modular structure (`res/qml/themes/LateNight`) that other developers can use to construct their own custom layouts without duplicating core logic.

---

## Design Choice: Laptops vs. Small Touch Screens

An interesting distinction between the theme layouts is how they address screen space. While the [New UI](https://mixxx.org/news/2025-08-06-qml-project/) features larger but fewer widgets optimized for touchscreen setups and tablet DJs, LateNight is packed with tiny widgets. This compact style is highly optimized for traditional laptop displays and desktop environments, where DJs want to maximize the amount of info and controls visible on a single screen without needing touch gestures. I know there are a lot of LateNight lovers in the community and I hope you all will like this new version of LateNight, a QML version, in which I'm trying my best to restore the same look and feel!

---

## Strategic Scope Reduction for a Stable Release

To ensure we deliver a reliable, fully functional skin by the end of this GSoC project, we have deliberately focused our scope. You might wonder why we are using a legacy library and legacy preferences bridge instead of a native QML implementation.

The main challenge with a fully native QML library is that it requires a new library backend. Because there is no clean separation between the legacy library backend and the legacy library widgets, we would have to rewrite the backend to support native QML. However, writing a new library backend is a massive, independent project on its own.

We can't wait for a new backend to get completed before moving forward with QML and more importantly, we didn't want to risk your music library in the process! So, to keep moving forward with QML without holding the project back we chose a pragmatic middle ground: safely bridging the legacy C++ widgets into the QML viewport via our bridge. This keeps your library robust and completely usable from day one. This concept applies to preferences as well, which is why we are using a legacy preferences bridge for now, which is planned to be adapted to our skin as part of my project.

---

## Planned Project Steps

To let the community know what work is currently planned, here are the main milestones for this project. **Note that our goal is to reuse the QML code from the New UI wherever possible**, as we are primarily building the LateNight-specific layout structures and styling, rather than implementing these widgets from scratch:

**Reusing & inheriting from the New UI**

- **Decks & stacked waveforms**: Porting the compact LateNight deck layout structures while reusing native waveform rendering.
- **Deck transport & parameters**: Wiring the play, cue, sync, loops, rates, hotcues, and beatgrid controls, sharing the New UI's QML logic wherever possible.

**Building shared infrastructure from scratch (benefiting both UIs)**

- **Effects, Sampler & Mic/Aux Racks**: Mounting the samplers and effects slots, reusing the New UI's expandable rack primitives.

**LateNight-specific composition & layout**

- **Legacy Library Bridge & Foundation**: Safely hosting the legacy C++ sidebar and track table inside the QML viewport (Completed).
- **Adapted Legacy Preferences**: Adapting the legacy preferences bridge to the new skin.
- **Channel Mixer**: Styling the EQ/filter columns, VU meters, and crossfader orientation controls by wrapping existing Mixer QML code.
- **Layout Variants**: Designing compact and mini deck sizes specifically tailored for traditional laptop screen heights.
- **Menu Bridge & Accessibility**: Bridging semantic QML menu actions to ensure native platform accessibility support.

---

Ultimately, releasing this experimental LateNight QML skin in a 2.x release is just the beginning. Our goal is to give you a hands-on preview of what lies ahead: the plan for Mixxx 3.0 is to leverage powerful new QML capabilities, bringing completely re-written libraries, modern preference windows, fully custom hotcue layouts, and many more exciting visual and functional features as introduced in the [QML Project announcement](https://mixxx.org/news/2025-08-06-qml-project/).

---

## Milestone 1: Legacy Library Integration Completed!

I am incredibly excited to share that the first core milestone of this project has already been completed and merged in [PR #16489](https://github.com/mixxxdj/mixxx/pull/16489)! This PR successfully bridges Mixxx's legacy QWidget-based Library into the new QML engine.

Here is a sneak peek at the first draft of the LateNight QML theme in action:

![LateNight QML First Draft]({static}/images/news/latenight_qml_first_draft.png)

### Check out the Demo Video:

@Video(https://www.youtube.com/watch?v=i3SaJFgAsCI)

---

## How to Test It Right Now

If you are a developer or a curious user who builds Mixxx from source, you can try out the new experimental LateNight QML skin today! Make sure you are building Mixxx from the latest `main` branch, where the new QML skin work is available.

Follow these steps to run and test it:

1. **Run Mixxx with the Developer Flag**:
   Launch your locally compiled Mixxx build from your terminal using the `--developer` flag:
   ```bash
   ./build/mixxx --developer
   ```
2. **Enable the Skin**:
   Once Mixxx opens, navigate to:
   **Preferences -> Interface** and change the theme/skin to **LateNight QML (Experimental)**. You will have to restart Mixxx once to see the WIP QML skin.
3. **Tip for Accessing Preferences**:
   While using the LateNightQML skin, you can re-access the Mixxx Preferences dialog at any time by **pressing and holding** the settings gear icon.

---

## Get Involved!

This project is fully open source, and I would love to hear feedback and ideas from the community, especially regarding accessibility and touch-screen usability.

- **Follow the Code**: Keep track of all the latest LateNight QML work on our [GitHub repository](https://github.com/mixxxdj/mixxx/pulls?q=sort%3Aupdated-desc+is%3Apr+%22%5BGSoC%5D+LateNightQML%22+).
- **Join the Discussion**: Come chat with us and follow the active project updates in the QML stream on [Zulip](https://mixxx.zulipchat.com/#narrow/channel/561492-project-qml/topic/.5BGSoC.5D.20LateNight.20QML/with/598947695).
- **Contribute**: If you're familiar with QML or C++, we are always looking for reviewers and testers!

Stay tuned for more updates, and happy mixxxing!
