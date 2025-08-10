
title: GSoC 2025 - Multi-Genre & Autocompletion Support Now in Mixxx!
authors: Antonio Giordano GSoC 2025 Contributor
status: draft
tags: GSoC2025, features, library, metadata, genres, multitrack
comments:yes

---

Hello everyone! I’m **Antonio Giordano**, and I’m excited to share the results of my **Google Summer of Code 2025** work on Mixxx. We’ve introduced **true multi-genre support** with smart autocompletion, a cleaner editing experience, and tools to migrate your existing library—without breaking your flow.

## Project Vision & Evolution

Our goal was to give DJs flexible, reliable control over genres—beyond a single free-text field. In earlier versions, you could type multiple genres (e.g., `Dance/Electronic/House`), but Mixxx treated that as **plain text**. It couldn’t understand or manage individual tags.

To get this right, we started with a **solid multi-genre foundation** that’s fast, consistent, and future-friendly. This foundation paves the way for richer features like hierarchical genres.

## What’s New

### 1 Single-Track Genre Editing (Chip UI + Autocomplete)

The **Track Properties** dialog now shows genres as compact **chips** with a white **×** to remove, plus **autocomplete** to add existing genres quickly.

- **Clean look & feel:** compact chips, thin scrollbar, readable contrast.
- **Autocomplete:** type a few letters and pick from existing genres.
- **No more long strings:** add/remove individual genres directly.
- **Always consistent:** the track table shows human-readable names everywhere.

**Note:** If you type a genre that doesn’t exist yet, Mixxx keeps it as a literal. You can map it later using the Orphan Genres tool (see below).

### 2 Multi-Track Editing (Experimental)

You can now edit genres across multiple tracks in one go (experimental):

- **Common view:** shows the **intersection** of genres shared by all selected tracks.
- **Batch operations:** add or remove genres for the whole selection.
- **Safe by design:** unique per-track genres are preserved.

### 3 A Smarter Library: Tree, Counts, and Drag & Drop

Genres now live in a **tree**. Each node shows **track count** and **total duration**. When you select a track, the genres it belongs to become **bold** in the tree—crate-like ergonomics for quick navigation.

- **Drag & drop:** drag tracks from the Tracklist onto a genre to assign it.
- **Quick menu:** right-click a track → **Genres** to (de)select on the fly.
- **Rename safely:** renaming a genre changes the **name**, not the ID, so tracks keep their associations.

### 4 Clean Migration: “Orphan Genres”

If your library contains free-text genres, Mixxx detects **orphans** and helps you clean them up:

- Right-click the **root** of the genre tree → **Edit Orphan Genres**.
- **Add** a new genre or **Link** a string to an existing one.
- Multi-word strings appear as both a full string *and* individual tokens; add the full string first if you want to keep it as a single genre.


## Why This Matters for DJs

**Before:**

- One free-text field. No understanding of individual genres.
- No autocomplete.
- Editing meant retyping a whole string.
- Inconsistent naming (“Electronic” vs “electronic”).
- No real bulk editing.

**Now:**

- **True multi-genre:** each tag is a first-class citizen.
- **Autocomplete:** fast, consistent tagging.
- **Visual editing:** add/remove with chips.
- **Bulk-friendly:** multi-track editor (experimental) for common operations.
- **Library-aware:** a genre tree with counts, duration, and drag & drop.
- **Backwards-compatible:** your library keeps working; migrate at your pace.

## How to Use It

### Single Track

1. Right-click a track → **Properties**.
2. In **Genre**, start typing and pick from autocomplete.
3. Click the **×** to remove a tag.
4. **OK** or **Apply** to save.

### Multi Track (Experimental)

1. Select multiple tracks → right-click → **Properties**.
2. See the **common** genres across the selection.
3. Add or remove tags in bulk.
4. Save: Mixxx preserves track-specific genres.

### Clean Up Old Text Tags

1. In the genre tree, right-click the **root** → **Edit Orphan Genres**.
2. **Add** new genres or **Link** strings to existing ones.
3. Repeat as needed, your sets keep running.

## Under the Hood (Brief)

- **Stable IDs** for genres and a fast **join table** (`genre_tracks`) so filtering and counts are snappy.
- A central **Genre DAO** (data access layer) that maps between IDs and names and normalizes raw data.
- The UI always shows **human-friendly names**; the storage format stays robust behind the scenes.

## Demo video (quick tour)

@Video(https://www.youtube.com/watch?v=5vg1IkYBj2Y)
*This short video bundles all demos: single-track tag chips, multi-track bulk edit, the Genre Tree with drag-and-drop and **F2** rename, and the **Edit Orphan Genres** flow*


## What’s Next

This foundation unlocks a lot of potential. Based on community input, we’re exploring:

- A **full hierarchical** genre tree with multi-level editing.
- **Fuzzy autocomplete** to handle typos gracefully.
- **Search** that also matches genre names.
- Smarter **import/export** of genre structures (share your taxonomy).
- Better **metadata export** across formats for multi-genre fields.
- Optional **auto-conversion** of known strings to stable IDs on import.

## Thank You

Huge thanks to the Mixxx mentors and community for guidance, reviews, and real-world feedback. This is a big step forward, and we’ll keep refining it together.
Please try it and tell us what works for you.

## Learn More

For comprehensive project details:

- **[Complete Project Documentation](https://github.com/mixxxdj/mixxx/issues/14897)**: Full development journey, technical decisions, and lessons learned
- **[GSoC Project Page](https://summerofcode.withgoogle.com/programs/2025/projects/sHcUR4s0)**: Official Google Summer of Code project details

**Feedback & Support:**

- Report issues on GitHub, share ideas on the forums, or chat with us on Zulip. We'd love to hear about your experience with the new genre features:

- **Report Issues**: [GitHub Issues](https://github.com/mixxxdj/mixxx/issues)
- **Share Feedback**: [Community Forums](https://mixxx.discourse.group)
- **Join Discussion**: [Zulip Chat](https://mixxx.zulipchat.com)
- **Feature Requests**: Help shape future enhancements!

---

*Happy mixing!*  
**Antonio Giordano**  
*GSoC 2025 Contributor*
