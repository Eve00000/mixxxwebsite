title: "GSoC 2026: Rebuilding the LateNight Theme in QML"
authors: Ayush Sah
status: draft
tags: gsoc, gsoc-2026, development, UI, QML, LateNight
comments: yes
summary: Introducing the Google Summer of Code 2026 project to port and rebuild Mixxx's beloved LateNight skin as a native, modern, and highly-performant QML theme.

Hi everyone! I am Ayush Sah, and I am thrilled to share that I will be working with Mixxx for Google Summer of Code (GSoC) 2026. Over the next few months, my project will focus on one of the most exciting shifts in Mixxx’s user interface history: **rebuilding the iconic LateNight theme in native QML**.

As Mixxx paves the way toward the landmark 3.0 release, transitioning its user interface from legacy QWidgets to [Qt Quick / QML](https://en.wikipedia.org/wiki/QML) is a core priority. This shift will unlock rich aesthetics, superior scaling, touch support, and modular theme customization. My goal is to ensure that LateNight, arguably one of Mixxx's most popular skins, is fully ported to QML with perfect visual and functional parity, serving as a clean blueprint for future community-made QML themes.

---

## Why LateNight & Why QML?

For years, **LateNight** has been a favorite for DJs performing in low-light environments. Its clean, dark layout, compact decks, and highly functional stacked waveforms make it both practical and visually appealing. However, the legacy skin system is driven by complex XML and QSS (Qt Style Sheets), which can be difficult to maintain, lack native fluid animations, and do not scale gracefully on modern High-DPI screens or mobile/touchscreen devices.

By porting LateNight to native QML, we aim to achieve:

1. **Modern Typography & Pixel-Perfect Scaling**: Dynamic scaling across 4K displays, retina screens, and touchscreen laptops.
2. **Superior Performance**: Leveraging GPU acceleration for UI rendering, leaving the CPU free to handle real-time audio and waveforms.
3. **Theme Extensibility**: Standardized, modular structure (`res/qml/themes/LateNight`) that other developers can use to construct their own custom layouts without duplicating core logic.

Ultimately, releasing this experimental LateNight QML skin in a 2.x release is just the beginning. Our goal is to give you a hands-on preview of what lies ahead: the plan for Mixxx 3.0 is to leverage powerful new QML capabilities, bringing completely re-written libraries, modern preference windows, fully custom hotcue layouts, and many more exciting visual and functional features.

---

## Milestone 1: Legacy Library Integration Completed!

I am incredibly excited to share that the first core milestone of this project has already been completed and merged in [PR #16489](https://github.com/mixxxdj/mixxx/pull/16489)! This PR successfully bridges Mixxx's legacy QWidget-based Library into the new QML engine.

Here is a sneak peek at the first draft of the LateNight QML theme in action:

![LateNight QML First Draft]({static}/images/news/latenight_qml_first_draft.png)

### Check out the Demo Video:

<video width="100%" controls>
  <source src="https://files.catbox.moe/vxzpkf.mp4" type="video/mp4">
  Your browser does not support HTML5 video.
</video>

---

## How to Test It Right Now

If you are a developer or a curious user who builds Mixxx from source, you can try out the new experimental LateNight QML skin today!

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
