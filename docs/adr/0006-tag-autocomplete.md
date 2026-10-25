# 0006. Tag autocomplete from existing categories

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

Tags are free-form ([0004](0004-lowercase-category-tags.md)), so near-duplicates
like `recipe` and `recipes` creep in. While typing a category, the form should
offer the categories already used on other bookmarks: a single match can be
completed directly, several matches appear in a dropdown, and the options narrow
as the user keeps typing.

## Decision

- **Source:** every tag on any saved bookmark, de-duplicated and sorted
  alphabetically, minus the tags already added to the form.
- **Matching:** prefix match against the typed text after the same normalization
  used for saving tags (lower-case, leading `#` dropped, spaces → hyphens), so
  `Web D` matches `web-dev`. An empty box suggests nothing.
- **One match:** the rest of the tag appears as faded ghost text after the cursor,
  with a small `tab ⇥` hint. **Tab** or **→** (at the end of the text) accepts it.
  If the only match is exactly what's typed, nothing is shown.
- **Several matches:** a dropdown under the field lists them with a color swatch
  and the typed part in bold. **↑/↓** move the highlight (cycling through a
  "nothing highlighted" slot), **Enter** picks the highlighted option, **Tab**
  picks the highlighted or first option, a click picks any option, and **Esc** or
  leaving the field closes the dropdown.
- **Enter with nothing highlighted** keeps the existing behavior: the typed text
  is added as a new tag, even if longer suggestions exist. Accepting a suggestion
  always takes an explicit action (Tab, →, arrows + Enter, or a click), so creating
  a new tag that happens to be a prefix of an existing one (`art` vs `art-history`)
  is never blocked.
- The input is an ARIA combobox (`aria-expanded`, `aria-activedescendant`) and
  the dropdown a `listbox`, so screen readers announce the options.

## Consequences

- Reusing existing categories takes a keystroke or two, which keeps tags
  consistent.
- Tab is taken over while a suggestion is showing; with no suggestions, Tab moves
  focus as usual.
- Matching is prefix-only, so typing `dev` won't suggest `web-dev`.
- Suggestions are recomputed from all bookmarks on each keystroke, which is fine
  at personal-bookmark scale.
