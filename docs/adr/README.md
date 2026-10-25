# Architecture Decision Records

Short records of the significant decisions made for Bookmark Burrow, in the
[Michael Nygard format](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).

| #    | Decision                                                                 | Status   |
|------|--------------------------------------------------------------------------|----------|
| 0001 | [Single-file, client-only app with localStorage](0001-single-file-client-only-app.md) | Accepted |
| 0002 | [One form for adding and editing bookmarks](0002-shared-add-edit-form.md) | Accepted |
| 0003 | [Drag-and-drop titles come from drag data only; no network fetch](0003-drag-drop-titles-local-only.md) | Accepted |
| 0004 | [Categories are free-form lower-case tags](0004-lowercase-category-tags.md) | Accepted |
| 0005 | [Optional group-by-category view, with bookmarks repeated per tag](0005-group-by-category-view.md) | Accepted |
| 0006 | [Tag autocomplete from existing categories](0006-tag-autocomplete.md) | Accepted |

To add a record, copy the structure of an existing one, use the next number,
and add a row above. Superseded records stay in place with their status updated.
