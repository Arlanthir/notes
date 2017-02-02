# Webpack

```bash
npm install webpack --save-dev
```

## Npm scripts to run local webpack

```javascript
"scripts": {
    "build": "webpack"
}
```

```bash
npm run build
```

## Concepts

### Entry
The starting point of the dependency graph of your application.

Single entry:
```javascript
entry: './src/index.js'
```

Multiple entry:
```javascript
entry: {
    app: './src/app.js',
    vendor: './src/vendor.js'
}
```

### Output
How to treat the bundled code.

Single entry:
```javascript
output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
}
```

Multiple entry:
```javascript
output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'dist')
}
```

### Loaders
By default, Webpack only knows how to load JavaScript. Third-party loaders tell Webpack how to load other files, such as ES2015, Sass, etc.

### Plugins
Actions performed on multiple files / bundled modules instead of a single file type (e.g.: minification).


## Configuration

### Simple example configuration

webpack.config.js
```javascript
var path = require('path');

module.exports = {
    // Support import statements and concats imported JS files
    entry: './app/app.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
}
```
