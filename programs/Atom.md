# Atom

`Ctrl Shift P` - Command Palette

## Configuration

If needed, use the app menu to add Shell Binaries

config.cson:

```cson
"*":
  "exception-reporting":
    userId: "77b6584e-6673-8fb5-59f5-cccfb3b1d466"
  welcome:
    showOnStartup: false
  core:
    excludeVcsIgnoredPaths: false
  editor:
    invisibles:
      cr: " "
      eol: " "
    scrollPastEnd: true
    showInvisibles: true
    tabLength: 4
    softTabs: false
    fontSize: 12
  docblockr: {}
  "autocomplete-plus":
    confirmCompletion: "enter"
```

keymap.cson:

```cson
'atom-text-editor:not([mini])':
  'ctrl-h': 'find-and-replace:select-next'
  'ctrl-i': 'editor:auto-indent'
  'ctrl-d': 'editor:delete-line'
  'ctrl-&': 'editor:toggle-line-comments'
  'ctrl-7': 'editor:toggle-line-comments'
  'ctrl-shift-up': 'editor:add-selection-above'
  'ctrl-shift-down': 'editor:add-selection-below'
```

osx keymap.cson:

```cson
'atom-text-editor:not([mini])':
    'cmd-h': 'find-and-replace:select-next'
    'cmd-d': 'editor:delete-line'
    'cmd-/': 'editor:toggle-line-comments'
    'cmd-7': 'editor:toggle-line-comments'
```

## Windows

### Open files with Atom by default
.exe is located in `C:\Users\<User>\AppData\Local\atom\`

## Packages
Packages available in https://atom.io/packages
Can be installed using the GUI or by running
`apm install <package-name>`

```
file-icons
#project-manager
```
