# 0001. Single-file, client-only app with localStorage

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

Bookmark Burrow is a personal bookmark manager. The requirement is that it runs
as a single-page app with no server: open the file in a browser and it works.

## Decision

- The whole app (HTML, CSS, JavaScript) lives in one file, `index.html`, with no
  build step and no JavaScript dependencies.
- Bookmarks are stored in the browser's `localStorage` under the key
  `bookmark-burrow`, as a JSON array of `{ id, title, url, tags, createdAt }`
  (`tags` added in [0004](0004-lowercase-category-tags.md)).
- IDs come from `crypto.randomUUID()`.
- Bookmark titles and URLs are rendered with `textContent` / DOM properties, never
  `innerHTML`, so stored data can't inject markup.
- URLs without a scheme get `https://`; anything other than `http:` / `https:`
  is rejected (blocks `javascript:` links).

## Consequences

- Zero setup: no install, no hosting, works offline.
- Data lives only in that one browser profile. It doesn't sync between devices
  or browsers, and clearing site data deletes it. There is no export/import yet.
- `localStorage` is scoped per origin. When opened as a `file://` URL, some
  browsers share or isolate that storage differently than a hosted page, so moving
  the file or later hosting it may appear to "lose" bookmarks (they are still in
  the old origin's storage).
- The page loads Google Fonts (Fredoka, Caveat) from `fonts.googleapis.com`. This is
  the only network request; it sends no bookmark data, and the app falls back to
  system fonts when offline.
