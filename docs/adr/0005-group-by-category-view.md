# 0005. Optional group-by-category view, with bookmarks repeated per tag

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

Bookmarks can have several tags ([0004](0004-lowercase-category-tags.md)). Users
want an optional view that groups the list by category, switched on with a
"Group by category" checkbox. A bookmark with several tags has to go somewhere:
in one "primary" group, in a combined group, or in every group it belongs to.
The requirement is every group.

## Decision

- A **Group by category** checkbox in the list header switches between the flat
  list (default) and the grouped view.
- In the grouped view, a bookmark appears once in each of its tags' groups.
  Groups are sorted alphabetically; bookmarks with no tags go in a final
  **no category** group. Within a group, bookmarks keep the flat list's order
  (newest first). Each group header shows the tag pill and how many links it has.
- The bookmark count in the header stays the number of unique bookmarks, not the
  number of rows shown.
- Edit and delete act on the bookmark, not the row: editing highlights every
  copy, and deleting animates every copy away before removing it.
- The checkbox state is remembered in `localStorage` under its own key,
  `bookmark-burrow:group-by-category`, separate from the bookmark data. It's a
  per-browser convenience: if storage is unavailable, the page falls back to the
  flat list.

## Consequences

- Every bookmark is easy to find under any of its tags.
- Bookmarks with many tags make the grouped view longer, and the same bookmark
  can be on screen several times.
- Grouping is computed on every render. That's fine for a personal bookmark list,
  but would need revisiting for thousands of bookmarks.
