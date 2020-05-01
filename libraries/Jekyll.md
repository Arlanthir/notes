# Jekyll

## Update gems

```bash
gem update --system
gem update
bundle update
```

### Rebuild native extensions

```bash
gem pristine --all
```

## Debug

```html
{{ page | jsonify }}
```
