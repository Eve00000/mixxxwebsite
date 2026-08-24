title: "GSoC 2026: Canonical Master Release Track (CMRT) based on the Complete Chromaprint Fingerprint"
authors: Swarnadip Kar (Swarnadip-Kar)
status: draft
tags: gsoc, gsoc-2026, development, CMRT, fingerprinting, MusicBrainz
comments: yes
summary: The solution is to use the complete Chromaprint fingerprint — the full duration, not just the first two minutes — as an identifier for a specific mastering.

**Organization:** Mixxx  
**Applicant:** Swarnadip Kar  
**Institution:** IIT Bhilai, B.Tech Computer Science (4th Year)  
**GitHub:** [Swarnadip-Kar](https://github.com/Swarnadip-Kar)  
**Timezone:** IST (UTC+5:30)  
**Mentor:** Evelynne

**Original GSoC Idea:** [GSoC 2026 Ideas: CMRT](https://github.com/mixxxdj/mixxx/wiki/GSOC-2026-Ideas#cannonical-master-release-track-based-on-the-complete-chroma-fingerprint-of-the-track)

**Original Proposal:** [Swarnadip_Kar_GSoC_26_CMRT_Proposal](https://docs.google.com/document/d/1tQYtuqihsSikyzlzEMVYfCW3zfjMNIASP3E1lBUkZ60/edit?tab=t.0#heading=h.b8iy8bwsq268)

---

Hello Everyone! I am Swarnadip Kar, a 4th-year Computer Science undergraduate at IIT Bhilai. 
I joined the Mixxx community in February 2026. Through this post I want to share my GSoC 2026 project with the community: **Canonical Master Release Track (CMRT) based on the Complete Chromaprint Fingerprint**.

---

## The Problem

### DJ-Intelligence: Why This Project Exists

This project forms the base for a bigger vision we call **DJ-Intelligence (DJ-I)** — the idea of sharing the next level of metadata: data and information that particularly interests DJs and music professionals. Things like POIs (Points of Interest) such as BPM shift points, audio start/end, cue points, key shift points, beat grids, downbeats, genres, ratings, and so on.

Currently every DJ software ships its own analyzers and its own interpretation of the results. There is no single "source of truth" to give a DJ correct BPM values (including different segments within a track) or correct musical keys (including key changes). If we want to share and exchange this kind of advanced metadata between DJs — through MusicBrainz/ListenBrainz for instance — we first need to solve a fundamental question: **are we actually talking about the same audio?**

Together with the companion GSoC idea "Graphical representation of BPM and Musical Keys in the waveform", this project lays the groundwork for DJ-I. The graphical project will need to save BPM curves, key curves, and POI data in an interchangeable format — and that data must be perfectly linked to a CMRT so it can be recreated on other systems and other files of the same mastering.

### Recording vs. Mastering: The Core Distinction

MusicBrainz defines a **[recording](https://musicbrainz.org/doc/Recording)** as a unique audio event — a particular performance captured in a studio or live. A single recording (identified by its MBID — MusicBrainz Identifier) can have many different **masterings**: the version on the original album, the 7-inch single cut, the "best of" compilation version, the streaming-optimized version, the music video version, the Various Artists compilation master, and so on.

A recording is not equal to a mastering. Each mastering may differ in loudness, EQ, dynamic range, intro/outro length, or subtle timing shifts introduced during the mastering process. For a DJ, these differences matter — a beat grid computed on the original album version of a track will not line up perfectly with the "best of" remaster, even though both are the "same song."

### What Is a Canonical Master Release Track?

The solution is to use the *complete* Chromaprint fingerprint — the full duration, not just the first two minutes — as an identifier for a specific mastering. Two copies of a track with identical complete fingerprints are provably the same mastering. From there the pregap offset between any two masterings can be calculated, and eventually this information can be shared through MusicBrainz so DJs worldwide can retrieve timing metadata that is guaranteed to match their specific version of a track.

Once we can identify tracks as being the same mastering, we can choose one version as the **Canonical Master Release Track (CMRT)** — the highest quality version of that mastering available. In the scope of this project (a DJ's local collection), the CMRT is simply the highest-quality file for a given mastering. For example, if a DJ has both a 128 kbps MP3 and a FLAC of the same mastering, the FLAC is the local CMRT. The offset of all other tracks of the same mastering needs to be calculated relative to this CMRT.

In a broader scope (across all users contributing to MusicBrainz), the CMRT would be the highest-quality version of each mastering that has been fingerprinted and registered — the data of highest quality available across the collections of all DJs, stored in the MBDB. This larger-scope CMRT subclustering is the separate MusicBrainz project mentioned above.

When we detect mastering duplicates in a DJ's collection, we suggest the DJ keep the CMRT (highest quality) and optionally remove the lower-quality duplicates.

This project builds the entire Mixxx-side foundation: database schema, fingerprint storage, background AcoustID lookup, preferences UI, queue view, and WTrackMenu integration.

---

## Check Out the Demo Video

@Video(https://www.youtube.com/watch?v=s_Cm_eHa4Vk)
(Video Credits: [@Eve00000](https://github.com/Eve00000))

---

## Call for Community Testing and Feedback

The Final PR ([#16771](https://github.com/mixxxdj/mixxx/pull/16771)) closes out the full feature set — fingerprinting, AcoustID lookup, mastering-duplicate grouping, canonical election, the library column, the data overlay, hotcue transfer, and manual re-election. Calling on the community to pull the branch and put it through real collections.

A few things worth knowing before testing:

- **You need files that are the same mastering (or very close to it).** Convert lossless tracks to lossy versions / stem versions and add these to your Mixxx collection. Grouping and offset calculation only mean something when compared against a real duplicate mastering of the same recording — unrelated tracks won't group, and testing against them won't tell you anything about offset accuracy.
- **Overlay/offset accuracy is easiest to judge with files that have a known, predefined offset** between them. You can eyeball it with a good beat sense against files with an unknown offset, but for real accuracy testing, use pairs where the true offset is already known ahead of time.
- **Match score is a decent predictor of offset accuracy.** In our own testing, match scores above ~85% consistently gave accurate offsets; below that, offset accuracy starts to drift.

Huge credit to Daniel for the original offset POC ([#15585](https://github.com/mixxxdj/mixxx/pull/15585)) that `FingerprintMatcher` builds on.

---

## Phase 1 — Foundation and AcoustID Pipeline

Phase 1 establishes everything required to compute, store, and look up fingerprints end-to-end. By the end of this phase, Mixxx can:

- Compute a complete Chromaprint fingerprint for any track in the analyzer pipeline
- Store it on disk (`.chroma` file) and index it in the database (SimHash + SHA-256)
- Submit it to AcoustID in the background and write the returned MusicBrainz IDs back to the library
- Expose fingerprint controls in Preferences, the right-click track menu, and a dedicated sidebar queue view

Phase 2 adds mastering duplicate detection, pregap/offset calculation, and beat-grid transfer between tracks that share the same CMRT.

---

## Phase 2 — Mastering-Duplicate Detection, Library UI, and Manual Control

Phase 2 builds mastering-duplicate detection on top of Phase 1's fingerprint storage, then surfaces it in the library UI. It goes on to:

- Identify tracks that are the same mastering via MBID-filtered SimHash pre-filtering and full fingerprint comparison
- Automatically group duplicate masterings and elect the highest-quality file as the Canonical Master Release Track (CMRT), re-electing on deletion
- Show CMRT grouping directly in the library table, with per-field display control
- Let a lower-quality member load its canonical track's beatgrid, cues, and hotcues instead of its own, non-destructively
- Let the user manually override canonical selection

---

## Want to Dive Deeper?

Everything above is the high-level picture. If you're a developer, reviewer, or just curious about the schema changes, pull requests, and implementation details behind CMRT, follow the full technical tracker on GitHub: [GSoC 2026: Canonical Master Release Track (CMRT) based on the Complete Chromaprint Fingerprint](https://github.com/mixxxdj/mixxx/issues/16607).