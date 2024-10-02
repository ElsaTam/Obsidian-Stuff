> [!NOTE]
> For a callout syntax with no dataview code, see https://github.com/ElsaTam/Obsidian-Stuff/blob/main/snippets/callouts/README.md#cards-deck.

You can embed notes in a list with [dataviewjs](https://github.com/blacksmithgu/obsidian-dataview) and add to the containing element the class `dataview-cards-deck` in order to the snippet to take effect.

![cards-decks-embeds](screenshots/card-decks-embeds.gif)

````md
```dataviewjs
let result = await dv.query(`LIST FROM "Path/To/Folder" SORT file.name`)
let places = result.value.values
let embeds = places.map(p => "[" + dv.fileLink(p.path, true) + "](<" + p.path + ">)")
dv.el("p", embeds, { cls: "dataview-cards-deck dataview-wide" })
```
````

In case you have a lot of notes, you can add the class `dataview-wide` and the card decks will take the full length, without changing the readable line length of your editor.

```
      | Lorem ipsum dolor sit amet. Nam vel    |
      | sed felis egestas. Nulla lacinia in    |
      | nisl vel ornare.                       |
+----------------------------------------------------+
| .dataview-cards-deck.dataview-wide                 |
+----------------------------------------------------+
      | Interdum et malesuada fames ac ante.   |
      | Maecenas dictum tortor aliquet. Cras   |
      | eu facilisis justo. Quisque turpis     |
      | ipsum, finibus                         |
      | +------------------------------------+ |
      | | .dataview-cards-deck               | |
      | +------------------------------------+ |
      | In est risus, posuere porta luctus.    |
      | Aenean tincidunt porttitor varius. Sed |
      | nec lectus vel eros molestie.          |
```

You can also query images from the files properties and embed them inside a link. In the following example, links to images (`[[image.png]]`) are stored under the property `image`.

![cards-decks-images](screenshots/card-decks-images.webp)

````md
```dataviewjs
let result = await dv.query(`TABLE image FROM "Path/To/Folder" WHERE image`);
let places = result.value.values;
console.log(places);
let embeds = places.map(p => "[" + dv.fileLink(p[1].path, true) + "](<" + p[0].path + ">)");
dv.el("p", embeds, { cls: "dataview-cards-deck", attr: { id: "dataview-id-places" } });
```
````

**Note on performances**
The rendered cards are only meant to be note preview. In order to improve performance, the snippet uses `content` and `content-visibility` properties. The style of your note might therefore be affected if they are using any cascading properties, but I think this is a good trade-off for small note previews.

The translation and scaling are only done using `transform` properties, which are trigerring only composite operations.

In case the snippet is still slow, you can remove the rules found at the end of the code. They add style to the hovering effect (like color change, etc), but are not mandatory to keep the snippet working. So it is safe to remove, it wil decrease the number of layout/paint operations.

**Note on the code**
If you use a different dataviewjs code to embed your cards, the snippet might not work properly. I'm still beginner when it comes to dataviewjs, so maybe there are other ways to fetch all notes. If you use an other code and the snippet breaks, please contact me and provide the code. I will try to adapt the snippet to make it works for your code.

**Note for Metabind users**
Metabind makes everything reload everytime you will hover a card containing Metabind elements, and cause lags. The only workaround I have found so far is to disable the hover event that triggers the Metabind feature *after* the embed notes are loaded. For that, I added a second piece of code to add a button that can be clicked after the loading has been done.
````md
```dataviewjs
dv.el("button", "Stop popover", { attr: { onclick: `
let decks = document.querySelectorAll(".dataview-cards-deck");
for (deck of decks) {
    console.log(deck);
    let links = deck.querySelectorAll("a.internal-link");
    for (link of links) {
        link.addEventListener("mouseover", (e) => {
            e.stopImmediatePropagation();
        })
    }
}` }});
```
````