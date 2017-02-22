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
    // Supports import statements and concats imported JS files
    entry: './src/main.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    // Adds sass capabilities to import, allowing:
    // import './styles.scss';
    module: {
        rules: [{
            test: /\.scss$/,
            use: [
                'style-loader', // Creates style nodes from JS strings
                'css-loader',   // Translates CSS into CommonJS
                'sass-loader'   // Compiles Sass to CSS
            ]
        }]
    }
}
```

### More complex configuration for production code
```javascript
var path = require('path');
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
    // Supports import statements and concats imported JS files
    entry: './src/main.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    // Convert styles.scss to styles.css, allowing
    // <link rel="stylesheet" href="dist/styles.css">
    // Still needs the import in JS code, and breaks Hot Module Reload (HMR)
    module: {
        rules: [{
            test: /\.scss$/,
            use: ExtractTextPlugin.extract({
                fallback: 'style-loader',
                use: ['css-loader', 'sass-loader']
            })
        }]
    },
    plugins: [
        new ExtractTextPlugin('styles.css'),
    ]
}
```

### Livereload

Webpack supports Hot Module Reload with a dedicated server.

To add Livereload to your own server instead, use `webpack-livereload-plugin`.

```javascript
const LiveReloadPlugin = require('webpack-livereload-plugin');
// ...
plugins: [
    new LiveReloadPlugin({
        appendScriptTag: true
    })
]
```

package.json:
```javascript
"scripts": {
    "build": "webpack",
    "watch": "webpack --watch"
}
```

To run:
```bash
npm run watch
```
