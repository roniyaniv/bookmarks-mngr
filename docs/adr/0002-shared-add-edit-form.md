# 0002. One form for adding and editing bookmarks

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

Each bookmark needs edit and delete actions. Editing could happen inline in the
list, in a modal, or in the existing add form. The requirement is that editing
uses the same form as adding.

## Decision

- Clicking **Edit** loads the bookmark into the add form and puts the form into
  edit mode: the heading and submit button text change, the form turns mint
  green, a **Never mind** button appears, and the bookmark being edited is
  highlighted in the list.
- A single `editingId` variable tracks the mode. `null` means add; a set ID means
  submit updates that bookmark in place (keeping its position and `id`).
- Submit, cancel, and deleting the bookmark being edited all return the form to
  add mode.
- **Delete** asks for confirmation with `confirm()`.

## Consequences

- One set of inputs and one validation path for both actions.
- Only one bookmark can be edited at a time.
- The page scrolls up to the form on edit, which moves the user away from the
  list; the highlight helps them find the item afterwards.
