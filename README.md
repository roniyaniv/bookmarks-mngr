# 🐿️ Bookmark Burrow

A playful, single-page bookmark manager. Everything runs in your browser and is
stored in `localStorage`: no server, no account, no build step, and your links
never leave your machine.

## Getting started

Open `index.html` in a browser:

```sh
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

That's it. There are no dependencies to install.

## Features

- **Add, edit, delete** bookmarks with a title and URL. Editing reuses the add
  form; deleting asks for confirmation first. URLs without a scheme get
  `https://`, and only `http`/`https` links are accepted.
- **Drag and drop** a link from your browser's address bar onto the drop zone to
  fill in the URL and title. If the browser doesn't include the page title in the
  drag (Chrome and Safari sometimes don't), the site's hostname is used instead;
  the page is never fetched.
- **Categories** as lower-case tags, shown as colored pills. `Dev Tools` becomes
  `dev-tools`, and each tag keeps the same color everywhere.
- **Tag autocomplete** from categories you've already used.
- **Group by category**: a bookmark with several tags appears under each of them;
  untagged bookmarks go under *no category*.
- **Live filter** by title, URL, or tag, with matches highlighted.

## Keyboard shortcuts

| Where           | Key                     | Action                                        |
|-----------------|-------------------------|-----------------------------------------------|
| Anywhere        | `/`                     | Jump to the filter box                        |
| Filter box      | `Esc`                   | Clear the filter                              |
| Categories box  | `Enter` or `,`          | Add the typed text as a tag                   |
| Categories box  | `Backspace` (when empty)| Remove the last tag                           |
| Categories box  | `Tab` or `→`            | Accept the suggested tag                      |
| Categories box  | `↑` / `↓`, then `Enter` | Pick a suggestion from the dropdown           |
| Categories box  | `Esc`                   | Close the suggestions                         |

Multiple words in the filter must all match, but each can match a different
field: `fun youtube` finds a bookmark tagged `fun` whose URL is on YouTube.

## Your data

Bookmarks are saved to `localStorage` under the key `bookmark-burrow` on every
change, and the *Group by category* setting under
`bookmark-burrow:group-by-category`. That means:

- They persist across reloads and browser restarts.
- They live only in the browser and profile you used. Nothing syncs between
  devices or browsers.
- Private/incognito windows, or clearing site data, will delete them.
- Moving or renaming `index.html` can make the bookmarks seem to disappear,
  especially in Firefox, because storage is tied to the file's location.
- There's no export/import yet, so `localStorage` is the only copy.

To inspect the raw data, run this in the browser's developer console:

```js
JSON.parse(localStorage.getItem("bookmark-burrow"))
```

The only network request the page makes is for its fonts (Fredoka and Caveat
from Google Fonts). Without a connection it falls back to system fonts.

## Project structure

```
index.html     The whole app: markup, styles, and script
docs/adr/      Architecture Decision Records
package.json   npm metadata (no dependencies or scripts yet)
```

## Design decisions

Significant decisions are recorded as ADRs in [`docs/adr/`](docs/adr/README.md):

1. [Single-file, client-only app with localStorage](docs/adr/0001-single-file-client-only-app.md)
2. [One form for adding and editing bookmarks](docs/adr/0002-shared-add-edit-form.md)
3. [Drag-and-drop titles come from drag data only; no network fetch](docs/adr/0003-drag-drop-titles-local-only.md)
4. [Categories are free-form lower-case tags](docs/adr/0004-lowercase-category-tags.md)
5. [Optional group-by-category view, with bookmarks repeated per tag](docs/adr/0005-group-by-category-view.md)
6. [Tag autocomplete from existing categories](docs/adr/0006-tag-autocomplete.md)
7. [Live text filter across title, URL, and tags](docs/adr/0007-live-text-filter.md)

When you make a new significant decision, add a record there.
