# Atom

`Ctrl Shift P` - Command Palette

## Configuration

If needed, use the app menu to add Shell Binaries

config.cson:

```cson
"*":
  core:
    projectHome: "/home/arlanthir/dev"
    telemetryConsent: "limited"
  editor:
    fontSize: 12
    invisibles:
      cr: " "
      eol: " "
    scrollPastEnd: true
    showInvisibles: true
    tabLength: 4
  "exception-reporting":
    userId: "eb62e58d-81ef-e59d-743b-9298cdb32e1f"
  "lint-sass-vue":
    noConfigDisable: true
  "linter-eslint":
    lintHtmlFiles: true
  "linter-sass-lint":
    noConfigDisable: true
  "linter-tslint":
    enableSemanticRules: true
  "linter-ui-default":
    panelHeight: 69
  "project-manager":
    alwaysOpenInSameWindow: true
  welcome:
    showOnStartup: false
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
atom-ternjs         # Autocomplete for JS, Packages > Atom Ternjs > Configure project
atom-typescript
docblockr
file-icons
language-vue
linter-eslint
linter-flake8
linter-sass-lint
lint-sass-vue
pigments
project-manager
```
