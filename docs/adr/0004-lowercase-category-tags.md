# 0004. Categories are free-form lower-case tags

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

Bookmarks need categories. They could be a single category from a fixed list or
any number of free-form tags. The requirement is lower-case tags, shown as pills
in the form and next to each bookmark in the list.

## Decision

- Each bookmark has a `tags` array of strings; a bookmark can have zero or more.
- Tags are normalized on entry: trimmed, a leading `#` removed, lower-cased, and
  runs of whitespace replaced with a single hyphen (`"Dev Tools"` → `dev-tools`).
  Duplicates on the same bookmark are ignored.
- In the form, Enter or a comma turns the typed text into a pill; pasting
  `a, b, c` adds all three. Backspace in the empty box removes the last pill, and
  each pill has a × button. Enter in an empty tag box submits the form, and text
  still in the box when the form is submitted is kept as a tag.
- A pill's color is picked from a fixed pastel palette by hashing the tag name, so
  the same tag is always the same color.
- Bookmarks saved before this change have no `tags` field and are treated as
  having none; no migration is needed.

## Consequences

- No list of categories to maintain; users invent tags as they go.
- Near-duplicates (`recipe` vs `recipes`) are possible because tags are free-form.
- There's no filtering yet. Tags can be used to group the list
  ([0005](0005-group-by-category-view.md)), and existing tags are offered as
  autocomplete suggestions ([0006](0006-tag-autocomplete.md)).
