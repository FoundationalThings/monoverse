# MonoVerse

(as authored by Claude Sonnet 4.6)

> One verse. Full screen. Nothing else.

A contemplative Bible reading experience designed to slow you down and let a single verse breathe. Built legacy-first — works on old iOS devices, modern browsers, and everything in between.

---

## What it is

MonoVerse presents one Bible verse at a time, full screen, with generous space and careful typography. Navigate sequentially through the entire Bible, or jump directly to any book, chapter, and verse.

This is an early, open development version. Things will change. Feedback welcome.

---

## Using it

Visit **[bible.foundationalthings.com](https://bible.foundationalthings.com)**

- **Next / Previous** — tap the arrows, swipe left/right, or use keyboard arrow keys
- **Jump to a verse** — use the Book · Chapter · Verse dropdowns at the top
- Works in any modern browser, and on older iOS devices via Safari

---

## Data

Bible text is the **Berean Standard Bible (BSB)**, served via the [Free Use Bible API](https://bible.helloao.org) by AO Lab. The BSB is dedicated to the public domain.

> _The Holy Bible, Berean Standard Bible, BSB is produced in cooperation with Bible Hub, Discovery Bible, OpenBible.com, and the Berean Bible Translation Committee. This text of God's Word has been dedicated to the public domain._

---

## Technical

- Single `index.html` — no build step, no framework, no dependencies
- ES5 JavaScript — compatible with legacy iOS Safari
- Live API fetch, chapter-level caching in memory
- Binary search font sizing — verse text fills a proportional stage box
- Responsive: landscape primary, portrait supported, phone to widescreen

---

## Status

Early POC / spike. Developed openly. More to come.

---

## Roadmap ideas

- Offline support
- Long verse splitting (3a / 3b)
- Context mode
- Personal verse tagging and collections
- Presentation mode for worship leaders
- Poetry display mode
- User theming

---

_Built with care. Developed in plain sight._
