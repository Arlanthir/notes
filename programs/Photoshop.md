# Photoshop

## Pixels Per Inch (PPI)
- Digital images should have 72 pixels per inch
- Print images should have 300 pixels per inch

## Shortcuts

| Action                           | Shortcut                                                           |
|----------------------------------|--------------------------------------------------------------------|
| Preferences                      | `CTRL + K`                                                         |
| Save for Web                     | `CTRL + ALT + SHIFT + S`                                           |
| Select Layer                     | Usar a MoveTool (seta preta) `CTRL` + click na área visível        |
| Show only a layer/group/mask     | `ALT` + click no olho, fazer o mesmo para voltar atrás             |
| Copy Merged (todas as layers)    | `SHIFT + CTRL + C`                                                 |
| Revert File                      | `F12`                                                              |
| Copy layer group while dragging  | `ALT` + click and drag                                             |
| Clear Selection                  | `CTRL + D`                                                         |
| Select Inverse                   | `ALT` + select                                                     |
| Select Pixels                    | `CTRL + click thumbnail                                            |
| Toggle Mask                      | `SHIFT` + click thumbnail                                          |
| Edit Mask                        | `ALT` + click thumbnail                                            |
| Toggle Guides                    | `CTRL + H`                                                         |
| Toggle Grid                      | `CTRL + '`                                                         |
| Edit Grid spacing                | `Preferences > Guides, Grids, Slices`                              |
| Toggle all tools                 | `TAB`                                                              |

| Tool             | Shortcut |
|------------------|----------|
| Move             | V        |
| Select           | M        |
| Pen              | P        |
| Type             | T        |

## Convert multiply / screen blending mode to alpha PNG
1. Select > Color Range
2. Pick white color for multiply OR black for screen
3. Increase fuzziness as required
4. OK
5. Layer > Layer Mask > Hide Selection
6. Change blending mode to normal
7. Save (for web) as PNG

## Move layer effects from a layer to a separate one
`Right Click on the effect > Create Layer`  
The layer will be applied to the original one, you can mask it and then unapply it by `ALT + left click`ing on the edge between the applied layer and the effect.
