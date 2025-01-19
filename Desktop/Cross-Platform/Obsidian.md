Obsidian is my primary notetaking application. It is **proprietary**, however it is free to use, secure, extendible, high-quality, and cross-platform. It is local-first (as opposed to online-first) and based on Markdown files.

## Plugins I use
- Auto Link Title
- Dataview
- Excalidraw
- Git
- Importer
- Kindle Highlights

## Useful Dataview Queries

### Recently modified notes

Tip: Replace `file.mtime` with `file.ctime` for recently *created* notes.
 
```
TABLE WITHOUT ID
file.folder as Path,
file.link as Name,
file.etags as Tags,
file.mtime as Date,
FROM ""
SORT file.mtime DESC
LIMIT 50
```

### List all outlinks

Useful for a "further reading" section.

```
LIST
FROM outgoing([[]])
```

### Sort by number of connections

This sorts by the amount of inlinks (pages linking to this page) and outlinks (pages linking from this page).

Replace `"notes"` with your notes folder (or `""` for all files).
Replace `!"notes/folder-to-ignore"` with a folder you want to exclude from the results.

```
TABLE WITHOUT ID
file.folder as Path,
file.link as Name,
file.etags as Tag,
file.mtime as Date,
length(file.inlinks) + length(file.outlinks) as Links
FROM "notes" and !"notes/folder-to-ignore"
WHERE (length(file.inlinks) + length(file.outlinks)) > 4
SORT (length(file.inlinks) + length(file.outlinks)) DESC
```

### Second order links

Put this in a `dataviewjs` code block rather than `dataview` (see more in the [docs](https://blacksmithgu.github.io/obsidian-dataview/queries/dql-js-inline/#dataview-js)).

```js
const current = dv.current();
const inlinks = current.file.inlinks;
for (const item of Array.from(inlinks)) {
  dv.el('h2', item)
  const itemData = dv.page(item.path);
  dv.paragraph((fullItem.file.frontmatter.related ?? fullItem.file.outlinks ?? []).join(', '))
}
```

### JavaScript tables

No idea what this is useful for yet, but in `dataviewjs` blocks you can make tables with arbitrary JavaScript:

```js
dv.table(['userId', 'id', 'title'], [
    ['1a', '1b', '1c'], 
    ['2a', '2b', '2c']
  ]);
```