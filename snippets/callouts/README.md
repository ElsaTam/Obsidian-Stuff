You will find in this folder several snippets to add new stylized callouts to your notes.

# Usage

Choose the snippets you want and include them in your `.obsidian/snippets/` folder.

# Assets

For styling effects, several of these snippets use images and specific fonts. As much as I can, I tried to embed them directly when I found free to use assets. You can find any source and licenses in the folder [assets license/](assets%20license/).

If any, the assets are always embed at the bottom of any css file. Sometimes, callouts come in different variants to pick one image among several. When this is the case, snippets can become heavy because they embed several images (see the `clue` callout for example). I tried as much as I could to reduce them using webp format, but still it can slow down the styling. Feel free to choose only one and remove the others if you know you won't use them.

# Compatibility issues

Some theme applies custom style to callouts. I can't test them all. So if you see any breaking when using a snippet, please open an issue or contact me on Discord and I will make the needed changes.

An other possible conflict is if other themes are using the same keywords than my snippets. Then things will get messy. You can change the used name in the css snippet. They are all using nested CSS code, so it should be easy to find the keyword to change, look for anything like `.callout[data-callout="keyword"]`.

# List of new callouts

## Clue

**FILE**: [callout-clue.css](callout-clue.css)

Add a small clue to your notes, and awake the detective that sleeps in you.

The snippet comes in several variations, 4 different tapes image (a, b, c, d), and 4 different paper images (a, b, c, d).  You can mix variants together, like `tape-b` and `paper-d`.

Versions A are used by default for both tape and paper, in case you don't specify anything. You can change that in the snippet, search for the comment `/* default */` and change value here.

When folded, only the tape is visible.

> [!NOTE]
> This snippet doesn't change between light and dark theme.

### Tape A, Paper A (default)

![clue-a](screenshots/clue-a.png)

```md
> [!clue|tape-a paper-a]+ Clue
> The red-bearded man was seen at 2am at the docks.
```

or

```md
> [!clue]+ Clue
> The red-bearded man was seen at 2am at the docks.
```

### Tape B, Paper B

![clue-b](screenshots/clue-b.png)

```md
> [!clue|tape-b paper-b]+ Clue
> The red-bearded man was seen at 2am at the docks.
```

### Tape C, Paper C

![clue-c](screenshots/clue-c.png)

```md
> [!clue|tape-c paper-c]+ Clue
> The red-bearded man was seen at 2am at the docks.
```

### Tape D, Paper D

![clue-d](screenshots/clue-d.png)

```md
> [!clue|tape-d paper-d]+ Clue
> The red-bearded man was seen at 2am at the docks.
```

## Letter

Write elegant handwritten letters directly into your vault.

I deleted vertical spacing between paragraph to have equal line heigths. If you wish to change that, just change the variable `--p-spacing` in the snippet. The background lines will adapt, as they are computed per paragraph and not on the entire callout.

![letter](screenshots/letter.png)

```md
> [!letter]+ Johnny Cash’s love letter to June Carter
> June 23 1994
>
> Odense, Denmark.
>
> Happy Birthday Princess,
>
> We get old and get used to each other. We think alike. We read each others minds. We know what the other wants without asking. Sometimes we irritate each other a little bit. Maybe sometimes take each other for granted.
>
> But once in awhile, like today, I meditate on it and realize how lucky I am to share my life with the greatest woman I ever met. You still fascinate and inspire me. You influence me for the better. You’re the object of my desire, the #1 Earthly reason for my existence. I love you very much.
>
> Happy Birthday Princess.
>
> John
```