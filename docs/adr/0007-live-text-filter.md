# 0007. Live text filter across title, URL, and tags

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

As the stash grows, users need to find bookmarks quickly. The requirement is a
filter box that matches any field (title, URL, tag) and updates the list on
every keystroke.

## Decision

- A filter box sits under the list header. The list re-renders on every `input`
  event; there's no submit button and no debounce.
- **Matching:** the query is lower-cased and split on whitespace into words. A
  bookmark is shown when **every** word appears as a substring of its title, its
  URL, or any of its tags. Different words may match different fields, so
  `fun youtube` finds a bookmark tagged `fun` whose URL is on YouTube. An empty
  query shows everything.
- **Highlighting:** matched text is wrapped in `<mark>` in titles, URLs, and tag
  pills, built with DOM nodes (never `innerHTML`) so bookmark data and the query
  can't inject markup. Text that changes length when lower-cased (rare Unicode
  cases) is shown unhighlighted rather than mis-marked.
- **Count:** while a filter is active, the header count shows `shown / total`.
  If nothing matches, the list shows a "Nothing in the burrow matches …" message.
- **Grouping** ([0005](0005-group-by-category-view.md)) applies to the filtered
  result: only matching bookmarks appear, still under each of their tags.
- **Keyboard:** `/` focuses the filter from anywhere except another text field;
  **Esc** clears it. The browser's built-in clear button for search fields also
  works.
- The filter text isn't saved; reloading the page clears it.

## Consequences

- Finding a bookmark takes a few keystrokes, with no mode or syntax to learn.
- Substring matching is broad: `https` matches every bookmark, and the URL field
  includes the scheme and path.
- A bookmark added or edited while a filter is active disappears from view if it
  doesn't match; the `shown / total` count is the hint that a filter is on.
- Filtering runs over all bookmarks on each keystroke, which is fine at
  personal-bookmark scale.
