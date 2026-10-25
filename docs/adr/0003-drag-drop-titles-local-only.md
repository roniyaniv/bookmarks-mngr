# 0003. Drag-and-drop titles come from drag data only; no network fetch

- **Status:** Accepted
- **Date:** 2026-10-25

## Context

The form has a drop zone: dragging a link from the browser's address bar onto it
fills in the URL and the page title.

Browsers don't always include the title in the drag data:

- Firefox sends `text/x-moz-url` (`url\ntitle`), which always has it.
- Chrome and Safari may send `text/html` with an `<a>` whose text is the title,
  but sometimes only the URL (`text/uri-list` / `text/plain`).

When the title is missing, the only way to get it is to download the page. A page
can't read other sites directly (CORS), so that would mean going through a
third-party proxy such as `api.allorigins.win`. An earlier version did exactly that.

## Decision

Keep everything local. The title is read only from the drag data, never by
fetching the page. When the drag data has no usable title (missing, or the same as
the URL), the title field gets the site's hostname (e.g. `github.com`), which the
user can edit before saving.

The proxy fallback was removed.

## Consequences

- No bookmark URL ever leaves the user's machine. This matters for private links
  such as internal tools or URLs carrying tokens.
- No dependency on a free third-party service's uptime, rate limits, or
  continued existence.
- In Chrome and Safari, some drops will get the hostname instead of the real page
  title, and the user has to type it in.
- Drops that miss the drop zone are swallowed (`dragover`/`drop` are cancelled on
  `window`) so the browser doesn't navigate away from the app.
