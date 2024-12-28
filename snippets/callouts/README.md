You will find in this folder several snippets to add new stylized callouts to your notes.

# Usage

Choose the snippets you want and include them in your `.obsidian/snippets/` folder. Since all of those callouts are now part of the theme, the documentation has moved to the theme documentation: https://elsatam.github.io/obsidian-fancy-a-story/docs/callouts/callouts.html
You can see all available callouts in the following gallery: https://elsatam.github.io/obsidian-fancy-a-story/docs/callouts/callouts-gallery.html (wait a few seconds for it to load properly). The result without the theme might be a little bit different but should work just fine.

# Assets

For styling effects, several of these snippets use images and specific fonts. As much as I can, I tried to embed them directly when I found free to use assets. You can find any source and licenses in the folder [assets license/](assets%20license/).

If any, the assets are always embed at the bottom of any css file. Sometimes, callouts come in different variants to pick one image among several. When this is the case, snippets can become heavy because they embed several images (see the `clue` callout for example). I tried as much as I could to reduce them using webp format, but still it can slow down the styling. Feel free to choose only one and remove the others if you know you won't use them.

# Compatibility issues

Some theme applies custom style to callouts. I can't test them all. So if you see any breaking when using a snippet, please open an issue or contact me on Discord and I will make the needed changes.

An other possible conflict is if other themes are using the same keywords than my snippets. Then things will get messy. You can change the used name in the css snippet. They are all using nested CSS code, so it should be easy to find the keyword to change, look for anything like `.callout[data-callout="keyword"]`.