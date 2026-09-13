# Building Tools

WorldEdit for people who have never typed `//set`.

## When it appears

The **Building Tools** button sits beside Close in the house menu, and only when **both** are true:

1. WorldEdit, FastAsyncWorldEdit or AsyncWorldEdit is installed
2. The player holds WorldEdit's own `worldedit.region.set`

A button for a plugin that is not there does nothing, and one for a player without the permission is an offer the server will refuse. The permission is WorldEdit's rather than a new one of ours, so a server that has already decided who may use `//set` has decided who may use this.

Nothing here touches WorldEdit's API: every button runs the command a builder would have typed, **as the player**. That is what makes it work on WorldEdit and FAWE alike, and what leaves WorldEdit deciding whether the player may do it.

## The Pattern

The top row of the menu is what will be placed: nine squares drawing the mix in its own proportions, not a line of text describing it. Clicking any of them opens the pattern editor.

```
. . . . i . . . .     what this is
. - - - - - - - .     less of that block
. B B B B B B B .     the block; click to take it out
. + + + + + + + .     more of that block
M M M M M M M M M     the mix, in the proportions it is placed in
. R A E B . X . .     start over, add, even out, back, close
```

A column per block, up to seven. The minus above and the plus below change that block's share; the **percentages** are what you see, worked out so three even blocks read 34/33/33 rather than a total that quietly is not 100.

The command it builds is ordinary WorldEdit syntax, printed on the item so somebody who wants to type it later can read it off the menu:

| Mix | Command |
|-----|---------|
| One block | `//set stone` |
| Even shares | `//set stone,glass,tinted_glass` |
| Anything else | `//set 60%stone,40%cobblestone` |

The last block cannot be removed, because an empty pattern is a `//set` with no argument, which is the error the whole menu exists to avoid.

## Operations

| Row | Buttons |
|-----|---------|
| **Selecting** | Wand, Corner 1 Here, Corner 2 Here, Reach Up and Down, What Is Selected, Forget Selection, Undo |
| **Filling** | Fill Selection, Fill the Gaps, Walls, Box, Hollow It Out, Lay On Top, Redo |
| **Shapes** | Sphere, Cylinder, Pyramid, Size, Copy, Paste, Repeat |
| **Moving** | Nudge, Turn a Quarter, Mirror, Smooth, Cut, Drain Water, Make It Look Natural |

**Size** cycles a short list (3, 5, 8, 12, 20, 32) rather than asking for a number, and is what the sphere, the cylinder, the pyramid and the drain use.

## Picking a Block

Nearly a thousand materials is twenty pages, so the picker has a search: paging to find one is not finding it. Blocks already in the mix are marked.
